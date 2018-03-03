# HTTP/2
The primary goals for HTTP/2 are to reduce latency by enabling full request and response multiplexing, minimize protocol overhead via efficient compression of HTTP header fields, and add support for request prioritization and server push.

# Server Push
HTTP/2 Server Push allows the server to send assets to the browser before it has even asked for them.

In HTTP/1, client requests for index.html. The browser process initial HTML, resolve links and makes separate request to fetch them. Check the initiator in the networks tab. It will be the parser. User has to wait for the browser to parse html, discover links and fetch it - increasing load time and rendering time.

Using http/2 push
```
const http2 = require('http2')
const server = http2.createSecureServer(
  { cert, key },
  onRequest
)

function push (stream, filePath) {
  const { file, headers } = getFile(filePath)
  const pushHeaders = { [HTTP2_HEADER_PATH]: filePath }

  stream.pushStream(pushHeaders, (pushStream) => {
    pushStream.respondWithFD(file, headers)
  })
}

function onRequest (req, res) {
  // Push files with index.html
  if (reqPath === '/index.html') {
    push(res.stream, 'bundle1.js')
    push(res.stream, 'bundle2.js')
  }

  // Serve file
  res.stream.respondWithFD(file.fileDescriptor, file.headers)
}
```