## Examples

**`series TAB ->`**

```javascript
return async.series({
  nameOfStepOne: function(callback) {
    // After naming step one and tabbing again, you are here
  }
});
```

**`step TAB ->`**

```javascript
return async.series({
  stepOne: function(callback) {
    ...
  },
  stepTwo: function(callback) {
    // Position yourself after step one, type "step"
    // and hit tab; you are prompted to name "stepTwo".
    // Hit tab again and you're here
  }
});
```

**`each TAB ->`**
```javascript
return async.eachSeries(myArray, function(item, callback) {
  // After typing myArray and pressing tab again, you are here
}, callback);
```

**`err TAB ->`**
```javascript
if (err) {
  return callback(err);
}
```

**`done TAB ->`**
```javascript
return callback(null);
```

**`skip TAB ->`**
```javascript
// Safely invoke callback when we didn't
// do anything async
return setImmediate(callback);
```

**`skipr TAB ->`**
```javascript
// Safely invoke callback with some data when we
// didn't anything async
return setImmediate(_.partial(callback, null, data));
```

**`whilst TAB ->`**
```javascript
var done = false;
return async.whilst(
  function() { return !done; },
  function(callback) {
    // You start typing again here
  }, callback
);
```

Your inner function keeps getting called until you set `done` to true.

## Philosophy

If you're gonna code with callbacks, you gotta use the [async module](https://github.com/caolan/async). Otherwise you're writing spaghetti code and infinitely nested curly braces.

The async module solves this problem, but it takes a lot of typing and that leads to mistakes. That's where these snippets come in.

These snippets have aggressively short keywords to make it easy to drill them into your fingers and improve your node programming style permanently.

All of these snippets start with `return` to avoid accidentally falling through to other code. Calling another function that takes a callback does *not* stop the execution of your function unles you return.

The `each` snippet deliberately uses `eachSeries`. `async.each` is usually a mistake because you haven't thought about the consequences of (100? 1,000? 1,000,000?) things happening at once.

Switch your code to `eachLimit` later if you have a rational reason to believe you can handle more than one at a time safely.

You should always use `skip` if you have not done any asynchronous work yet in your function and you need to invoke a callback asynchronously to avoid crashing the JavaScript stack. If you don't use `skip` (which calls `setImmediate`), your code will seem fine until one day you have a few hundred items and the JavaScript stack crashes. This is especially true with `async.eachSeries`.

## Credits

These snippets were developed to support our work at [P'unk Avenue](https://punkave.com).
