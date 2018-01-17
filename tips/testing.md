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
