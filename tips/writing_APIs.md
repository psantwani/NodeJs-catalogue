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