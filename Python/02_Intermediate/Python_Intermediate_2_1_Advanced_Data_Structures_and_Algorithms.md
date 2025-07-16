# Python Intermediate 2.1: Advanced Data Structures and Algorithms

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Collections Module](#2-collections-module)
- [3. Comprehensions](#3-comprehensions)
- [4. Generators and Iterators](#4-generators-and-iterators)
- [5. Decorators (Basic)](#5-decorators-basic)
- [6. Sorting and Searching Algorithms](#6-sorting-and-searching-algorithms)
- [7. Time and Space Complexity](#7-time-and-space-complexity)
- [8. Recursion and Dynamic Programming](#8-recursion-and-dynamic-programming)
- [9. Practical Examples](#9-practical-examples)
- [10. Review Questions](#10-review-questions)
- [11. Answers to Review Questions](#11-answers-to-review-questions)

---

## 1. Introduction

This module covers advanced data structures and algorithms in Python, focusing on practical usage, performance, and code clarity. Mastery of these topics is essential for efficient, maintainable, and scalable software.

## 2. Collections Module

The `collections` module provides specialized data structures:
- `Counter`: Counting hashable objects
- `defaultdict`: Dictionary with default values
- `deque`: Double-ended queue

```python
from collections import Counter, defaultdict, deque
c = Counter('abracadabra')
d = defaultdict(int)
d['missing'] += 1
q = deque([1,2,3])
q.appendleft(0)
```

## 3. Comprehensions

Comprehensions provide concise syntax for creating lists, dicts, and sets.

```python
squares = [x**2 for x in range(10)]
even_dict = {x: x%2==0 for x in range(10)}
unique = {x for x in 'abracadabra'}
```

## 4. Generators and Iterators

Generators produce items one at a time, saving memory.

```python
def gen():
    for i in range(5):
        yield i
for x in gen():
    print(x)
```

Iterators implement `__iter__` and `__next__`:
```python
class MyRange:
    def __init__(self, n): self.n = n
    def __iter__(self):
        self.i = 0
        return self
    def __next__(self):
        if self.i < self.n:
            val = self.i
            self.i += 1
            return val
        else:
            raise StopIteration
for x in MyRange(3): print(x)
```

## 5. Decorators (Basic)

Decorators modify function behavior.

```python
def log(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper
@log
def add(a, b): return a + b
```

## 6. Sorting and Searching Algorithms

### Sorting
- Built-in: `sorted()`, `.sort()`
- Custom: Use `key` argument

```python
lst = [5,2,9,1]
sorted_lst = sorted(lst)
sorted_by_abs = sorted(lst, key=abs)
```

### Searching
- Linear search: O(n)
- Binary search: O(log n) (requires sorted list)

```python
def binary_search(arr, target):
    left, right = 0, len(arr)-1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

## 7. Time and Space Complexity

Big O notation describes algorithm efficiency:
- O(1): Constant time
- O(n): Linear time
- O(log n): Logarithmic time
- O(n^2): Quadratic time

Analyze both time and space for optimal solutions.

## 8. Recursion and Dynamic Programming

### Recursion
A function calls itself to solve subproblems.

```python
def factorial(n):
    if n == 0: return 1
    return n * factorial(n-1)
```

### Dynamic Programming
Store results of subproblems to avoid recomputation.

```python
def fib(n, memo={}):
    if n in memo: return memo[n]
    if n <= 1: return n
    memo[n] = fib(n-1, memo) + fib(n-2, memo)
    return memo[n]
```

## 9. Practical Examples

### Example: Word Frequency Counter
```python
from collections import Counter
text = "the quick brown fox jumps over the lazy dog"
words = text.split()
freq = Counter(words)
print(freq)
```

### Example: Custom Sort
```python
students = [
    {"name": "Alice", "score": 88},
    {"name": "Bob", "score": 95},
    {"name": "Charlie", "score": 78}
]
sorted_students = sorted(students, key=lambda x: x["score"], reverse=True)
print(sorted_students)
```

## 10. Review Questions
1. What is the purpose of the `collections` module?
2. How do comprehensions improve code clarity?
3. What is the difference between a generator and an iterator?
4. Describe the time complexity of binary search.
5. How does dynamic programming optimize recursive algorithms?

## 11. Answers to Review Questions
1. **It provides specialized data structures for efficient data management.**
2. **They allow concise, readable creation of lists, dicts, and sets.**
3. **A generator yields items one at a time; an iterator implements `__iter__` and `__next__`.**
4. **O(log n), much faster than linear search for sorted lists.**
5. **By storing subproblem results, it avoids redundant computation and improves efficiency.**
