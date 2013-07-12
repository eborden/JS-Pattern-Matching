Destructuring is one of the most powerful features of ES6. It allows for painless assignment of variables. However, it could be so much more.

Destructuring is pattern matching. You can see this in one of the most shopped around examples of destructuring, flipping context.

```
var a = 1,
    b = 2;
console.log([b, a] = [a, b]);
```

Inherently this operation is matching a pattern. It returns the given array in spyder monkey (one of the few engines supporting destructuring). This can be a useful tool.

Consider the following statement:

```
if (i = true) {
  console.log('this passes');
}
```

The implicit value is obvious in left hand assignment. It is the value of ```i```. With a destructured statement the value is less implicit, but logical to infer.

```
console.log([a, b] = [1, 2]);//[1, 2]
console.log([a, b] = [1]);//[1]
```

Now consider the following:

```
([a, b] = [1, 2]).length;//true, all values have been fufilled
([a, b] = [1]).length == 2;//false
```

Rest parameters get a bit more complicated, since a rest parameter will return an empty array when no value is present:

```
console.log([a, ...rest] = [1, 2]);//[1, [2]]
console.log([a, ...rest] = [1]);//[1, []]
```

So length fails to provide any significant test. Length again fails in the case of arrays with undefined values.

```
var x = [];
x[0] = 1;
x[2] = 3;
([a, b, c] = x).length;//3
```

With this in mind a testing operator needs to be implemented, so we can get the full power of pattern matching. I propose the ```?=``` operator as an existential pattern matcher.

```
[a, b] ?= [1, 2];//true
```
```
[a, b] ?= [1];//false
```
```
[a, ...rest] ?= [1, 2];//true
```
```
[a, ...rest] = [1];//false
```
```
var x = [];
x[0] = 1;
x[2] = 3;
[a, b, c] ?= x;//false
```

```?=``` providing ```let``` style assignment would further its usefulness.

```
if ([x, ...rest] ?= [1, 2, 3, 4]) {
  console.log(rest);
}
```

In this context rest parameters could be expanded to include an unassigned rest.

```
[a, ...] ?= [1, 2];//true
[a, ...] ?= [1];//false
```

With this pattern matching (and tail call optimization) we could implement elegant recursive functions.

```
function reverse (a) {
  if (a.length < 1) {
     return [];
  } else if ([x, ...rest] ?= a) {
     return reverse(rest).concat(x);
  }
}
```

I have excluded the use of object destructuring for simplicity, but the concept easily transfers:

```
if ({parent: {child: {name: n}}} ?= obj) {
  console.log('child has a name');
}
```
