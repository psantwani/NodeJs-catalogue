# The fs module

All the methods have asynchronous and synchronous forms. You should always use the asynchronous API when developing production code, as it won't block the event loop so you can build performant applications. You should only use the synchronous API when building proof of concept applications, or small CLIs.

Streams are a first-class construct in Node.js for handling data. There are three main concepts to understand:
1. source - the object where your data comes from
2. pipeline - where your data passes through (you can filter, or modify it here)
3. sink - where your data ends up

Good reference: [Stream Handbook](https://github.com/substack/stream-handbook)

Copying a file using streams (copy is not a core fs feature)
```
const fs = require('fs')
const readableStream = fs.createReadStream('original.txt')
var writableStream = fs.createWriteStream('copy.txt')

readableStream.pipe(writableStream)
```

You could also transform the files using streams
```
const fs = require('fs')
const zlib = require('zlib')

fs.createReadStream('original.txt.gz')
  .pipe(zlib.createGunzip())
  .pipe(fs.createWriteStream('original.txt'))
```

- fs.access
Used to check if a user have permissions for the given file or path.
1. fs.constants.F_OK - to check if the path is visible to the calling process,
2. fs.constants.R_OK - to check if the path can be read by the process,
3. fs.constants.W_OK - to check if the path can be written by the process,
4. fs.constants.X_OK - to check if the path can be executed by the process.

Do not use fs.access to check for accessibility before calling fs.open, readFile, writeFile, etc, another process might change the file before you check and do the actual operation => Race condition.

- Useful fs-like modules:
1. graceful-fs
2. mock-fs: test against some mock files and folders
3. lockfile: restrict access to a file, allowing only one process to access at a time

# Process module
The process object (which is an instance of the EventEmitter) is a global variable that provides information on the currently running Node.js process.

- Events to watch out for
1. uncaughtException - This event is emitted when an uncaught JavaScript exception bubbles back to the event loop. By default, if no event listeners are added to the uncaughtException handler, the process will print the stack trace to stderr and exit
```
process.on('uncaughtException', (err) => {
  // here the 1 is a file descriptor for STDERR
  fs.writeSync(1, `Caught exception: ${err}\n`)
})
```
Important advice -
a. Recovering from uncaughtException is strongly discouraged, it is not safe to continue normal operation after it.
b. The handler should only be used for synchronous cleanup of allocated resources.

2. unhandledRejection
```
const fs = require('fs-extra')

fs.copy('/tmp/myfile', '/tmp/mynewfile')
  .then(() => console.log('success!'))
```
If the file does not exist, we will get a unhandledRejection error because we missed the error handling in our Promise.

Use [make-promise-safe](https://github.com/mcollina/make-promises-safe) to print the stack trace when such unhandledRejections occur.

3. Signal Events
Two important POSIX signal events - 
a. SIGTERM: Sent to process to request its termination. Unlike SIGKILL, it can listened to or ignored by the process. This facilitates graceful shutdown. 
- App gets SIGTERM event ==> App notifies load balancer that its not ready for a new request ==> app finish all ongoing requests ==> release all of its resources (like DB conn) ==> app exits with "success" status code (process.exit())

b. SIGUSR1
SIGUSR1 and SIGUSR2 are used for user-defined conditions. Node uses this in built-in debugger. 
Usage: 
```kill -USR1 PID_OF_THE_NODE_JS_PROCESS```
Once you did that, the Node.js process in question will let you know that the debugger is running:
```
Starting debugger agent.
Debugger listening on [::]:5858
```

# Process module methods
1. process.cwd() : current working directory of the node.js process.
2. process.env
3. process.exit([code])
4. process.kill(pid, [signal])

# Exit codes
1. "1": Uncaught fatal exception, not handled by uncaughtException
2. "5": Fatal error in v8 (like memory allocation failure)
3. "9": Invalid argument