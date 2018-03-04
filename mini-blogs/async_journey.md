# Problem with Async
Node.js itself is single threaded, but some tasks can run parallelly - thanks to its asynchronous nature.

But what does running parallelly mean in practice?
Since we program a single threaded VM, it is essential that we do not block execution by waiting for I/O, but handle them concurrently with the help of Node.js's event driven APIs.

## Async JS
Achieved by higher order functions (pass function as a param to a function).

## Callbacks
So called error-first callbacks are in the heart of Node.js itself. Challenges:
- Spaghetti code
- Error handling easy to miss
- Cant return values with return statement

## Async module
An NPM module to run things in parallel or sequentially.
```
async.map([1, 2, 3], AsyncSquaringLibrary.square, 
  function(err, result){
  // result will be [1, 4, 9]
});
```

## Promises
A promise represents the eventual result of an asynchronous operation.
Check this [Stackoverflow answer](https://stackoverflow.com/questions/42013104/placement-of-catch-before-and-after-then) to know the difference in the placement of catch before and after then.
When using Promises you may Polyfill issues in runtime. Use [Bluebird](https://github.com/petkaantonov/bluebird) as a full featured promise library.

## Callbacks and Promises
A good practice to support both for backward compatibility.
```
function foo(cb) {
  if (cb) {
    return cb();
  }
  return new Promise(function (resolve, reject) {
    
  });
}
```
Or use [callbackify](https://www.npmjs.com/package/callbackify)

## Generator/yield (ES6)
Covered in [koa tutorial](../quick-tutorials/koa.md)

## async await (ES7)
Under the hood async functions using Promises - this is why the async function will return with a Promise.