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

# fs.access
Used to check if a user have permissions for the given file or path.
1. fs.constants.F_OK - to check if the path is visible to the calling process,
2. fs.constants.R_OK - to check if the path can be read by the process,
3. fs.constants.W_OK - to check if the path can be written by the process,
4. fs.constants.X_OK - to check if the path can be executed by the process.

Do not use fs.access to check for accessibility before calling fs.open, readFile, writeFile, etc, another process might change the file before you check and do the actual operation => Race condition.

Useful fs-like modules:
1. graceful-fs
2. mock-fs: test against some mock files and folders
3. lockfile: restrict access to a file, allowing only one process to access at a time