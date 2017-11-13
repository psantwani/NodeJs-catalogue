FRP uses functional utilities like map, filter, and reduce to create and process data flows which propagate changes through the system: hence, reactive. When input x changes, output y updates automatically in response.

Example -
```
_('click', $('#cats-btn'))
  .throttle(500)	// can be fired once in every 500ms 
  .pipe(getDataFromServer)
  .map(formatCats)
  .pipe(UI.render);
```

Use [Highland](http://highlandjs.org/), [Bacon](https://github.com/baconjs/bacon.js/) to implement FRP in NodeJs.

```
var stream = require('stream');               
var _ = require('highland');
//Create a writeable stream
var toConsole = new stream.Writable({
  objectMode: true 
});
toConsole._write = function (data, encoding, done) {
  console.log(data);
  done();
};
//Random function
function underThree (cat) {
  return cat.age < 3;
}
_(catWS)
  .map(JSON.parse)
  .sequence()
  .filter(underThree)
  .map(util.format)
  .pipe(toConsole);
```