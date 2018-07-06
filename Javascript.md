### 1. How to flatten an array?

```javascript
function flatten (array) {
  return array.reduce((acc, val) =>
    Array.isArray(val) ? acc.concat(flatten(val)) : acc.concat(val), []);
}

```

ES6 has `flat`.

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
  // we can prefix the event name to make sure no overwriting
  event = '$' + event;
  (this.events[event] || this.events[event] = []).push(callback);

  return {
    release: function () {
      // simple way is to remove, another is to go through all functions to keep the live ones
      this.events[event].splice(this.events[event].indexOf(callback), 1);
      // or swap with the last one and delete tail
    }
  }
}

Emitter.prototype.emit = function (event) {
  event = '$' + event;
  var args =  Array.prototype.slice.call(arguments, 1);
  var events = this.events[event]; 
  for (var i = 0; i < events.length; i++) {
    if (events[i])
    events[i].apply(events[i], args);
  }
}

```

### Optimize the following solution

```javascript
// Given input
// could be potentially more than 3 keys in the object above
items = [
  {color: 'red', type: 'tv', age: 18},
  {color: 'silver', type: 'phone', age: 20}
  //...
]

excludes = [
  {k: 'color', v: 'silver'},
  {k: 'type', v: 'tv'},
  //....
]

// following function is to exclude items
function excludeItems(items, excludes) {
   excludes.forEach(pair => {
      items = items.filter(item => item[pair.k] === item[pair.v]);
   });
   return items;
}
```

The current solution would run two loops, making runtime O(n^2). We can actually pre-process `excludes` to make it a hashmap, and do filtering in O(1).

The idea is to make excludes like this:
```javascript
excludes = {
  color: {
    silver: true,
    black: true
  },
  type: {
    tv: true,
    //...
  },
  age: {

  }
}

// function to to transform
function transform(excludes) {
  var res = {}
  excludes.forEach(item => {
    if (!res[item.k]) {
      res[item.k] = {}
    }
    res[item.k][item.v] = true
  })
  return res
}
```

So for filtering, we can do 

```javascript
  items = items.filter(item => !(excludes.color[item.color] && excludes.type[item.type] && excludes.age[item.age]))
```

Because item's properties are constant number, filtering an item is O(1), so the total time complexity is O(n+n)


### .What are the advantages of using ES6 maps over objects? What about using ES6 sets over arrays? 

1. Map preverses the order of its keys, stable iteration performance across browsers.
2. Map tends to perform better in storing large set of data, especially when keys are unknown until run time, and when all keys are the same type and all values are the same type.
3. Map can use Object as key.

While:
1. Object has concise literal syntax.
2. Object has build-in support for JSON(JSON.stringfy)
3. Object can be used with destructuring.

Set:
1. Set only contains different elements
2. Set tends to perform better with basic operations, especially with large data set.


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
        const now = performance.now();
        const args = arguments;
        const self = this;
        
        if (last == null || now > last + threshold)
            last = now;
            fn.apply(self, args);
        }
    };
}
```

### Write a class that support store DOM node and value

```javascript
function DOMStore() {
  this.map = {}
}

DOMStore.prototype.set = function (node, value) {
  if (node.__key) {
      node.__key = performance.now()
  }

  this.map[node.__key] = value
}

DOMStore.prototype.has = function (node) {
  return node.__key
}

DOMStore.prototype.get = function (node, value) {
  return this.map[node.__key]
}
```







