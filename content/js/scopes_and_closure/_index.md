---
# Title, summary, and page position.
linktitle: Scopes and Closure
summary: 
weight: 1
icon: book-reader
icon_pack: fas

# Page metadata.
title: Scopes and Closure
date: "2021-09-10T22:32:00Z"
type: book  # Do not modify.
---

## What are Scopes (Lexical Scopes/Environments)?
Lexical scopes contain an environment record in which the variables are stored, and a reference to their parent environments (if any). Lexical scopes build a tree structure. It is defined at lexing time. In other words, lexical scope is based on where variables and blocks of scope are authored, by you, at write time, and thus is (mostly) set in tone by the time the lexer processes your code. 

Engine, compiler and scope talk to each other when JS code is run. You can imagine scopes as bubbles. When a variable is needed, it is looked for from the inner scopes to the outer scopes. The look-up process stops once it finds the first match.

<!-- ## Dynamic scopes
Cheating lexical: eval(..) and with but we should never use them because they defeat the Engine's ability to perform compile-time optimizations regarding scope look-up, -->

### Global Scopes（全局作用域）
var declarations add to the global scope while let declarations do not
```js
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```


### Function scopes（函数作用域）
#### Why using function scope?* 
Function scope deals with variables defined by var, let and const
Collision Avoidance: Global namespace, Module management
*Writing this way will pollute the global scope*
var a = 2;

function foo() { // <-- insert this

	var a = 3;
	console.log( a ); // 3

} // <-- and this
foo(); // <-- and this

console.log( a ); // 2

*This is definitely better: named instead of anonymous, immediately invoked function expressions*
var a = 2;

(function foo(){ // <-- insert this

	var a = 3;
	console.log( a ); // 3

})(); // <-- and this

console.log( a ); // 2


(function...是function expression
如何区别function declaration和function expression
除function...以外全部是function expression

#### Anonymous functions
Advantages：easy to type
Disadvantages：hard to debug, cannot call itself in case of recursion, no name as a description

### Block Scopes（块级作用域）
*Why using block scopes?*
Declare variables as close as possible, collect garbage at the end of a block
*What are block scopes*
With, try/catch, {}
How to use block scopes?



### Hoisting

Use let/const instead of var inside block scopes
```js
function varTest() {
  var x = 1;
  {
    var x = 2;  // the same variable
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  {
    let x = 2;  // a different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

# Scope Closure
## What is Closure?
When we define an inner function in a lexical scope, transport it outside of its scope, and call it in some other scope. That function still has a reference to the scope in which it is defined, and that reference is called closure.

1. Although the execution context in which we define the inner function is disposed, we can still access this function.
2. We can use free variables (that are neither defined in the function nor passed into the function.)



## Examples of Closure:

### Example 1
### bar is defined in the scope of foo, but called in the global scope
function foo() {
	var a = 2;

	function bar() {
		console.log( a );
	}

	return bar;
}

var baz = foo();
baz(); // 2 -- Whoa, closure was just observed, man.


### Example 2
### baz is defined in the scope of foo, but called in the scope of bar
var fn;

function foo() {
	var a = 2;

	function baz() {
		console.log( a );
	}

	fn = baz; // assign `baz` to global variable
}

function bar() {
	fn(); // look ma, I saw closure!
}

foo();
bar(); // 2

### Example 3
### timer is defined in the scope of wait but called in the global scope
function wait(message) {

	setTimeout( function timer(){
		console.log( message );
	}, 1000 );

}

wait( "Hello, closure!" );


### Example 4
https://github.com/mqyqingfeng/Blog/issues/9


### Example 5
### Modify this code so it prints 5,0,1,2,3,4
Original
```js
for(var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000)
}
console.log(i);
// prints 5,5,5,5,5,5
```
Mod 1: Use A Closure
```js
for(var i = 0; i < 5; i++) {
  let j = i;
  setTimeout(function() {
    console.log(j);
  }, 1000)
}
console.log(i);
// prints 5,0,1,2,3,4
```
Mod 2: Use A Closure In a Different Way
```js
for(var i = 0; i < 5; i++) {
  setTimeout((function(i) {
    return function () {
      console.log(i);
    }
  })(i), 1000)
}
console.log(i);
// prints 5,0,1,2,3,4
```

## What are Execution Contexts (Call Stack)?
Execution Contexts contain the current evaluation state of code, a reference to the code (function) itself, and possibly references to the current lexical context (scope). Execution contexts are managed in a stack.


## Event Loop, Microtasks and Macrotasks
When the JS engine is busy, the tasks are enqueued into the macro/micro task queues. Tasks in the queues are waiting to be run in the call stack. They are not asynchronous tasks but tasks (registered functions) returned by asynchronous operations such as setTimeout, then, etc.



