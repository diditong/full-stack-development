---
# Title, summary, and page position.
linktitle: This
summary: 
weight: 1
icon: book-reader
icon_pack: fas

# Page metadata.
title: This
date: "2021-09-17T17:30:00Z"
type: book  # Do not modify.
---

This post summarizes almost everything about `this` binding
## Two always:
1. `this` always refers to an object
2. what `this` refers to always depends on the call site

## Differences between the valuation of `this` and that of regular variables:
1. the valuation of `this` occurs in function calls, while the valuation of variables occurs upon compiling.
2. the valuation of `this` depends on the call stack/execution context, while the valuation of variables depends on the lexical scope/environment.

## Variations:
1. Default binding: In the strict mode, this refers to undefined, while in the non-strict mode, it refers to the window object.
2. Implicit binding: Binds to the object from which the function is called, e.g., `obj.fn()`.
3. Implicit unbinding: Binds to the window object instead of the calling object, e.g. `var func = obj.fn(); func()`; 
4. Explicit binding: Intentionally binds an object to the calling function using `call()`, `apply()`, and `bind()`, e.g., fn.call(obj), fn.apply(obj), fn.bind(obj). Note that when obj is `null` or `undefined`, `this` of fn is bound to the window object.
5. Binding with `new`: Binds `this` to the instance created with `new` and the constructor.  
6. Anonymous functions: Same rules applied as the regular functions
7. Arrow functions: Irrelevant to `this`. `this` belongs to the outer function.
8. `this` and `return`:
  * if a reference-type value is returned, `this` refers to the returned object.
  * otherwise (a primitive-type value is returned), `this` refers to the instance created by `new` and the constructor.