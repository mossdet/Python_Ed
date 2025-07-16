# Python Advanced 3.1: Performance Optimization

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Profiling and Optimization](#2-profiling-and-optimization)
- [3. Speed Optimization Techniques](#3-speed-optimization-techniques)
- [4. Caching Strategies](#4-caching-strategies)
- [5. Advanced Performance Patterns](#5-advanced-performance-patterns)
- [6. Design Patterns](#6-design-patterns)
- [7. Review Questions](#7-review-questions)
- [8. Answers to Review Questions](#8-answers-to-review-questions)

---

## 1. Introduction

This module covers advanced techniques for profiling, optimizing, and scaling Python applications, including memory and speed optimization, caching, and design patterns for high-performance systems.

## 2. Profiling and Optimization

### cProfile and line_profiler
```python
import cProfile
import pstats

def slow_function():
    total = 0
    for i in range(10000):
        total += i ** 2
    return total

cProfile.run('slow_function()')
```
- Use `line_profiler` for line-by-line analysis.

### Memory Profiling
```python
from memory_profiler import profile
@profile
def my_func():
    a = [1] * (10**6)
    b = [2] * (2 * 10**7)
    del b
my_func()
```

### Cython and Numba
- Use Cython to compile Python code to C for speed.
- Use Numba for JIT compilation of numerical code.
```python
from numba import jit
@jit(nopython=True)
def fast_sum(arr):
    total = 0
    for x in arr:
        total += x
    return total
```

## 3. Speed Optimization Techniques
- Analyze algorithmic complexity (Big O notation).
- Choose optimal data structures (e.g., dict vs. list).
- Use generators and lazy evaluation for memory efficiency.
- Vectorize operations with NumPy.

## 4. Caching Strategies
- Use `functools.lru_cache` for memoization.
```python
from functools import lru_cache
@lru_cache(maxsize=128)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)
```
- Use Redis for distributed caching.
- Optimize database queries and use HTTP caching headers.

## 5. Advanced Performance Patterns
- Object pooling and reuse.
- Batch processing for I/O efficiency.
- Asynchronous processing for I/O-bound tasks.
- Parallel processing with multiprocessing.
- Vectorization with NumPy for large datasets.

## 6. Design Patterns
- Singleton, Factory, Observer, Strategy, Command, Adapter, Decorator.
- MVC and MVP architectures for scalable applications.

## 7. Review Questions
1. How do you profile the performance of a Python function?
2. What is the benefit of using Numba or Cython?
3. How does `lru_cache` improve performance?
4. When should you use batch processing?
5. Name two design patterns useful for high-performance Python systems.

## 8. Answers to Review Questions
1. **Use cProfile, line_profiler, or memory_profiler to analyze runtime and memory usage.**
2. **They compile Python code to machine code for significant speedups in numerical tasks.**
3. **It caches function results, avoiding redundant computation.**
4. **When processing large numbers of I/O operations to reduce overhead.**
5. **Singleton, Factory, Observer, Strategy, Command, Adapter, Decorator, MVC, MVP.**
