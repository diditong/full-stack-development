---
# Title, summary, and page position.
linktitle: Union Find
summary: 
weight: 7
icon: book-reader
icon_pack: fas

# Page metadata.
title: Union Find
date: "2021-08-21T06:21:00Z"
type: book  # Do not modify.
---



## What is Union Find?
The resource that I have used to study Union Find is Wikipedia: https://en.wikipedia.org/wiki/Disjoint-set_data_structure. I learned that Union Find is a data structure that allows three operations on it: creating a new set, finding the ancestor of a set, and merging two sets. I also learned the various implementations of the union and find functions.


## When to use Union Find?
Here is a list of Union Find problems: https://leetcode.com/tag/union-find/. One commonality of them is that it either asks you to find number of groups in an established setting or to merge some groups if they meet certain requirements. 


## How to use Union Find? 
I want to talk about how to use Union Find to solve these problems. It typically involves two steps: 1. implement Union Find 2. modify it to get the job done. 

### Implementations of Union Find
According to Wikipedia, there are several variations of both the union function and the find function.

#### Making new sets
```js
constructor() {
  this.size = {};
  this.parent = {};
  this.rank = {};
}
```

#### Finding set representatives
Recursive Path Compression:
```js
find(x) {
  if (x !== this.parent[x]) {
      this.parent[x] = this.find(this.parent[x]);
  }
  return this.parent[x];
}
```

Two-pass Iterative Path Compression:
```js
find(x) {
  let root = x;
  while (this.parent[root] !== root) {
    root = this.parent[root];
  }
  while (this.parent[x] !== root) {
    const temp = this.parent[x];
    this.parent[x] = root;
    x = this.parent[x];
  }
  return x;
}
```

One-pass Iterative Path Compression:
```js
find(x) {
  while (this.parent[x] !== x) {
    [x, this.parent[x]] = [this.parent[x], this.parent[this.parent[x]]];
  }
  return x;
}
```

Path Halving:
```js
find (x) {
  while (this.parent[x] !== x) {
    this.parent[x] = this.parent[this.parent[x]];
    x = this.parent[x];
  }
  return x;
}
```

#### Merging two sets
The two variation of Union are union by class and union by rank.

```js
union(x, y) {
  x = this.find(x);
  y = this.find(y);
  if (x === y) return;
  if (this.size[y] > this.size[x])
      [x, y] = [y, x];
  this.parent[y] = x;
  this.size[x] += this.size[y];
  delete this.size[y];
}
```

```js
union(x, y) {
  x = this.find(x);
  y = this.find(y);
  if (x === y) return;
  if (this.size[y] >= this.size[x])
      [x, y] = [y, x];
  this.parent[y] = x;
  if (this.rank[x] === this.rank[y]) this.rank[x] += 1;
  delete this.rank[y];
}
```
Notes:
1. Note that the `rank` property in union by rank does not refer to the exact rank of the tree set. Instead, it is the upper bound of the set. 
2. Union by class and union by rank guarantee that the find function in the worst cases takes O(log(N)) (rather than O(N)) time to complete. 


### How to modify Union Find?

I have found the following template (written in JavaScript) very useful and easy to remember as well.
```js
class UnionFind {
    constructor() {
      this.size = {};
      this.parent = {};
    }
    
    find(x) {
      if (x !== this.parent[x]) {
          this.parent[x] = this.find(this.parent[x]);
      }
      return this.parent[x];
    }
    
    union(x, y) {
      x = this.find(x);
      y = this.find(y);
      if (x === y) return;
      if (this.size[y] >= this.size[x])
          [x, y] = [y, x];
      this.parent[y] = x;
      this.size[x] += this.size[y];
      delete this.size[y];
    }
}
```