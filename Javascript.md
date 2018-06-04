### 1. How to flatten an array?

```javascript
function flatten (array) {
  return array.reduce((acc, val) =>
    Array.isArray(val) ? acc.concat(flatten(val)) : acc.concat(val), []);
}

```

Note that there is a proposal for adding `flatten` to `Array` in the new ES draft. Also the hilarious incident about changing name to "smoosh" for web compatibility concerns.

### 2. Given a node from a DOM tree, find the node in the same position from an identical DOM tree.

```javascript
function toArray (arrayLike) {
  return Array.from ? Array.from(arrayLike) : Array.prototype.slice.call(arrayLike);
}

function findDOMNode (node, root) {
  var traversePath = [];
  var currentNode = node;
  var index = 0;

  while (currentNode.parentNode) {
    index = toArray(currentNode.parentNode.childNodes).indexOf(currentNode);
    traversePath.push(index);
    currentNode = currentNode.parentNode;
  }

  currentNode = root;
  while (traversePath.length) {
    index = traversePath.pop();
    currentNode = currentNode.childNodes[index];
  }

  return currentNode;
}
```
v
### 3.Write an emitter class:

```javascript


// emitter = new Emitter();

// 1. Support subscribing to events.
// sub = emitter.subscribe('event_name', callback);
// sub2 = emitter.subscribe('event_name', callback2);

// 2. Support emitting events.
// This particular example should lead to the `callback` above being invoked with `foo` and `bar` as parameters.
// emitter.emit('event_name', foo, bar);

// 3. Support unsubscribing existing subscriptions by releasing them.
// sub.release(); // `sub` is the reference returned by `subscribe` above 

function Emitter () {
  this.events = {};
}

Emitter.prototype.subscribe = function (event, callback) {
  (this.events[event] || this.events[event] = []).push(callback);
  var index = this.events[event].length - 1;

  return {
    release: function () {
      this.events[event].splice(index, 1);
    }
  }
}

Emitter.prototype.emit = function (event) {
  var args =  Array.prototype.slice.call(arguments, 1);
  var events = this.events[event]; 
  for (var i = 0; i < events.length; i++) {
    events[i].apply(this, args);
  }
}

```

### 4.What are the advantages of using ES6 maps over objects? What about using ES6 sets over arrays? 

1. Map preverses the order of its keys, stable iteration performance across browsers.
2. Map tends to perform better in storing large set of data, especially when keys are unknown until run time, and when all keys are the same type and all values are the same type.
3. Map can use Object as key.

While:
1. Object has concise literal syntax.
2. Object has build-in support for JSON(JSON.stringfy)
3. Object can be used with destructuring.

Set:
1. Set only contains different elements
2. Set tends to perform better with basic operations, especially with largt data set.


### What is throttle and debounce? Write an implementation.

Throttling enforces a maximum number of times a function can be called over time.
Debouncing enforces that a function not be called again until a certain amount of time has passed without it being called. 

```javascript
function debounce(fn) {
  var threshold = 100;
  var timer = null;
  return function debounced () {
    clearTimeout(timer);
    var self = this;
    var args = arguments;
    timer = setTimeout(function(){
      fn.apply(self, args);
    }, threshold);
  }
}

function throttle (fn, threshold) {
    threshold = threshold || 100;
    let last;
    let timer;
    return function throttle () {
        const now = +new Date;
        const args = arguments;
        const self = this;
        if(last && now < last + threshold){
            clearTimeout(timer);
            timer = setTimeout(function(){
                fn.apply(self, args);
            }, threshold)
        } else {
            last = now;
            fn.apply(self, args);
        }
    };
}
```







