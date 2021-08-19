---
# Title, summary, and page position.
linktitle: Matrix Manipulation
summary: 
weight: 3
icon: book-reader
icon_pack: fas

# Page metadata.
title: Matrix Manipulation
date: "2021-08-18T11:00:00Z"
type: book  # Do not modify.
---
The most common matrix manipulations usually involve operations such as flipping over the diagonal, rotating by 90 degrees clockwise, etc. Here, I'm going to break them down into several categories. 

Flip Over Diagonals
```js
function flipOverDiagonals(matrix) {
  const n = matrix.length;
  for (let i=1; i<n; i++) {
    for (let j=0; j<i; j++) {
      [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
    }
  }
  return matrix;
}

function flipOver2ndDiagonals(matrix) {
  const n = matrix.length;
  for (let i=0; i<n; i++) {
    for (let j=0; j<n-i; j++) {
      [matrix[i][j], matrix[n-1-j][n-1-i]] = [matrix[n-1-j][n-1-i], matrix[i][j]];
    }
  }
  return matrix;
}
```

Flip Over Midlines
```js
function flipOverVerticalMidline(matrix) {
  for (let row of matrix) {
    row.reverse();
  }
  return matrix;
}

function flipOverHorizontalMidline(matrix) {
  for (let i=0; i<Math.floor(matrix.length/2); i++) {
    [matrix[i], matrix[matrix.length-i-1]] = [matrix[matrix.length-i-1], matrix[i]];
  }
```

Rotate By 90 Degrees: it is just a combination of a flip over the 1st/2nd diagonal and a flip over the vertial midline
```js
function rotateBy90Clockwise(matrix) {
  flipOverDiagonals(matrix);
  flipOverVerticalMidline(matrix);
}

function rotateBy90CounterClockwise(matrix) {
  flipOver2ndDiagonals(matrix);
  flipOverVerticalMidline(matrix);
}
```

Rotate By 180 Degrees: yet another combination of two flips
```js
function rotateBy180(matrix) {
  flipOverVerticalMidline(matrix);
  flipOverHorizontalMidline(matrix);
}
```