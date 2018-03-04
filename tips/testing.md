1. To run your tests in Node.js, [Travis](https://travis-ci.org/) is a great choice.

# Unit-testing

1. Spies - To know how many times a function gets called or what arguments are passed to them.
```
it('calls subscribers on publish', function () {
  var callback = sinon.spy()
  PubSub.subscribe('message', callback)

  PubSub.publishSync('message')

  assertTrue(callback.called)
})
```

2. Stubs - Control a method's behavior to force a code path (like throwing errors) or to prevent calls to external resources (like HTTP APIs).
```
it('calls all subscribers, even if there are exceptions', function (){
  var message = 'an example message'
  var error = 'an example error message'
  var stub = sinon.stub().throws()
  var spy1 = sinon.spy()
  var spy2 = sinon.spy()

  PubSub.subscribe(message, stub)
  PubSub.subscribe(message, spy1)
  PubSub.subscribe(message, spy2)

  PubSub.publishSync(message, undefined)

  assert(spy1.called)
  assert(spy2.called)
  assert(stub.calledBefore(spy1))
})
```

3. Mocks - Fake method with pre-programmed behavior and expectations.
```
it('calls all subscribers when exceptions happen', function () {
  var myAPI = { 
    method: function () {} 
  }

  var spy = sinon.spy()
  var mock = sinon.mock(myAPI)
  mock.expects("method").once().throws()

  PubSub.subscribe("message", myAPI.method)
  PubSub.subscribe("message", spy)
  PubSub.publishSync("message", undefined)

  mock.verify()
  assert(spy.calledOnce)
// example taken from the sinon documentation site: http://sinonjs.org/docs/
})
```

# Code coverage 

1. Using [istanbul](https://github.com/gotwarlost/istanbul)
pacakge.json script to use instanbul with mocha
```
istanbul cover _mocha $(find ./lib -name \"*.spec.js\" -not -path \"./node_modules/*\")
```

# Logging
1. Debug module
Code: 
```
const debug = require('debug')('my-namespace')
const name = 'my-app'
debug('booting %s', name)
```
Script:
```DEBUG=my-namespace node app.js```
Debugging multiple modules:
```DEBUG=my-namespace,express* node app.js```

2. Logging for applications using winston or bunyan.
```
const winston = require('winston')

winston.log('info', 'Hello log files!', {
  someKey: 'some-value'
})
```

3. Use [trace](https://trace.risingstack.com/) when logging in distributed systems. We have to use correlation identifiers in distributed systems.
```
const uuid = require('uuid')
const id = uuid.v1()
```

# TDD
You dont write tests for yourself. You write them for those who make changes later. Test driven development is a methodology for writing the tests first for a given module and for the actual implementation afterward. 
3 Steps -
a. writing failing tests
b. writing code that satisfies our tests
c. refactor.

Test Runner: Mocha
Assertions: Chai
Stubs/Mocks: Sinon
Utilities:
Chai-as-Promised
Sinon-Chai

Step by step use cases [here](https://blog.risingstack.com/getting-node-js-testing-and-tdd-right-node-js-at-scale/)
