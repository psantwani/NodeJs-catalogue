# Hello World 

```
var koa = require('koa');
var app = koa();

app.use(function *() {
  this.body = 'Hello World';
});

app.listen(3000);

```

# Generators
> Wouldn't it be nice, that when you execute your function, you could pause it at any point, calculate something else, do other things, then return to it, even with some value and continue?

```
function* foo () {
  var val = yield 'A';
  console.log(val);           // 'B'
}
var bar =  foo();

console.log(bar.next());    // { value: 'A', done: false }

//Passing value in a next call()
console.log(bar.next('B')); // { value: undefined, done: true }
```

- ```function *foo (arg) { return arg }``` returns a generator function
- ```var bar = foo(123);``` return an iterator object
- ```bar.next();``` returns ```{ value: 123, done: true }```
- Calling next() starts the generator and it runs until it hits a yield.
- If you specify a return value in the generator, it will be returned in the last iterator object (when done is true)
- In ```for (v of foo()) { console.log(v) }```, you cannot pass a value in a next() call and the loop will throw away the returned value


# Thunks
Primarily they are used to assist a call to another function. They can be used to move node's callbacks from the argument list, outside in a function call. Use [thunkify](https://github.com/visionmedia/node-thunkify).

```
var read = function (file) {
  return function (cb) {
    require('fs').readFile(file, cb);
  }
}

read('package.json')(function (err, str) { })
```

Steps
- Convert function to a thunk and put it in generator
- next() returns a function whose parameter is the callback of the thunkified function
- Check error in callback (and throw if needed) or call ```next()``` with received data

```
//Synchronous code, with good error handling, but still happens async.
var thunkify = require('thunkify');
var fs = require('fs');
var read = thunkify(fs.readFile);

function *bar () {
  try {
    var x = yield read('input.txt');
  } catch (err) {
    throw err;
  }
  console.log(x);
}
var gen = bar();
gen.next().value(function (err, data) {
  if (err) gen.throw(err);
  gen.next(data.toString());
})
```