---
title: Array Methods
linktitle: Array Methods
type: book
date: "2021-08-10T07:00:00+08:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---
## How To Implement JavaScript Array Methods From Scratch?

### How to implement map() from scratch?

According to MDN Web Docs, the syntax for `Array.prototype.map()` is `map(function callbackFn(element, index, array) { ... }, thisArg)`

The only required parameter for the callback function is `element` which is the current element being processed in the array, while `index` and `array` are optional. Ignoring the optionals, we can write a simplified version of the map function as follows

```js
const myCustomMapFunction = function(callbackFn) {
    const newArray = [];

    // 'this' refers to the array
    for (let i = 0; i < this.length; i++) {
        newArray[i] = callbackFn(this[i]);
    }

    return newArray;
}

Array.prototype.myCustomMapFunction = myCustomMapFunction;
```
Now, let us take care of the optional parameters. Notice that `index` stands for the index of the current element being processed in the array, and that `array` stands for the array `map` was called upon. We can simply pass `i` and `this` to our callback function

```js
const myCustomMapFunction = function(callbackFn) {
    const newArray = [];

    for (let i = 0; i < this.length; i++) {
        newArray[i] = callbackFn(this[i], i, this);
    }

    return newArray;
}

Array.prototype.myCustomMapFunction = myCustomMapFunction;
```

It is not yet the end of the story. We still need to worry about `thisArg` which is the value to use as this when calling `callbackFn`. We can simply use `Function.prototype.bind` to change what `this` in `callbackFn` refers to. 

```js
const myCustomMapFunction = function(callbackFn, thisArg) {
    const newArray = [];

    // assign this (the array) to a new variable
    const arr = this;

    // bind thisArg to callbackFn
    const boundCallback = callbackFn.bind(thisArg);

    for (let i = 0; i < arr.length; i++) {
        newArray[i] = boundCallback(arr[i], i, arr);
    }

    return newArray;
}

Array.prototype.myCustomMapFunction = myCustomMapFunction;
```


### How to implement filter() from scratch?
According to MDN Web Docs, the syntax for `Array.prototype.filter()` is `filter(function callbackFn(element, index, array) { ... }, thisArg)`. Similar to `myCustomMapFunction`, we can write a custom filter function.

```js
const myCustomFilterFunction = function(callbackFn, thisArg) {
    const newArray = [];

    // assign this (the array) to a new variable
    const arr = this;

    // bind thisArg to callbackFn
    const boundCallback = callbackFn.bind(thisArg);

    for (let i = 0; i < arr.length; i++) {
      if (boundCallback(arr[i], i, arr)) {
        newArray.push(arr[i]);
      }
    }

    return newArray;
}

Array.prototype.myCustomFilterFunction = myCustomFilterFunction;
```

### How to implement reduce() from scratch?
According to MDN Web Docs, the syntax for `Array.prototype.reduce()` is `reduce(function callbackFn(accumulator, currentValue, index, array) { ... }, initialValue)`. 

```js
const myCustomReduceFunction = function(callbackFn, initialValue) {
    // deal with the edge cases
    if (!this.length) return !initialValue ? TypeError('Reduce of empty array with no initial value') : initialValue;

    let res = (initialValue === undefined) ? this[0] : initialValue;
    const begin = (initialValue === undefined) ? 1 : 0;

    console.log('res ===> ', res);
    console.log('begin ===> ', begin);

    for (let i=begin; i<this.length; i++) {
        res = callbackFn(res, this[i], i, this);
    }
    
    return res;
}

Array.prototype.myCustomReduceFunction = myCustomReduceFunction;

```

### How to implement flat() from scratch?

```js
var flatten = function(arr) {
    var res = [];
    for (item of arr) {
        if (typeof item === ‘number’) {
 			res.push(item);
        } else {
	        res = res.concat(flatten(item));
	    }
    }
    return res;
}
```