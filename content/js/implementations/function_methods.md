---
title: Function Methods
linktitle: Function Methods
type: book
date: "2021-08-11T05:29:00+08:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---


## How To Implement JavaScript Function Methods (Call, Bind and Apply) From Scratch?


### How to implement `Function.prototype.call` from scratch?

To implement call() from scratch, we need to know its 

第一点：this绑定在了env上面

```js
var env = {value: 'hello'};
var caller = function () {
	console.log(this.value);
}
caller.call(env); // hello
```

如何实现?
```js
var env = {
	value: 1,
	caller: function() {
		console.log(this.value);
	}
}
```
所以call2可以这样实现：
```js
Function.prototype.call2 = function (context) {
	context.func = this;
	context.func();
	delete context.func;
}
```

第二点：caller是可以接受参数的
比如
```js
var caller = function(first, last) {
	console.log(this.value+first+last);
}

caller.call(env, ‘Jiashuo’,’Tong’)会输出helloJiashuoTong
```
所以在call2中提取传入的参数
```js
Function.prototype.call2 = function (context) {
	context.func = this;
  args = [];
  for (let i=1; i<arguments.length; i++) {
    args.push(arguments[i]);
  }
	context.func(…args);
	delete context.func;
}
```

第三点：call()函数可以传null，当为null时，指向window
所以开头加上var context = context || window;
第四点：caller()函数会返回值
所以把context.fn(…args)赋值给result，最终返回result
```js
Function.prototype.call2 = function (context) {
  var context = context || window;
  context.func = this;
  args = [];
  for (let i=1; i<arguments.length; i++) {
    args.push('arguments[' + i + ']');
  }
  var result = eval('context.func(' + args + ')');
  delete context.func;
  return result;
}
```

### How to implement `Function.prototype.apply` from scratch?
```js
Function.prototype.apply2 = function (context, arr) {
  var context = Object(context) || window;
  context.fn = this;
  var result;
  if (!arr) {
    result = context.fn();
  }
  else {
    result = context.fn(…arr);
  }
  delete context.fn
  return result;
}
```

### How to implement `Function.prototype.bind` from scratch?

We find the following description of `Function.prototype.bind` from MDN Web Docs

> The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

Reading through the first part of the sentence, we know what `bind` is expected to return. It returns a function which is almost the same as the calling function except that its `this` keyword refers to the first object passed into it. Making use of `apply`, we can write a simplied version of bind() as

```js
Function.prototype.bind2 = function (context) {
	var self = this; // 提取this函数
	return function () {
		return self.apply(context);
  }
}
```

The second part of the sentence describes the other usage of `bind`: We can also bind arguments to the calling function as illustrated by the following example. 

```js
var foo = {
    value: 'val'
};

function bar(arg1, arg2) {
    console.log(arg1);
    console.log(arg2);
}

var bindFoo = bar.bind(foo, 'hello');
bindFoo('word'); // helloworld
```

Thus, we bind part of the arguments to the calling funtion upon returning from `bind` and pass some more arguments while calling the returned function. Therefore, we should somehow pack up the arguments passed in different stages and pass them as a whole to the returned function when it is called. Here is an implementation that works. 

```js
Function.prototype.bind2 = function (context) {
	var self = this;
	var boundArgs = Array.prototype.slice.call(arguments, 1);
	return function () {
		var args = Array.prototype.slice.call(arguments);
		return self.apply(context, boundArgs.concat(args));
	}
}
```