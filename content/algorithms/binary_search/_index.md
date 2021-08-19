---
# Title, summary, and page position.
linktitle: Binary Search
summary: 
weight: 1
icon: book-reader
icon_pack: fas

# Page metadata.
title: Binary Search
date: "2021-08-13T09:11:00Z"
type: book  # Do not modify.
---
Common patterns:
1. Detect target in array. Return index if found, or -1 if not found.
```js
const findIndex = (arr, target) => {
  let l = 0;
  let r = arr.length-1;
  
  while (l < r) {
    const m = Math.floor((l+r) / 2);
    const val = arr[m];
    if (val === target) 
      return m;
    else if (val < target)
      l = m+1;
    else if (val > target)
      r = m-1;
  }

  if (arr[l] === target) return l;
  return -1;
}
```
2. Find the index of the first element that is strictly greater than target.
```js
const findIndex = (arr, target) => {
  let l = 0;
  let r = arr.length-1;
  
  while (l < r) {
    const m = Math.floor((l+r) / 2);
    const val = arr[m];
    if (val <= target) {
      l = m+1;
    } 
    else if (val > target) {
      r = m;
    }
  }

  if (arr[l] > target) return l;
  return -1;
}
```
3. Find the index of the last element that is less than or equal to target.
val(2) = val(3) + 1
4. Find the index of the last element that is strictly smaller than target.
```js
const findIndex = (arr, target) => {
  let l = 0;
  let r = arr.length-1;
  
  while (l < r) {
    const m = Math.ceil((l+r) / 2);
    const val = arr[m];
    if (val >= target) {
      r = m-1;
    } 
    else if (val < target) {
      l = m;
    }
  }

  if (arr[l] < target) return l;
  return -1;
}
```
5. Find the index of the first element that is greater than or equal to target.
val(4) = val(5) - 1



