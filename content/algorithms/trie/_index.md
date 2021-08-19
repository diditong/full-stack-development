---
# Title, summary, and page position.
linktitle: Trie
summary: 
weight: 6
icon: book-reader
icon_pack: fas

# Page metadata.
title: Trie
date: "2021-08-18T12:37:00Z"
type: book  # Do not modify.
---


There are tons of resources out there with which you can learn the concept of trie. Assuming that you already know what it is, we start from discussing when to use it. Here is my opinion.

> Whenever you need to find one string from multiple strings many times, you probably should build a trie to accelerate the process.

As we know, searching for a string of average length m from n strings takes O(nm) time if an array is used. A trie, like a tree, can speed up the process to O(log(m)) which is amazing. However, since building a trie itself takes O(nm) time, we should not bother creating one if we are going to perform searching on it only once.

If you know it when you need a trie to solve a problem, the rest to do would be pretty much routine work, that is, to build a trie from raw data and search the trie in whatever way it requires. Different people have different implementations of these functions, but I thought people might find the following template code (written in JavaScript) useful. I find myself almost always (9 times out of 10) using this subroutine to build the trie. 

```js
var getTrie = function(words) {
    var trie = new Object();
    var obj;
    for (const word of words) {
        obj = trie;
        for (const char of word) {
            if (!obj[char]) {
                obj[char] = new Object();
            }
            obj = obj[char];
        }
        obj['!'] = word;
    }
    return trie;
}
```


A collection of problems of this type:
1. LC208
2. LC212
3. Split String Based on Multiple Delimiters