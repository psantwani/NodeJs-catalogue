# Use HTTP methods
API routes should always use nouns as resource identifiers.

# Use HTTP status codes correctly
```2xx```, if everything is OK
```3xx```, if resource was moved,
```4xx```, if the request cannot be fullfilled because of a client error (like requesting a resource that does not exist)
```5xx``, if something went wrong on the API side (like an exception)

# Use HTTP headers to send metadata
Information like pagination, rate-limiting or authentication. If you are using a custom HTTP header, its best practice to prefix it with X.
Note: Node.js imposes a 80kb size limit on the headers object.

# Pick the right framework for your Node.js REST API
Restify comes with automatic (DTrace)[http://dtrace.org/blogs/about/] support for all your handlers.

# Black box test your Node.js REST APIs
Black-box testing is a method of testing where the functionality of an application is examined without the knowledge of its internal structures or workings. Run your black-box test scenarios on a known subset of production data, populate the database with crafted data before the test cases are run.
```
const request = require('supertest')

describe('GET /user/:id', function() {
  it('returns a user', function() {
    // newer mocha versions accepts promises as well
    return request(app)
      .get('/user')
      .set('Accept', 'application/json')
      .expect(200, {
        id: '1',
        name: 'John Math'
      }, done)
  })
})
```

# Do JWT-Based, Stateless Authentication
Your auth layer must be stateless too. For this, JSON Web Token is ideal.
It consists of 3 parts - Header (containing type of token and hashing algo), Payload (containing the claims), Signature (JWT doesnt encrypt the payload, it just signs it).
```
const koa = require('koa')
const jwt = require('koa-jwt')

const app = koa()

app.use(jwt({ 
  secret: 'very-secret' 
}))

// Protected middleware
app.use(function *(){
  // content of the token will be available on this.state.user
  this.body = {
    secret: '42'
  }
})
```

To access the protected endpoints, you have to provide the token in the Authorization header field.
```
curl --header "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ" my-website.com 
```

# Use conditional requests
They are HTTP requests that are executed differently based on different HTTP headers.
Eg. Last-Modified -  Indicates when the resource was last modified.
Etag - to indiciate entity tag.
If-Modified-Since - Used with Last-Modified.
If-None-Match - Used with Etag header.

Scenario: Client requests for the ```doc``` resource for the first time, so neither If-Modified-Since and If-None-Match headers are applied. Hence server responds with 200 OK with Last-Modified and E-tag in header.
The client can now set If-Modified-Since and If-None-Match headers once it tries to request ```doc``` again. In this case, the server will simply respond with ```304-Not Modified``` status and wont send the resource again.

# Rate limiting
To tell API users how many requests they have left, set the following headers
```X-Rate-Limit-Limit``` : No. of requests allowed in given time interval
```X-Rate-Limit-Remaining``` : No. of requests remaining in given time interval
```X-Rate-Limit-Reset``` : The time when the rate limit will be reset.

# API documentation
[API blueprint](https://apiblueprint.org/)
[Swagger](http://swagger.io/)

# Understanding and Measuring HTTP timings

SSL/TLS: TLS is a cryptographic protocol that provides communications security over a computer network. SSL (Secure Sockets Layer) is a deprecated predecessor to TLS.

Timeline of a usual HTTP request-
Sum of the following - 
1. DNS Lookup : Every new domain requires a full round trip to do a DNS lookup.
2. TCP Connection: Time to establish TCP between source and destination host. A proper multi-step handshake process.
3. TLS handshake: During this time, the handshake process endpoints exchange authentication and keys to establish or resume secure sessions. There is no TLS handshake in a not HTTPS request
4. Time to First Byte (TTFB): Time spent waiting for initial response. Includes the latency of a round trip to the server + time spent waiting for the server to process the request and deliver the reponse.
5. Content transfer: Time spent receiving response data.

More time taken by 
1. DNS Lookup means issue with the DNS provider or with DNS cache settings
2. TTFB means you should check latency between 2 endpoints, also the current payload on the server
3. Content Transfer means ineffecient body response or slow n/w connections.

```
 const timings = {
    // use process.hrtime() as it's not a subject of clock drift
    startAt: process.hrtime(),
    dnsLookupAt: undefined,
    tcpConnectionAt: undefined,
    tlsHandshakeAt: undefined,
    firstByteAt: undefined,
    endAt: undefined
  }

  const req = http.request({ ... }, (res) => {
    res.once('readable', () => {
      timings.firstByteAt = process.hrtime()
    })
    res.on('data', (chunk) => { responseBody += chunk })
    res.on('end', () => {
      timings.endAt = process.hrtime()
    })
  })
  req.on('socket', (socket) => {
    socket.on('lookup', () => {
      timings.dnsLookupAt = process.hrtime()
    })
    socket.on('connect', () => {
      timings.tcpConnectionAt = process.hrtime()
    })
    socket.on('secureConnect', () => {
      timings.tlsHandshakeAt = process.hrtime()
    })
  }) 
  ```

  Use time parameter in request module
  ```
  request({
  uri: 'https://risingstack.com',
  method: 'GET',
  time: true
}, (err, resp) => {
  console.log(err || resp.timings)
})
```