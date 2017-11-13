# Sample App

```
var hapi = require('hapi');

// creating the hapi server instance
var server = new hapi.Server();

// adding a new connection that can be listened on
server.connection({
  port: 3000,
  host: 'localhost',
  labels: ['web']
});

// defining our routes
server.route({
  method: 'GET',
  path: '/',
  handler: function (request, reply) {
    reply('Hello world!');
  }
});

// starting the server
server.start(function (err) {
  if (err) {
    throw err;
  }
  
  console.log('hapi server started');
});
```

# Request Lifecycle
1. Client sends a request.
2. ```onRequest``` extension point: Lookup route using request path; parse cookies.
3. ```onPreAuth```: Authenticate request; read and parse payload; authenticate request payload.
4. ```onPostAuth```: Validate path params; validate query and params.
5. ```onPreHandler```: Route prequisities; Handler(reply function).
6. ```onPostHandler```: Validate response payload.
7. ```onPreResponse```: Send Response. Emits response event.

We can modify each request at the extension points, using ```server.ext()```.

# Plugins
1. Creating plugins.
```
var myPlugin = {
  register: function (server, options, next) {
    server.route({
      method: 'GET',
      path: '/',
      handler: function (request, reply) {
        reply('Hello world!');
      }
    });
    next();
  }
}

myPlugin.register.attributes = {
  name: 'myPlugin',
  version: '1.0.0'
};

module.exports = myPlugin;
```
2. Loading plugins.
```
server.register({
    register: require('myplugin'),
    options: {
        message: 'hello'
    }
}, function (err) {
  if (err) {
    throw(err);
  }
  server.start();
});
```

# Configuration
1. Pass configuration to the server and want to access it in every plugin.
```
var config = {
  dbConnection: 'mongodb://db1.example.net,db2.example.net:2500/?replicaSet=test'
};
var server = new Hapi.Server({
  app: config
});
```

# Server Methods
Server methods can be used to share functions by attaching them to the server instance.
```
var fetch = function (username, next) {
  // get tweets from the twitter API
  next(null, tweets);
};

server.method({
  name: 'twitter.fetch',
  method fetch
});
```
Access the method using ```server.methods.twitter.fetch```.