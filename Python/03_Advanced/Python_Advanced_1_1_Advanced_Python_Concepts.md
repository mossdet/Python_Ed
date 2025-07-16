# Python Advanced 1.1: Advanced Python Concepts

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Advanced Functions and Decorators](#2-advanced-functions-and-decorators)
- [3. Concurrency and Parallelism](#3-concurrency-and-parallelism)
- [4. Review Questions](#4-review-questions)
- [5. Answers to Review Questions](#5-answers-to-review-questions)

---

## 1. Introduction

This module covers advanced Python concepts essential for high-level software engineering, including advanced function techniques, decorators, metaprogramming, and concurrency.

## 2. Advanced Functions and Decorators

### Closures and Higher-Order Functions
Closures are functions that capture variables from their enclosing scope.
```python
def make_multiplier(x):
    def multiplier(n):
        return x * n
    return multiplier
mul3 = make_multiplier(3)
print(mul3(10))  # 30
```
Higher-order functions take other functions as arguments or return them.

### Advanced Decorators
Decorators can take arguments or be applied to classes.
```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")
```

### Metaclasses and Descriptors
Metaclasses allow you to customize class creation.
```python
class Meta(type):
    def __new__(cls, name, bases, dct):
        dct['created_by_meta'] = True
        return super().__new__(cls, name, bases, dct)

class MyClass(metaclass=Meta):
    pass
print(MyClass.created_by_meta)  # True
```
Descriptors manage attribute access.
```python
class Descriptor:
    def __get__(self, instance, owner):
        return 'value from descriptor'
class MyClass:
    attr = Descriptor()
obj = MyClass()
print(obj.attr)
```

### Context Managers (Custom Implementation)
```python
class FileManager:
    def __init__(self, filename):
        self.filename = filename
    def __enter__(self):
        self.file = open(self.filename, 'r')
        return self.file
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
with FileManager('test.txt') as f:
    data = f.read()
```

## 3. Concurrency and Parallelism

### Threading and Multiprocessing
```python
import threading
import multiprocessing

def worker():
    print("Worker running")

# Threading
thread = threading.Thread(target=worker)
thread.start()
thread.join()

# Multiprocessing
process = multiprocessing.Process(target=worker)
process.start()
process.join()
```

### asyncio and Asynchronous Programming
```python
import asyncio
async def main():
    print("Hello")
    await asyncio.sleep(1)
    print("World")
asyncio.run(main())
```

### GIL (Global Interpreter Lock)
- The GIL allows only one thread to execute Python bytecode at a time.
- Use multiprocessing or async for CPU-bound and I/O-bound tasks, respectively.

### concurrent.futures
```python
from concurrent.futures import ThreadPoolExecutor, as_completed

def task(n):
    return n * n
with ThreadPoolExecutor(max_workers=4) as executor:
    futures = [executor.submit(task, i) for i in range(5)]
    for future in as_completed(futures):
        print(future.result())
```

## 4. Review Questions
1. What is a closure in Python?
2. How do you create a decorator that takes arguments?
3. What is a metaclass and when would you use one?
4. How does the GIL affect concurrency in Python?
5. What is the difference between threading and multiprocessing?

## 5. Answers to Review Questions
1. **A closure is a function that captures variables from its enclosing scope.**
2. **By defining a decorator factory that returns a decorator function.**
3. **A metaclass customizes class creation; use it for advanced class behaviors.**
4. **The GIL allows only one thread to execute Python bytecode at a time, limiting true parallelism.**
5. **Threading is for I/O-bound tasks (shares memory); multiprocessing is for CPU-bound tasks (separate memory).**
