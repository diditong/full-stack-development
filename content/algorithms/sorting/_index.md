---
# Title, summary, and page position.
linktitle: Sorting Algorithms
summary: 
weight: 6
icon: book-reader
icon_pack: fas

# Page metadata.
title: Sorting Algorithms
date: "2021-08-18T15:30:00Z"
type: book  # Do not modify.
---
1. Quick Sort
```js
const partition = (nums, lo, hi, k) => {
  let l = lo;
  let r = hi;
  while (l <= r) {
    while (l <= r && nums[l] < k) l++;
    while (l <= r && nums[r] >= k) r--;
    if (l <= r) {
      console.log('l, r===>', l, r);
      [nums[l], nums[r]] = [nums[r], nums[l]];
      l += 1;
      r -= 1;
    }
  }
  return l;
}

const quickSort = (nums, lo, hi) => {
  if (lo < hi) {
    const p = partition(nums, lo, hi, nums[hi]);
    console.log(lo, hi, nums);
    quickSort(nums, lo, p-1);
    quickSort(nums, p, hi);
  }
}

const arr = [3,1,4,1,5,9,2,6];
quickSort(arr, 0, arr.length-1);
console.log('arr===>', JSON.stringify(arr));
```

2. Merge Sort
```js
const mergeSort = (nums) => {
  if (nums.length <= 1) return nums;
  const m = Math.floor(nums.length / 2);
  const left = mergeSort(nums.slice(0, m));
  const right = mergeSort(nums.slice(m));

  let i = 0;
  let j = 0;
  const res = [];
  while (i < left.length || j < right.length) {
    if (i === left.length && j < right.length) {
      res.push(right[j++]);
    }
    else if (j === right.length && i < left.length) {
      res.push(left[i++]);
    }
    else {
      if (left[i] < right[j]) 
        res.push(left[i++]);
      else
        res.push(right[j++]);
    }
  }
  console.log(res);
  return res;
}
```

3. Bubble Sort
```js
const bubbleSort = (nums) => {
  let sorted = false;
  while (!sorted) {
    sorted = true;
    for (let i=0; i<nums.length-1; i++) {
      if (nums[i] > nums[i+1]) {
        sorted = false;
        [nums[i], nums[i+1]] = [nums[i+1], nums[i]];
      }
    }
  }
}
```
Avg. time complexity:
Best case:
Worst case:

4. Recursive Bubble Sort
```js
const bubbleSort = (nums, n) => {
  if (n === 1) return;

  for (let i=0; i<nums.length; i++) {
    if (arr[i] > arr[i+1])
      [arr[i], arr[i+1]] = [arr[i+1], arr[i]];
  }

  bubbleSort(nums, n-1);
}
```
Avg. time complexity:
Best case:
Worst case:

5. Selection Sort
```js
const selectionSort = (nums) => {
  for (let i=0; i<nums.length-1; i++) {
    let min = nums[i];
    let k = i;
    for (let j=i; j<nums.length; j++) {
      if (nums[j] < min) {
        min = nums[j];
        k = j;
      }
    }
    [nums[k], nums[i]] = [nums[i], nums[k]];
  }
}
```
Avg. time complexity:
Best case:
Worst case:


6. Insertion Sort
```js
const insertionSort = (nums) => {
  for (let i=1; i<nums.length; i++) {
    const temp = nums[i];
    let j = i-1;
    while (j >= 0 && nums[j] > temp) {
      nums[j+1] = nums[j];
      j -= 1;
    }
    nums[j+1] = temp;
  }
}
```
Avg. time complexity:
Best case:
Worst case:


Sorting Algorithms

Tree Sort 

1. Build a BST
2. Perform In-order Traversal

Heap Sort

1. Build a heap from the data
2. Pop the heap into a stack
3. Do 2 until the heap is empty

Pancake Sort - for sorting pancakes


Sorting Algorithms For Structured Data:

Bucket Sort - for sorting numbers that are evenly distributed in a range


Counting Sort - for sorting integers that are distributed in a small range


Cycle Sort - for sorting consecutive integers




QMBRSIPHB