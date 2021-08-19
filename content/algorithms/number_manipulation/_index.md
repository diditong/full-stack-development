---
# Title, summary, and page position.
linktitle: Number Manipulation
summary: 
weight: 4
icon: book-reader
icon_pack: fas

# Page metadata.
title: Number Manipulation
date: "2021-08-18T11:06:00Z"
type: book  # Do not modify.
---

For this type of problems, you usually get a number or a group of numbers as the input, perform some manipulations on it/them, and return the result. The most common manipulations involved are reversing an integer, swapping digits, etc. The most common practice when dealing with number manipulations is to get the last digit with `digit = num % 10` while updating the original with `num = num / 10`. Here is an example.\
Problem: Sum of Reversed Numbers. (E.g., [12, 34, 4300, 5500]=>8964)\
Solution:
```js
const reverseSum = (ints) => {
  const reverseInt = (int) => {
    let numZeros = 0;
    while (int % 10 === 0) {
      int /= 10;
      numZeros += 1;
    }
    let res = 0;
    while (int !== 0) {
      res = res * 10 + int % 10;
      int = Math.floor(int / 10);
    }
    return res * Math.pow(10, numZeros);
  }
  const reducer = (accu, curr) => {
    return accu + reverseInt(curr);
  }
  return ints.reduce(reducer, 0);
}
```

Another useful technique to accomplish such tasks is to break up the integer into an array of individual digits, and move/change the digits, as demonstraded by the example below.\
Problem: Swap Number By Pair. (E.g., 12345=>21435, 123456=>21436)
Solution:
```js
const swapNumberByPair = (num) => {
  const digArr = [];
  while (num !== 0) {
    digArr.push(num % 10);
    num = Math.floor(num / 10);
  }
  digArr.reverse();
  console.log(digArr);
  for (let i=0; i+1<digArr.length; i+=2) {
    [digArr[i], digArr[i+1]] = [digArr[i+1], digArr[i]];
  }
  console.log(digArr);
  let res = 0;
  for (let i=0; i<digArr.length; i++) {
    res = res*10+digArr[i];
  }
  return res; 
}
```

