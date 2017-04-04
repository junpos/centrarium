---
layout: post
title:  "ES6: Rest and defaults parameters"
date:   2017-03-29 01:43:59
author: Jun Kim
categories: ES6
---

### Rest parameters

In a variadic function that accepts any number of arguments, `arguments object` is the only way to access them.
Let's write a simple function to demonstrate how it works.
For example, `isDivisibleAll(10, 1, 2)` would return true, and `isDivisibleAll(10, 1, 3)` would return false.

```js
// ES5 old way
function isDivisibleAll(dividend) {
  for (var i = 1; i < arguments.length; i++) {
    var divisor = arguments[i];
    if (dividend % divisor !== 0) {
      return false;
    }
  }

  return true;
}
```

It uses the magical `arguments object`, an array-like object containing the parameters to the function. One thing we need to remember is the first additional argument is located at `1` not `0`. It doesn't look very intuitive at all.

So here comes *Rest Parameters*
```js
// ES6 Rest Parameters
function isDivisibleAll(dividend, ...divisors) {
  for (var divisor of divisors) {
    if (dividend % divisor !== 0) {
      return false;
    }
  }

  return true;
}
```

This version of the function works exactly the same except this contains the special `...divisors` syntax where the ellipsis before `divisors` indicates it is a rest parameter. All the other passed parameters are put into an array and assigned to the variable `divisors`.
Note that the parameters before the rest parameter are called *Usual Parameters*, and only the last parameter could be marked as a rest parameter. Even though there are no extra arguments, the rest parameter will never be `undefined`, but an empty array

------

### Default parameters
JavaScript has inflexible form of default parameters; when no value is passed the parameters are default to `undefined`.

```js
// ES5 old way
function greeting (name) {
  name = typeof name !== 'undefined' ? name : 'Jun';
  console.log('Good morning ' + name + '!');
}

greeting(); // 'Good morning Jun!'
greeting('Mike'); // 'Good morning Mike!'

```
This simple functions sets name to be `'Jun'` if no value is passed and it defaults to `undefined`


```js
// ES6 Default Parameters
function greeting (name = 'jun') {
  console.log('Good morning ' + name + '!');
}

greeting(); // 'Good morning Jun!'
greeting('Mike'); // 'Good morning Mike!'
```

The part with the `=` is an expression specifying the default value of the parameter if it's not passed from caller. Note that if the value of parameter is `null` it is considered as a passed value and the default parameter will be ignored.


```js
greeting(null); // 'Good morning null!'
```


------
### Conclusion

Technically it's not new behavior, but rest parameters and default parameters definitely would makes your code more expressive and readable. Why not use it!
