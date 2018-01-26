# Most Intetersting Interview Questions Collection

### 1. Is it ever possible that (a ==1 && a== 2 && a==3) could evaluate to true, in JavaScript?
Short anwser, yes, with the help of `toString`, `valueOf` or `Object.defineProperty`. See [Stackoverflow thread](https://stackoverflow.com/questions/48270127/can-a-1-a-2-a-3-ever-evaluate-to-true). Moreover, see how JavaScript types and how `ToPrimitive` works in [here](http://2ality.com/2012/01/object-plus-object.html)
