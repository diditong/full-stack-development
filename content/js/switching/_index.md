---
# Title, summary, and page position.
linktitle: Switching Languages
summary: 
weight: 3
icon: book-reader
icon_pack: fas

# Page metadata.
title: Switching Languages
date: "2021-08-10T00:07:26Z"
type: book  # Do not modify.
---

It is often the case that we need to switch to our secondary programming language in a coding interview, e.g., there is no ready-to-use heaps, queues, etc. in JavaScript so we want to switch to Python. However, people can mess up with grammars of different languages especially they have been writing code in the original language for a long time. Even though they usually recall the correct grammar after several trials, it can seem so unprofessiontal that the interviewer gets very well annoyed. To solve this problem, I have created the following table which summarizes the grammar to use (in columns) for each use case (in rows). 

| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |

|  | Python | JavaScript |
| :----: | :----: | :----: |
| Identifiers | No identifiers | const, let, and var |
| Symbols | : for func definiton, no ; | {} for func definition, ; after each sentence |
| Naming Conventions | var_name, funcName, ClassName, CONST_NAME | varName, funcName, ClassName, CONST_NAME |
| List/Array | [] -- append(), pop(), len() | [] -- push(), pop(), .length |
| Dictionary/Object | {} -- .keys(), .values(), .items(), key in dict | {} Object.keys(), Object.values(), Object.entries(), key in obj  |
| Set | {} -- add, remove, ... in {} | new Set() -- add, delete, has | 
| Heap | import heapq -- h[0], heapq.heappush(h, item), heapq.heappop() | **no such implementations** |
| Queue | from collections import deque -- q.push(), q.popleft() | **no such implementations** |
| Stack | [] -- pop() | [] -- pop() |
| Max Int | float('inf') | Number.MAX_SAFE_INTEGER |
| For Element | for elem in arr: | for (let elem of arr) {} |
| Destructuring Assignment | p1, p2 = line.split('.') | const [p1, p2] = line.split('.') |
| Nothing | None | undefined |
| Create Matrix | [[0 for i in range(w)] for j in range(h)] | Array.from({length: h}, ()=>new Array(w).fill(0)) |
| Copy Matrix | [[l1[i][j] for i in range(w)] for j in range(h)] | var newArray = currentArray.map(arr=>arr.slice();); |
| Random Number | import random, random.random() | import Math, Math.random() |
| Floor | |
| Swapping Elements | arr[i], arr[j] = arr[j], arr[i] | [arr[i], arr[j]] = [arr[j], arr[i]] |


