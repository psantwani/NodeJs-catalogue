# Singletons

It restrict the number of instantiations of a "class" to one.
```var areaCalc = require('./area');```. No matter how many times we require the module, it'll exist as single instance in the app.

# Observers

An object maintains a list of observers and notifies them automatically on state changes.
```
var util = require('util');
var EventEmitter = require('events').EventEmitter;

function MyFancyObservable() {
  EventEmitter.call(this);
}

MyFancyObservable.prototype.hello = function (name) {
  this.emit('hello', name);
};

util.inherits(MyFancyObservable, EventEmitter);
```

Test it :
```
var MyFancyObservable = require('MyFancyObservable');
var observable = new MyFancyObservable();

observable.on('hello', function (name) {
  console.log(name);
});

observable.hello('john');
```

# Factories
Provide a generic interface for creating objects (creational pattern). You can inject the module dependencies using this pattern.
```
function MyClass (options) {
  this.options = options;
}

function create(options) {
  // modify the options here if you want
  return new MyClass(options);
}

module.exports.create = create;
```

# Dependency Injection
A design pattern in which one or more dependencies (or services) are injected, or passed by reference, into a dependent object. Again, It makes testing a lot easier.

# Middlewares/pipeline
Output of one unit/function is the input for the next.

# Streams
You can think of streams as special pipelines. They are better at processing bigger amounts of flowing data, even if they are bytes, not objects.