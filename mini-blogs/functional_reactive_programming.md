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