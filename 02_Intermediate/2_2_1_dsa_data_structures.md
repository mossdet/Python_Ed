# 2.2.1 Advanced Data Structures

## Table of Contents
- [Collections Module](#collections-module)
- [Comprehensions](#comprehensions)
- [Generators and Iterators](#generators-and-iterators)
- [Decorators (Basic Understanding)](#decorators-basic-understanding)
- [Practical Examples](#practical-examples)
- [Performance Considerations](#performance-considerations)
- [Best Practices](#best-practices)
- [Exercises](#exercises)

---

## Collections Module

The `collections` module in Python provides specialized container datatypes that extend the functionality of built-in types like `dict`, `list`, `set`, and `tuple`. These data structures are optimized for specific use cases and offer enhanced performance and functionality.

### Counter

`Counter` is a subclass of `dict` designed for counting hashable objects. It's particularly useful for frequency analysis and statistical operations.

#### Theoretical Foundation
A Counter maintains a dictionary where elements are stored as keys and their counts as values. Internally, it uses a hash table implementation, providing O(1) average case access time for count operations.

```python
from collections import Counter

# Basic usage
text = "hello world"
char_count = Counter(text)
print(char_count)  # Counter({'l': 3, 'o': 2, 'h': 1, 'e': 1, ' ': 1, 'w': 1, 'r': 1, 'd': 1})

# Working with lists
numbers = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
num_count = Counter(numbers)
print(num_count)  # Counter({4: 4, 3: 3, 2: 2, 1: 1})

# Most common elements
print(num_count.most_common(2))  # [(4, 4), (3, 3)]

# Counter arithmetic
counter1 = Counter({'a': 3, 'b': 1})
counter2 = Counter({'a': 1, 'b': 2, 'c': 1})

print(counter1 + counter2)  # Counter({'a': 4, 'b': 3, 'c': 1})
print(counter1 - counter2)  # Counter({'a': 2})
print(counter1 & counter2)  # Counter({'a': 1, 'b': 1}) - intersection
print(counter1 | counter2)  # Counter({'a': 3, 'b': 2, 'c': 1}) - union
```

#### Advanced Counter Operations

```python
# Update counter
counter = Counter({'a': 1, 'b': 2})
counter.update({'a': 2, 'c': 1})
print(counter)  # Counter({'a': 3, 'b': 2, 'c': 1})

# Subtract counts
counter.subtract({'a': 1, 'b': 1})
print(counter)  # Counter({'a': 2, 'b': 1, 'c': 1})

# Elements method (returns iterator)
counter = Counter({'a': 2, 'b': 1})
print(list(counter.elements()))  # ['a', 'a', 'b']

# Total count
print(sum(counter.values()))  # 3
```

### defaultdict

`defaultdict` is a subclass of `dict` that provides a default value for missing keys. This eliminates the need for checking key existence and reduces code complexity.

#### Theoretical Foundation
The defaultdict uses a factory function to generate default values. When a missing key is accessed, the factory function is called with no arguments to provide a default value, which is then inserted into the dictionary.

```python
from collections import defaultdict

# Basic usage with list
dd_list = defaultdict(list)
dd_list['colors'].append('red')
dd_list['colors'].append('blue')
print(dd_list)  # defaultdict(<class 'list'>, {'colors': ['red', 'blue']})

# With int (useful for counting)
dd_int = defaultdict(int)
text = "hello"
for char in text:
    dd_int[char] += 1
print(dd_int)  # defaultdict(<class 'int'>, {'h': 1, 'e': 1, 'l': 2, 'o': 1})

# With set
dd_set = defaultdict(set)
dd_set['fruits'].add('apple')
dd_set['fruits'].add('banana')
print(dd_set)  # defaultdict(<class 'set'>, {'fruits': {'apple', 'banana'}})

# Custom factory function
def default_factory():
    return "N/A"

dd_custom = defaultdict(default_factory)
dd_custom['existing'] = 'value'
print(dd_custom['existing'])     # 'value'
print(dd_custom['non_existing']) # 'N/A'
```

#### Advanced defaultdict Usage

```python
# Nested defaultdict
nested_dd = defaultdict(lambda: defaultdict(int))
nested_dd['level1']['level2'] = 42
print(nested_dd)  # defaultdict(<function <lambda> at 0x...>, {'level1': defaultdict(<class 'int'>, {'level2': 42})})

# Converting to regular dict
regular_dict = dict(dd_int)
print(type(regular_dict))  # <class 'dict'>

# Grouping data
from collections import defaultdict

students = [
    ('Alice', 'Math', 95),
    ('Bob', 'Math', 87),
    ('Alice', 'Science', 92),
    ('Bob', 'Science', 88),
    ('Charlie', 'Math', 91)
]

grades_by_student = defaultdict(list)
for name, subject, grade in students:
    grades_by_student[name].append((subject, grade))

print(dict(grades_by_student))
# {'Alice': [('Math', 95), ('Science', 92)], 'Bob': [('Math', 87), ('Science', 88)], 'Charlie': [('Math', 91)]}
```

### deque (Double-ended queue)

`deque` (pronounced "deck") is a generalization of stacks and queues that supports efficient addition and removal of elements from both ends.

#### Theoretical Foundation
A deque is implemented as a doubly-linked list of blocks, where each block contains multiple elements. This design provides O(1) time complexity for append and pop operations at both ends, making it superior to lists for queue-like operations.

```python
from collections import deque

# Basic operations
dq = deque([1, 2, 3, 4, 5])
print(dq)  # deque([1, 2, 3, 4, 5])

# Append operations
dq.append(6)        # Add to right
dq.appendleft(0)    # Add to left
print(dq)  # deque([0, 1, 2, 3, 4, 5, 6])

# Pop operations
right_element = dq.pop()        # Remove from right
left_element = dq.popleft()     # Remove from left
print(f"Removed: {left_element}, {right_element}")  # Removed: 0, 6
print(dq)  # deque([1, 2, 3, 4, 5])

# Rotation
dq.rotate(2)    # Rotate right by 2
print(dq)  # deque([4, 5, 1, 2, 3])

dq.rotate(-1)   # Rotate left by 1
print(dq)  # deque([5, 1, 2, 3, 4])
```

#### Advanced deque Operations

```python
# Maximum length deque (circular buffer)
circular_buffer = deque(maxlen=3)
for i in range(5):
    circular_buffer.append(i)
    print(f"Added {i}: {circular_buffer}")

# Output:
# Added 0: deque([0], maxlen=3)
# Added 1: deque([0, 1], maxlen=3)
# Added 2: deque([0, 1, 2], maxlen=3)
# Added 3: deque([1, 2, 3], maxlen=3)
# Added 4: deque([2, 3, 4], maxlen=3)

# Extending deque
dq = deque([1, 2, 3])
dq.extend([4, 5])           # Extend right
dq.extendleft([0, -1])      # Extend left (note: order is reversed)
print(dq)  # deque([-1, 0, 1, 2, 3, 4, 5])

# Other useful methods
dq = deque([1, 2, 3, 2, 4])
print(dq.count(2))          # 2
print(dq.index(3))          # 2
dq.reverse()
print(dq)                   # deque([4, 2, 3, 2, 1])
```

---

## Comprehensions

Comprehensions provide a concise way to create sequences in Python. They follow the mathematical set-builder notation and are both readable and efficient.

### List Comprehensions

#### Theoretical Foundation
List comprehensions create a new list by applying an expression to each element of an iterable, optionally filtering elements based on a condition. They are implemented using the same underlying mechanism as generator expressions but immediately evaluate to create a list in memory.

```python
# Basic syntax: [expression for item in iterable]
squares = [x**2 for x in range(10)]
print(squares)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# With condition: [expression for item in iterable if condition]
even_squares = [x**2 for x in range(10) if x % 2 == 0]
print(even_squares)  # [0, 4, 16, 36, 64]

# Multiple conditions
filtered_squares = [x**2 for x in range(20) if x % 2 == 0 if x > 5]
print(filtered_squares)  # [36, 64, 100, 144, 196, 256, 324]

# String processing
words = ['hello', 'world', 'python', 'programming']
uppercase_words = [word.upper() for word in words if len(word) > 5]
print(uppercase_words)  # ['PYTHON', 'PROGRAMMING']
```

#### Advanced List Comprehensions

```python
# Nested comprehensions
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [num for row in matrix for num in row]
print(flattened)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Creating matrix
matrix_2d = [[i*j for j in range(3)] for i in range(3)]
print(matrix_2d)  # [[0, 0, 0], [0, 1, 2], [0, 2, 4]]

# Conditional expressions within comprehension
numbers = [1, -2, 3, -4, 5]
processed = [abs(x) if x < 0 else x**2 for x in numbers]
print(processed)  # [1, 2, 9, 4, 25]

# Complex transformations
data = [{'name': 'Alice', 'age': 25}, {'name': 'Bob', 'age': 30}]
names = [person['name'].upper() for person in data if person['age'] > 25]
print(names)  # ['BOB']
```

### Dictionary Comprehensions

```python
# Basic syntax: {key_expr: value_expr for item in iterable}
squares_dict = {x: x**2 for x in range(5)}
print(squares_dict)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# With condition
even_squares_dict = {x: x**2 for x in range(10) if x % 2 == 0}
print(even_squares_dict)  # {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# Transforming existing dictionary
original = {'a': 1, 'b': 2, 'c': 3}
transformed = {k.upper(): v**2 for k, v in original.items()}
print(transformed)  # {'A': 1, 'B': 4, 'C': 9}

# Filtering dictionary
scores = {'Alice': 95, 'Bob': 87, 'Charlie': 92, 'David': 78}
high_scores = {name: score for name, score in scores.items() if score > 85}
print(high_scores)  # {'Alice': 95, 'Bob': 87, 'Charlie': 92}

# Swapping keys and values
original = {'a': 1, 'b': 2, 'c': 3}
swapped = {v: k for k, v in original.items()}
print(swapped)  # {1: 'a', 2: 'b', 3: 'c'}
```

### Set Comprehensions

```python
# Basic syntax: {expression for item in iterable}
squares_set = {x**2 for x in range(10)}
print(squares_set)  # {0, 1, 4, 9, 16, 25, 36, 49, 64, 81}

# Automatic deduplication
text = "hello world"
unique_chars = {char.upper() for char in text if char.isalpha()}
print(unique_chars)  # {'E', 'H', 'L', 'O', 'R', 'W', 'D'}

# Complex filtering
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
prime_like = {x for x in numbers if all(x % i != 0 for i in range(2, int(x**0.5) + 1)) and x > 1}
print(prime_like)  # {2, 3, 5, 7}
```

---

## Generators and Iterators

Generators and iterators are fundamental concepts in Python that enable lazy evaluation and memory-efficient processing of large datasets.

### Iterator Protocol

#### Theoretical Foundation
An iterator is an object that implements the iterator protocol, consisting of `__iter__()` and `__next__()` methods. The iterator protocol enables objects to be used in loops and supports lazy evaluation, processing one item at a time rather than loading entire sequences into memory.

```python
# Understanding the iterator protocol
class NumberIterator:
    def __init__(self, max_value):
        self.max_value = max_value
        self.current = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current < self.max_value:
            current_value = self.current
            self.current += 1
            return current_value
        else:
            raise StopIteration

# Using custom iterator
numbers = NumberIterator(5)
for num in numbers:
    print(num)  # Prints 0, 1, 2, 3, 4

# Manual iteration
numbers = NumberIterator(3)
iterator = iter(numbers)
print(next(iterator))  # 0
print(next(iterator))  # 1
print(next(iterator))  # 2
# print(next(iterator))  # Would raise StopIteration
```

### Generator Functions

#### Theoretical Foundation
Generator functions use the `yield` keyword to produce values on-demand. When a generator function is called, it returns a generator object without executing the function body. The function executes only when `next()` is called, pausing at each `yield` and resuming from that point on subsequent calls.

```python
# Basic generator function
def simple_generator():
    yield 1
    yield 2
    yield 3

gen = simple_generator()
print(next(gen))  # 1
print(next(gen))  # 2
print(next(gen))  # 3

# Generator with parameters
def countdown(n):
    while n > 0:
        yield n
        n -= 1

for num in countdown(5):
    print(num)  # Prints 5, 4, 3, 2, 1

# Infinite generator
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
print([next(fib) for _ in range(10)])  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

#### Advanced Generator Patterns

```python
# Generator expressions
squares_gen = (x**2 for x in range(10))
print(type(squares_gen))  # <class 'generator'>
print(list(squares_gen))  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Generator with state
def stateful_generator():
    state = {'count': 0}
    
    def inner():
        while True:
            state['count'] += 1
            yield f"Call #{state['count']}"
    
    return inner()

gen = stateful_generator()
print(next(gen))  # Call #1
print(next(gen))  # Call #2

# Generator pipeline
def read_data():
    for i in range(10):
        yield f"data_{i}"

def process_data(data_gen):
    for item in data_gen:
        yield item.upper()

def filter_data(data_gen):
    for item in data_gen:
        if '5' in item:
            yield item

# Pipeline composition
pipeline = filter_data(process_data(read_data()))
print(list(pipeline))  # ['DATA_5']
```

### Advanced Generator Techniques

```python
# Generator with send() method
def coroutine_example():
    value = None
    while True:
        value = yield value
        if value is not None:
            value = value * 2

gen = coroutine_example()
next(gen)  # Prime the generator
print(gen.send(5))   # 10
print(gen.send(3))   # 6

# Generator with exception handling
def robust_generator():
    try:
        for i in range(5):
            yield i
    except GeneratorExit:
        print("Generator is being closed")
    finally:
        print("Cleanup operations")

gen = robust_generator()
print(next(gen))  # 0
print(next(gen))  # 1
gen.close()       # Triggers GeneratorExit

# yield from for delegation
def sub_generator():
    yield 'A'
    yield 'B'
    yield 'C'

def main_generator():
    yield from sub_generator()
    yield 'D'

gen = main_generator()
print(list(gen))  # ['A', 'B', 'C', 'D']
```

---

## Decorators (Basic Understanding)

Decorators are a powerful and elegant feature in Python that allow you to modify, extend, or wrap the behavior of functions or classes without permanently altering their source code. They represent a form of metaprogramming that enables clean separation of concerns and code reusability.

### What Are Decorators?

A decorator is essentially a callable (function, method, or class) that takes another function as input and returns a modified or enhanced version of that function. The key insight is that decorators allow you to add functionality to existing code in a modular and reusable way.

**Core Concept**: Decorators implement the **Decorator Pattern** from software engineering, which allows behavior to be added to objects dynamically without altering their structure.

### How Do Decorators Work?

#### The Mechanism
When you use the `@decorator` syntax, Python performs the following transformation:

```python
@decorator
def function():
    pass

# Is equivalent to:
def function():
    pass
function = decorator(function)
```

This means the decorator receives the original function as an argument and returns a new function (or the same function with modifications).

#### Execution Flow
1. **Definition Time**: The decorator is applied when the function is defined, not when it's called
2. **Wrapper Creation**: The decorator typically creates a wrapper function that contains the enhanced behavior
3. **Function Replacement**: The original function name now refers to the wrapper function
4. **Runtime Execution**: When the decorated function is called, the wrapper executes, which may call the original function

### Why Use Decorators?

#### 1. **Separation of Concerns**
Decorators allow you to separate core business logic from cross-cutting concerns like:
- Logging
- Authentication
- Caching
- Performance monitoring
- Input validation

#### 2. **Code Reusability**
Once written, a decorator can be applied to multiple functions without code duplication.

#### 3. **Clean Code**
Decorators keep your function definitions clean and focused on their primary purpose.

#### 4. **Non-Intrusive Enhancement**
You can add functionality without modifying the original function's source code.

### When Are Decorators Used?

#### Common Use Cases:

1. **Logging and Debugging**
   ```python
   @log_calls
   def important_function():
       # Function logic
   ```

2. **Authentication and Authorization**
   ```python
   @require_login
   def admin_panel():
       # Admin functionality
   ```

3. **Caching/Memoization**
   ```python
   @cache_result
   def expensive_computation():
       # Heavy computation
   ```

4. **Rate Limiting**
   ```python
   @rate_limit(calls_per_minute=60)
   def api_endpoint():
       # API logic
   ```

5. **Input Validation**
   ```python
   @validate_input(int, str)
   def process_data(number, text):
       # Processing logic
   ```

6. **Performance Monitoring**
   ```python
   @measure_time
   def database_query():
       # Database operations
   ```

### Theoretical Foundation

#### Function Objects and First-Class Citizens
In Python, functions are first-class objects, meaning they can be:
- Assigned to variables
- Passed as arguments to other functions
- Returned from functions
- Stored in data structures

This first-class nature is what makes decorators possible.

```python
# Functions as objects
def greet(name):
    return f"Hello, {name}!"

# Assign function to variable
my_function = greet
print(my_function("World"))  # Hello, World!

# Pass function as argument
def call_function(func, arg):
    return func(arg)

result = call_function(greet, "Python")
print(result)  # Hello, Python!

# Return function from function
def create_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

double = create_multiplier(2)
print(double(5))  # 10
```

### Understanding Closures - The Foundation of Decorators

Closures are essential to understanding how decorators work. A closure occurs when a nested function references variables from its enclosing scope.

```python
def outer_function(message):
    def inner_function():
        print(message)  # Accesses variable from outer scope
    return inner_function

# Create a closure
my_closure = outer_function("Hello from closure!")
my_closure()  # Hello from closure!

# The inner function "remembers" the message variable
# even after outer_function has finished executing
```

### Step-by-Step Decorator Construction

Let's build a decorator from scratch to understand the mechanics:

#### Step 1: Basic Function Wrapper
```python
def basic_wrapper(func):
    def wrapper():
        print("Before function execution")
        result = func()
        print("After function execution")
        return result
    return wrapper

# Manual decoration
def say_hello():
    print("Hello!")

say_hello = basic_wrapper(say_hello)
say_hello()
```

#### Step 2: Handling Arguments
```python
def flexible_wrapper(func):
    def wrapper(*args, **kwargs):  # Accept any arguments
        print(f"Calling {func.__name__} with args: {args}, kwargs: {kwargs}")
        result = func(*args, **kwargs)
        print(f"Function {func.__name__} returned: {result}")
        return result
    return wrapper

@flexible_wrapper
def add_numbers(a, b):
    return a + b

result = add_numbers(5, 3)
```

#### Step 3: Preserving Function Metadata
```python
import functools

def proper_decorator(func):
    @functools.wraps(func)  # Preserves original function's metadata
    def wrapper(*args, **kwargs):
        print(f"Executing {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@proper_decorator
def example_function():
    """This is an example function."""
    pass

print(example_function.__name__)  # example_function
print(example_function.__doc__)   # This is an example function.
```

### Types of Decorators

#### 1. Function Decorators
```python
def timing_decorator(func):
    import time
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timing_decorator
def slow_function():
    import time
    time.sleep(1)
    return "Complete"
```

#### 2. Parameterized Decorators
```python
def repeat_decorator(times):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for i in range(times):
                if i == times - 1:  # Return result only on last iteration
                    return func(*args, **kwargs)
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat_decorator(3)
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")  # Prints "Hello, Alice!" three times
```

#### 3. Class-Based Decorators
```python
class CallCounter:
    def __init__(self, func):
        self.func = func
        self.count = 0
        functools.update_wrapper(self, func)
    
    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call #{self.count} to {self.func.__name__}")
        return self.func(*args, **kwargs)
    
    def get_count(self):
        return self.count

@CallCounter
def test_function():
    return "Test complete"

test_function()  # Call #1 to test_function
test_function()  # Call #2 to test_function
print(f"Total calls: {test_function.get_count()}")  # Total calls: 2
```

### Advanced Decorator Patterns

#### 1. Decorator with State
```python
def stateful_decorator(func):
    state = {'calls': 0, 'total_time': 0}
    
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        
        state['calls'] += 1
        state['total_time'] += (end - start)
        
        print(f"Call #{state['calls']}, "
              f"Average time: {state['total_time'] / state['calls']:.4f}s")
        return result
    
    wrapper.get_stats = lambda: state.copy()
    return wrapper

@stateful_decorator
def sample_function():
    import time
    time.sleep(0.1)
    return "Done"

sample_function()
sample_function()
print(sample_function.get_stats())
```

#### 2. Conditional Decorator
```python
def conditional_decorator(condition):
    def decorator(func):
        if condition:
            @functools.wraps(func)
            def wrapper(*args, **kwargs):
                print(f"Condition met: executing {func.__name__}")
                return func(*args, **kwargs)
            return wrapper
        else:
            return func  # Return original function unchanged
    return decorator

DEBUG = True

@conditional_decorator(DEBUG)
def debug_function():
    print("This function runs with debug info")

debug_function()  # Will print debug info if DEBUG is True
```

### Real-World Decorator Examples

#### 1. Authentication Decorator
```python
def require_auth(permission_level):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            # Simulate getting current user
            current_user = get_current_user()  # Hypothetical function
            
            if not current_user:
                raise PermissionError("Authentication required")
            
            if current_user.permission_level < permission_level:
                raise PermissionError("Insufficient permissions")
            
            return func(*args, **kwargs)
        return wrapper
    return decorator

@require_auth(permission_level=5)
def admin_function():
    return "Admin operation completed"
```

#### 2. Retry Decorator
```python
def retry(max_attempts=3, delay=1):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            import time
            
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise e
                    print(f"Attempt {attempt + 1} failed: {e}. Retrying...")
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(max_attempts=3, delay=0.5)
def unreliable_function():
    import random
    if random.random() < 0.7:  # 70% chance of failure
        raise ConnectionError("Network error")
    return "Success!"
```

#### 3. Caching with Expiration
```python
def cache_with_expiry(expiry_seconds=60):
    def decorator(func):
        import time
        cache = {}
        
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            key = str(args) + str(sorted(kwargs.items()))
            current_time = time.time()
            
            if key in cache:
                result, timestamp = cache[key]
                if current_time - timestamp < expiry_seconds:
                    print(f"Cache hit for {func.__name__}")
                    return result
                else:
                    print(f"Cache expired for {func.__name__}")
                    del cache[key]
            
            print(f"Cache miss for {func.__name__}")
            result = func(*args, **kwargs)
            cache[key] = (result, current_time)
            return result
        
        wrapper.clear_cache = lambda: cache.clear()
        return wrapper
    return decorator

@cache_with_expiry(expiry_seconds=5)
def expensive_computation(n):
    import time
    time.sleep(1)  # Simulate expensive operation
    return n ** 2
```

### Decorator Chaining

Multiple decorators can be applied to a single function:

```python
@timer
@cache
@validate_input(int)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Equivalent to:
# fibonacci = timer(cache(validate_input(int)(fibonacci)))
```

### Best Practices for Decorator Usage

#### 1. Always Use `functools.wraps`
```python
import functools

def my_decorator(func):
    @functools.wraps(func)  # Preserves function metadata
    def wrapper(*args, **kwargs):
        # Decorator logic
        return func(*args, **kwargs)
    return wrapper
```

#### 2. Handle Exceptions Properly
```python
def safe_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except Exception as e:
            print(f"Error in {func.__name__}: {e}")
            # Decide whether to re-raise or handle gracefully
            raise
    return wrapper
```

#### 3. Make Decorators Configurable
```python
def configurable_decorator(param1=None, param2=None):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            # Use param1 and param2 in decorator logic
            return func(*args, **kwargs)
        return wrapper
    return decorator

# Can be used with or without parameters
@configurable_decorator()
def func1():
    pass

@configurable_decorator(param1="value")
def func2():
    pass
```

### Common Pitfalls and Solutions

#### 1. Losing Function Metadata
```python
# Problem: Without functools.wraps
def bad_decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@bad_decorator
def my_function():
    """Important documentation"""
    pass

print(my_function.__name__)  # wrapper (not my_function)
print(my_function.__doc__)   # None (lost documentation)

# Solution: Use functools.wraps
def good_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```

#### 2. Accessing Decorated Function
```python
def my_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    
    # Store reference to original function
    wrapper.__wrapped__ = func
    return wrapper

@my_decorator
def my_function():
    pass

# Access original function
original = my_function.__wrapped__
```

### Performance Considerations

Decorators add a small overhead to function calls. For performance-critical code:

```python
# Minimize work in wrapper
def efficient_decorator(func):
    # Do expensive setup once, outside the wrapper
    setup_data = expensive_setup()
    
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        # Minimal work in wrapper
        return func(*args, **kwargs)
    return wrapper
```

### Basic Decorators

```python
# Simple decorator
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Something is happening before the function is called.
# Hello!
# Something is happening after the function is called.

# Decorator with arguments
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

greet("World")
# Output:
# Hello, World!
# Hello, World!
# Hello, World!
```

### Advanced Decorator Patterns

```python
import functools
import time

# Timing decorator
def timing_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} took {end_time - start_time:.4f} seconds")
        return result
    return wrapper

@timing_decorator
def slow_function():
    time.sleep(1)
    return "Done"

result = slow_function()
# Output: slow_function took 1.0012 seconds

# Memoization decorator
def memoize(func):
    cache = {}
    
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        key = str(args) + str(sorted(kwargs.items()))
        if key not in cache:
            cache[key] = func(*args, **kwargs)
        return cache[key]
    return wrapper

@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(10))  # Much faster due to memoization

# Validation decorator
def validate_types(*types):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for i, (arg, expected_type) in enumerate(zip(args, types)):
                if not isinstance(arg, expected_type):
                    raise TypeError(f"Argument {i} must be of type {expected_type.__name__}")
            return func(*args, **kwargs)
        return wrapper
    return decorator

@validate_types(int, str)
def process_data(number, text):
    return f"Processing {number}: {text}"

print(process_data(42, "Hello"))  # Works fine
# process_data("42", "Hello")  # Would raise TypeError
```

### Class Decorators

```python
# Class-based decorator
class CountCalls:
    def __init__(self, func):
        self.func = func
        self.count = 0
    
    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call #{self.count} to {self.func.__name__}")
        return self.func(*args, **kwargs)

@CountCalls
def say_hello():
    print("Hello!")

say_hello()  # Call #1 to say_hello
say_hello()  # Call #2 to say_hello

# Property decorator
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def radius(self):
        return self._radius
    
    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value
    
    @property
    def area(self):
        return 3.14159 * self._radius ** 2

circle = Circle(5)
print(circle.area)  # 78.53975
circle.radius = 3
print(circle.area)  # 28.27431
```

---

## Practical Examples

### Example 1: Log Analysis System

```python
from collections import defaultdict, Counter, deque
import re
from datetime import datetime

class LogAnalyzer:
    def __init__(self):
        self.logs_by_level = defaultdict(list)
        self.error_counts = Counter()
        self.recent_errors = deque(maxlen=100)  # Keep last 100 errors
    
    def parse_log_line(self, line):
        # Example log format: "2023-07-15 10:30:45 [ERROR] Database connection failed"
        pattern = r'(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) \[(\w+)\] (.+)'
        match = re.match(pattern, line)
        if match:
            timestamp, level, message = match.groups()
            return {
                'timestamp': datetime.strptime(timestamp, '%Y-%m-%d %H:%M:%S'),
                'level': level,
                'message': message
            }
        return None
    
    def process_logs(self, log_lines):
        for line in log_lines:
            log_entry = self.parse_log_line(line)
            if log_entry:
                self.logs_by_level[log_entry['level']].append(log_entry)
                
                if log_entry['level'] == 'ERROR':
                    self.error_counts[log_entry['message']] += 1
                    self.recent_errors.append(log_entry)
    
    def get_error_summary(self):
        return {
            'total_errors': sum(self.error_counts.values()),
            'most_common_errors': self.error_counts.most_common(5),
            'recent_errors': list(self.recent_errors)[-10:]  # Last 10 errors
        }
    
    def get_logs_by_level(self, level):
        return self.logs_by_level.get(level, [])

# Usage
analyzer = LogAnalyzer()
sample_logs = [
    "2023-07-15 10:30:45 [ERROR] Database connection failed",
    "2023-07-15 10:31:00 [INFO] User logged in",
    "2023-07-15 10:31:15 [ERROR] Database connection failed",
    "2023-07-15 10:31:30 [WARNING] Low disk space",
    "2023-07-15 10:31:45 [ERROR] API timeout"
]

analyzer.process_logs(sample_logs)
print(analyzer.get_error_summary())
```

### Example 2: Data Processing Pipeline

```python
import json
from collections import defaultdict

# Generator-based data processing pipeline
def read_json_data(filename):
    """Generator that reads JSON data line by line"""
    with open(filename, 'r') as file:
        for line in file:
            try:
                yield json.loads(line)
            except json.JSONDecodeError:
                continue

def filter_data(data_gen, condition):
    """Filter data based on condition"""
    for item in data_gen:
        if condition(item):
            yield item

def transform_data(data_gen, transform_func):
    """Transform each data item"""
    for item in data_gen:
        yield transform_func(item)

def aggregate_data(data_gen, key_func, value_func):
    """Aggregate data by key"""
    aggregated = defaultdict(list)
    for item in data_gen:
        key = key_func(item)
        value = value_func(item)
        aggregated[key].append(value)
    return dict(aggregated)

# Usage example
def process_user_data():
    # Simulated data processing
    data = [
        {"user_id": 1, "action": "login", "timestamp": "2023-07-15T10:00:00"},
        {"user_id": 2, "action": "purchase", "timestamp": "2023-07-15T10:05:00", "amount": 50.0},
        {"user_id": 1, "action": "logout", "timestamp": "2023-07-15T10:10:00"},
        {"user_id": 3, "action": "purchase", "timestamp": "2023-07-15T10:15:00", "amount": 75.0},
    ]
    
    # Generator pipeline
    def data_generator():
        for item in data:
            yield item
    
    # Filter purchases only
    purchase_data = filter_data(
        data_generator(),
        lambda x: x.get('action') == 'purchase'
    )
    
    # Transform to extract relevant info
    purchase_info = transform_data(
        purchase_data,
        lambda x: {
            'user_id': x['user_id'],
            'amount': x['amount'],
            'date': x['timestamp'][:10]
        }
    )
    
    # Aggregate by user
    result = aggregate_data(
        purchase_info,
        lambda x: x['user_id'],
        lambda x: x['amount']
    )
    
    return result

print(process_user_data())  # {2: [50.0], 3: [75.0]}
```

### Example 3: Caching Decorator with Statistics

```python
import functools
import time
from collections import defaultdict

class CacheStats:
    def __init__(self):
        self.hits = 0
        self.misses = 0
        self.call_times = []
    
    def hit(self):
        self.hits += 1
    
    def miss(self):
        self.misses += 1
    
    def add_call_time(self, duration):
        self.call_times.append(duration)
    
    def get_stats(self):
        total_calls = self.hits + self.misses
        hit_rate = self.hits / total_calls if total_calls > 0 else 0
        avg_time = sum(self.call_times) / len(self.call_times) if self.call_times else 0
        
        return {
            'total_calls': total_calls,
            'hits': self.hits,
            'misses': self.misses,
            'hit_rate': hit_rate,
            'average_call_time': avg_time
        }

def advanced_cache(maxsize=128):
    def decorator(func):
        cache = {}
        stats = CacheStats()
        access_order = deque()
        
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            start_time = time.time()
            
            # Create cache key
            key = str(args) + str(sorted(kwargs.items()))
            
            if key in cache:
                stats.hit()
                # Move to end (most recently used)
                access_order.remove(key)
                access_order.append(key)
                result = cache[key]
            else:
                stats.miss()
                result = func(*args, **kwargs)
                
                # If cache is full, remove least recently used
                if len(cache) >= maxsize:
                    lru_key = access_order.popleft()
                    del cache[lru_key]
                
                cache[key] = result
                access_order.append(key)
            
            end_time = time.time()
            stats.add_call_time(end_time - start_time)
            
            return result
        
        wrapper.cache_stats = stats
        wrapper.clear_cache = lambda: (cache.clear(), access_order.clear())
        
        return wrapper
    return decorator

@advanced_cache(maxsize=3)
def expensive_function(n):
    time.sleep(0.1)  # Simulate expensive operation
    return n ** 2

# Test the cached function
print(expensive_function(2))  # Cache miss
print(expensive_function(3))  # Cache miss
print(expensive_function(2))  # Cache hit
print(expensive_function(4))  # Cache miss
print(expensive_function(5))  # Cache miss (evicts least recently used)
print(expensive_function(2))  # Cache miss (was evicted)

print(expensive_function.cache_stats.get_stats())
```

---

## Performance Considerations

### Memory Efficiency

```python
import sys

# List vs Generator memory usage
def memory_comparison():
    # List comprehension - creates entire list in memory
    list_comp = [x**2 for x in range(1000000)]
    print(f"List size: {sys.getsizeof(list_comp)} bytes")
    
    # Generator expression - creates generator object only
    gen_exp = (x**2 for x in range(1000000))
    print(f"Generator size: {sys.getsizeof(gen_exp)} bytes")
    
    # Memory usage difference is significant
    print(f"Memory ratio: {sys.getsizeof(list_comp) / sys.getsizeof(gen_exp):.2f}x")

memory_comparison()
```

### Time Complexity Analysis

```python
import time
from collections import deque, defaultdict

def performance_comparison():
    # List vs deque for queue operations
    def test_list_queue(n):
        queue = []
        start = time.time()
        for i in range(n):
            queue.append(i)
        for i in range(n):
            queue.pop(0)  # O(n) operation
        return time.time() - start
    
    def test_deque_queue(n):
        queue = deque()
        start = time.time()
        for i in range(n):
            queue.append(i)
        for i in range(n):
            queue.popleft()  # O(1) operation
        return time.time() - start
    
    n = 10000
    list_time = test_list_queue(n)
    deque_time = test_deque_queue(n)
    
    print(f"List queue time: {list_time:.4f}s")
    print(f"Deque queue time: {deque_time:.4f}s")
    print(f"Speedup: {list_time / deque_time:.2f}x")

performance_comparison()
```

---

## Best Practices

### 1. Choose the Right Data Structure

```python
# Use Counter for frequency counting
from collections import Counter

# Good
def count_words(text):
    return Counter(text.split())

# Less efficient
def count_words_manual(text):
    word_count = {}
    for word in text.split():
        word_count[word] = word_count.get(word, 0) + 1
    return word_count

# Use defaultdict to avoid key existence checks
from collections import defaultdict

# Good
def group_by_length(words):
    groups = defaultdict(list)
    for word in words:
        groups[len(word)].append(word)
    return groups

# Less clean
def group_by_length_manual(words):
    groups = {}
    for word in words:
        length = len(word)
        if length not in groups:
            groups[length] = []
        groups[length].append(word)
    return groups
```

### 2. Use Generators for Large Data Sets

```python
# Good - Memory efficient
def process_large_file(filename):
    with open(filename, 'r') as file:
        for line in file:
            yield line.strip().upper()

# Less efficient - Loads entire file into memory
def process_large_file_list(filename):
    with open(filename, 'r') as file:
        return [line.strip().upper() for line in file]
```

### 3. Decorator Best Practices

```python
import functools

# Always use functools.wraps
def my_decorator(func):
    @functools.wraps(func)  # Preserves function metadata
    def wrapper(*args, **kwargs):
        # Decorator logic
        return func(*args, **kwargs)
    return wrapper

# Handle exceptions in decorators
def safe_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except Exception as e:
            print(f"Error in {func.__name__}: {e}")
            return None
    return wrapper
```

---

## Exercises

### Exercise 1: Log File Analyzer
Create a log analyzer that:
1. Uses `Counter` to count different log levels
2. Uses `defaultdict` to group logs by date
3. Uses `deque` with maxlen to keep recent errors
4. Provides a generator function to read large log files

### Exercise 2: Data Processing Pipeline
Build a data processing pipeline that:
1. Reads JSON data using generators
2. Filters data based on multiple conditions
3. Transforms data using comprehensions
4. Aggregates results using appropriate collections

### Exercise 3: Advanced Caching System
Implement a caching decorator that:
1. Supports LRU (Least Recently Used) eviction
2. Tracks cache statistics (hits, misses, hit rate)
3. Provides cache size monitoring
4. Includes cache clearing functionality

### Exercise 4: Text Analysis Tool
Create a text analysis tool that:
1. Uses comprehensions to process text data
2. Implements generators for memory-efficient processing
3. Uses collections for frequency analysis
4. Includes decorators for timing and memoization

---

## Summary

This section covered advanced data structures and concepts essential for intermediate Python programming:

1. **Collections Module**: Specialized containers (`Counter`, `defaultdict`, `deque`) that provide enhanced functionality and performance for specific use cases.

2. **Comprehensions**: Concise syntax for creating lists, dictionaries, and sets with optional filtering and transformation.

3. **Generators and Iterators**: Memory-efficient tools for lazy evaluation and processing large datasets.

4. **Decorators**: Powerful mechanism for modifying function behavior without changing the original code.

These concepts form the foundation for writing efficient, readable, and maintainable Python code. Understanding when and how to use each tool is crucial for developing professional-quality applications.

The next sections will build upon these concepts to explore algorithms, file handling, and more advanced programming patterns.
