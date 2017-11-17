> Swagger is a simple yet powerful representation of your RESTful API. With a Swagger-enabled API, you get interactive documentation, client SDK generation and discoverability.

1. Its an API description language. Its a contract between the backend and the frontend developer. If the document changes, it means the API has changed.
2. [swaggerize-hapi](https://github.com/krakenjs/swaggerize-hapi) module when using Hapi for node services. Check [JOI validation](https://github.com/hapijs/joi). We not only get plain routes, but the types are casted to the types in the description, and the payload is already validated.
3. Swagger not only works for defining communication between client and server - but it can work between servers as well(microservices).
4. You can get the Swagger UI from [Swagger-UI](https://github.com/swagger-api/swagger-ui). The builder automatically creates the /api-docs endpoint where the JSON description is available.
5. Setup using Docker 
```
docker build -t swagger-ui-builder .
docker run -p 127.0.0.1:8080:8080 swagger-ui-builder
```

# Swagger Descriptor 
This is design-driven development. We design our endpoint's behavior by describing them in YML or JSON file.
Swagger's boilerplate
```
swagger: '2.0'
info:
  title: SAMPLE API
  version: '0.0.1'
host: 0.0.0.0
schemes:
  - http
  - https
basePath: '/v1'
produces:
  - application/json
```

Specifying paths
```
paths:
  /info:
    get:
      tags:
      - info
      summary: returns basic info of the server
      responses:
        200:
          description: successful operation
        default:
            description: unexpected error
            schema:
            $ref: Error
```
What this snippet does is it creates an /info endpoint, that returns 200 OK if everything went well and an error if something bad happened. What is $ref - That is Swagger's way to stay DRY. You can define the API resources in your Swagger file. Write once, use anywhere.

# Using Swagger with Node.js

1. Use [enjoi](https://www.npmjs.com/package/enjoi): No more validation in your route handler.

Example -
```
definitions:
  User:
    type: object
    required:
    - username
    - password
    properties:
      id:
        type: string
      username:
        type: string
      password:
          type: string
```
When creating a server, just create a Hapi plugin for your API.
```
var Hapi = require('hapi'),
var swaggerize = require('swaggerize-hapi');

var server = new Hapi.Server();

server.register({
    plugin: swaggerize,
    options: {
        api: require('./config/pets.json'),
        handlers: Path.join(__dirname, './handlers')
    }
});
```

