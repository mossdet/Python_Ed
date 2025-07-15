# Python Beginner 2.2: Functions

## Table of Contents
1. [Learning Objectives](#learning-objectives)
2. [Function Fundamentals](#1-function-fundamentals)
3. [Function Parameters and Arguments](#2-function-parameters-and-arguments)
4. [Return Statements](#3-return-statements)
5. [Variable Scope](#4-variable-scope)
6. [Lambda Functions](#5-lambda-functions)
7. [Advanced Function Concepts](#6-advanced-function-concepts)
8. [Function Documentation and Testing](#7-function-documentation-and-testing)
9. [Built-in Functions](#8-built-in-functions)
10. [Error Handling in Functions](#9-error-handling-in-functions)
11. [Performance and Best Practices](#10-performance-and-best-practices)
12. [Review Questions](#review-questions)
13. [Practical Exercises](#practical-exercises)
14. [Next Steps](#next-steps)
15. [Additional Resources](#additional-resources)

## Learning Objectives
By the end of this module, you will be able to:
- Define and call functions with various parameter types
- Understand and apply function return values effectively
- Master variable scope concepts (local, global, nonlocal)
- Create and use lambda functions for simple operations
- Implement advanced function features like default parameters and variable arguments
- Write comprehensive function documentation and docstrings
- Handle errors gracefully within functions
- Apply best practices for writing clean, reusable functions
- Utilize built-in functions effectively

## 1. Function Fundamentals

### 1.1 What are Functions?

Functions are reusable blocks of code that perform specific tasks. They promote code reusability, modularity, and maintainability.

**Basic Function Structure:**
```python
def function_name(parameters):
    """Optional docstring"""
    # Function body
    return result  # Optional

# Function call
result = function_name(arguments)
```

**Simple Function Example:**
```python
def greet():
    """Print a greeting message"""
    print("Hello, World!")

# Call the function
greet()  # Output: Hello, World!
```

### 1.2 Function Definition and Calling

```python
# Function definition
def calculate_area(length, width):
    """Calculate the area of a rectangle"""
    area = length * width
    return area

# Function calls
rectangle_area = calculate_area(5, 3)
print(f"Area: {rectangle_area}")  # Output: Area: 15

# Multiple calls
areas = [
    calculate_area(4, 6),
    calculate_area(2, 8),
    calculate_area(7, 3)
]
print(f"Areas: {areas}")  # Output: Areas: [24, 16, 21]
```

### 1.3 Function Naming Conventions

```python
# Good function names (snake_case)
def calculate_total_price():
    pass

def get_user_input():
    pass

def validate_email_address():
    pass

def process_payment_data():
    pass

# Functions should be verbs or verb phrases
def save_file():
    pass

def load_configuration():
    pass

def send_notification():
    pass
```

### 1.4 The `pass` Statement

```python
def placeholder_function():
    """This function will be implemented later"""
    pass  # Placeholder - does nothing

def under_construction():
    """Feature under development"""
    pass

# pass can be used anywhere a statement is required
if True:
    pass  # No operation
```

## 2. Function Parameters and Arguments

### 2.1 Positional Parameters

```python
def introduce_person(name, age, city):
    """Introduce a person with their details"""
    print(f"Hello, I'm {name}")
    print(f"I'm {age} years old")
    print(f"I live in {city}")

# Positional arguments - order matters
introduce_person("Alice", 25, "New York")
# Output:
# Hello, I'm Alice
# I'm 25 years old
# I live in New York

# Wrong order gives unexpected results
introduce_person(25, "Alice", "New York")
# Output:
# Hello, I'm 25
# I'm Alice years old
# I live in New York
```

### 2.2 Keyword Arguments

```python
def create_profile(name, age, city, occupation="Unknown"):
    """Create a user profile"""
    return {
        "name": name,
        "age": age,
        "city": city,
        "occupation": occupation
    }

# Keyword arguments - order doesn't matter
profile1 = create_profile(name="Bob", city="London", age=30)
profile2 = create_profile(age=28, name="Carol", city="Paris", occupation="Engineer")

print(profile1)
# Output: {'name': 'Bob', 'age': 30, 'city': 'London', 'occupation': 'Unknown'}

print(profile2)
# Output: {'name': 'Carol', 'age': 28, 'city': 'Paris', 'occupation': 'Engineer'}
```

### 2.3 Default Parameters

```python
def power(base, exponent=2):
    """Calculate base raised to exponent (default: square)"""
    return base ** exponent

# Using default value
print(power(5))     # Output: 25 (5^2)
print(power(3))     # Output: 9 (3^2)

# Overriding default value
print(power(2, 3))  # Output: 8 (2^3)
print(power(4, 0))  # Output: 1 (4^0)

# Multiple default parameters
def format_name(first, last, middle="", title=""):
    """Format a person's name"""
    parts = [title, first, middle, last]
    return " ".join(part for part in parts if part)

print(format_name("John", "Doe"))
# Output: John Doe

print(format_name("Jane", "Smith", middle="Marie"))
# Output: Jane Marie Smith

print(format_name("Robert", "Johnson", title="Dr."))
# Output: Dr. Robert Johnson
```

### 2.4 Variable Arguments (*args)

```python
def sum_numbers(*args):
    """Sum any number of arguments"""
    total = 0
    for number in args:
        total += number
    return total

# Various number of arguments
print(sum_numbers(1, 2, 3))           # Output: 6
print(sum_numbers(1, 2, 3, 4, 5))     # Output: 15
print(sum_numbers(10))                # Output: 10
print(sum_numbers())                  # Output: 0

def print_info(name, *hobbies):
    """Print name and hobbies"""
    print(f"Name: {name}")
    if hobbies:
        print("Hobbies:")
        for hobby in hobbies:
            print(f"  - {hobby}")
    else:
        print("No hobbies listed")

print_info("Alice", "reading", "swimming", "cooking")
# Output:
# Name: Alice
# Hobbies:
#   - reading
#   - swimming
#   - cooking
```

### 2.5 Keyword Variable Arguments (**kwargs)

```python
def create_user(**kwargs):
    """Create a user with flexible attributes"""
    user = {}
    for key, value in kwargs.items():
        user[key] = value
    return user

# Flexible keyword arguments
user1 = create_user(name="Alice", age=25, city="New York")
user2 = create_user(name="Bob", occupation="Engineer", salary=75000)

print(user1)
# Output: {'name': 'Alice', 'age': 25, 'city': 'New York'}

print(user2)
# Output: {'name': 'Bob', 'occupation': 'Engineer', 'salary': 75000}

def flexible_function(required_arg, *args, **kwargs):
    """Function with all types of arguments"""
    print(f"Required: {required_arg}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

flexible_function("Hello", 1, 2, 3, name="Alice", age=25)
# Output:
# Required: Hello
# Args: (1, 2, 3)
# Kwargs: {'name': 'Alice', 'age': 25}
```

### 2.6 Argument Unpacking

```python
# Unpacking lists/tuples with *
def calculate_rectangle_area(length, width):
    return length * width

dimensions = (5, 3)
area = calculate_rectangle_area(*dimensions)
print(f"Area: {area}")  # Output: Area: 15

# Unpacking dictionaries with **
def greet_person(name, age, city):
    return f"Hello {name}, age {age} from {city}"

person_info = {"name": "Alice", "age": 25, "city": "New York"}
greeting = greet_person(**person_info)
print(greeting)  # Output: Hello Alice, age 25 from New York

# Complex unpacking example
def process_data(id, *values, **metadata):
    """Process data with ID, values, and metadata"""
    print(f"ID: {id}")
    print(f"Values: {values}")
    print(f"Metadata: {metadata}")

data_values = [1, 2, 3, 4, 5]
data_meta = {"source": "sensor", "timestamp": "2024-01-01"}

process_data(100, *data_values, **data_meta)
# Output:
# ID: 100
# Values: (1, 2, 3, 4, 5)
# Metadata: {'source': 'sensor', 'timestamp': '2024-01-01'}
```

## 3. Return Statements

### 3.1 Basic Return Values

```python
def add_numbers(a, b):
    """Add two numbers and return the result"""
    result = a + b
    return result

def get_absolute_value(number):
    """Return the absolute value of a number"""
    if number < 0:
        return -number
    return number

# Using return values
sum_result = add_numbers(5, 3)
print(f"Sum: {sum_result}")  # Output: Sum: 8

abs_value = get_absolute_value(-7)
print(f"Absolute value: {abs_value}")  # Output: Absolute value: 7
```

### 3.2 Multiple Return Values

```python
def get_name_parts(full_name):
    """Split full name into first and last name"""
    parts = full_name.split()
    if len(parts) >= 2:
        return parts[0], parts[-1]
    return parts[0], ""

def calculate_stats(numbers):
    """Calculate min, max, and average of numbers"""
    if not numbers:
        return None, None, None
    
    min_val = min(numbers)
    max_val = max(numbers)
    avg_val = sum(numbers) / len(numbers)
    
    return min_val, max_val, avg_val

# Unpacking multiple return values
first, last = get_name_parts("John Doe")
print(f"First: {first}, Last: {last}")  # Output: First: John, Last: Doe

minimum, maximum, average = calculate_stats([1, 2, 3, 4, 5])
print(f"Min: {minimum}, Max: {maximum}, Avg: {average}")
# Output: Min: 1, Max: 5, Avg: 3.0
```

### 3.3 Early Returns

```python
def validate_age(age):
    """Validate age and return validation result"""
    if not isinstance(age, int):
        return False, "Age must be an integer"
    
    if age < 0:
        return False, "Age cannot be negative"
    
    if age > 150:
        return False, "Age seems unrealistic"
    
    return True, "Age is valid"

def find_item(items, target):
    """Find item in list and return index"""
    for i, item in enumerate(items):
        if item == target:
            return i  # Early return when found
    return -1  # Not found

# Using early returns
is_valid, message = validate_age(25)
print(f"Valid: {is_valid}, Message: {message}")

index = find_item(['apple', 'banana', 'cherry'], 'banana')
print(f"Index: {index}")  # Output: Index: 1
```

### 3.4 Return None

```python
def print_info(name, age=None):
    """Print information about a person"""
    print(f"Name: {name}")
    if age is not None:
        print(f"Age: {age}")
    # No explicit return - returns None

def process_data(data):
    """Process data if valid"""
    if not data:
        return None  # Explicit None return
    
    # Process data
    processed = [item.upper() for item in data]
    return processed

# Functions without explicit return return None
result = print_info("Alice", 25)
print(f"Result: {result}")  # Output: Result: None

# Handling None returns
processed = process_data([])
if processed is None:
    print("No data to process")
```

## 4. Variable Scope

### 4.1 Local Scope

```python
def calculate_total():
    """Local variables are only accessible within the function"""
    price = 100  # Local variable
    tax = 0.08   # Local variable
    total = price * (1 + tax)
    return total

result = calculate_total()
print(f"Total: {result}")  # Output: Total: 108.0

# This would cause a NameError
# print(price)  # Error: name 'price' is not defined
```

### 4.2 Global Scope

```python
# Global variable
counter = 0

def increment_counter():
    """Access global variable"""
    global counter  # Declare as global
    counter += 1
    print(f"Counter: {counter}")

def read_counter():
    """Read global variable (no modification)"""
    print(f"Current counter: {counter}")

# Using global variables
increment_counter()  # Output: Counter: 1
increment_counter()  # Output: Counter: 2
read_counter()       # Output: Current counter: 2

# Global constants (convention: UPPERCASE)
PI = 3.14159
GRAVITY = 9.8

def calculate_circle_area(radius):
    """Use global constant"""
    return PI * radius ** 2

area = calculate_circle_area(5)
print(f"Circle area: {area}")  # Output: Circle area: 78.53975
```

### 4.3 Nonlocal Scope

```python
def outer_function():
    """Demonstrate nonlocal scope"""
    x = 10  # Enclosing scope variable
    
    def inner_function():
        nonlocal x  # Refer to enclosing scope
        x += 5
        print(f"Inner x: {x}")
    
    def another_inner():
        print(f"Another inner x: {x}")
    
    print(f"Initial x: {x}")
    inner_function()
    another_inner()
    print(f"Final x: {x}")

outer_function()
# Output:
# Initial x: 10
# Inner x: 15
# Another inner x: 15
# Final x: 15
```

### 4.4 Scope Resolution (LEGB Rule)

```python
# LEGB: Local, Enclosing, Global, Built-in
x = "global"

def outer():
    x = "enclosing"
    
    def inner():
        x = "local"
        print(f"Inner x: {x}")        # Local
        print(f"Built-in len: {len}")  # Built-in
    
    inner()
    print(f"Outer x: {x}")  # Enclosing

outer()
print(f"Global x: {x}")  # Global

# Output:
# Inner x: local
# Built-in len: <built-in function len>
# Outer x: enclosing
# Global x: global
```

### 4.5 Best Practices for Scope

```python
# Avoid global variables when possible
def bad_example():
    """Bad: Modifying global state"""
    global total_sales
    total_sales += 100

# Better: Pass values and return results
def good_example(current_sales, new_sale):
    """Good: Pure function"""
    return current_sales + new_sale

# Use function parameters instead of globals
def calculate_discount(price, discount_rate=0.1):
    """Good: All dependencies are explicit"""
    return price * (1 - discount_rate)

# Constants are acceptable globals
MAX_RETRIES = 3
API_URL = "https://api.example.com"

def make_request(endpoint, retries=MAX_RETRIES):
    """Using global constants is fine"""
    url = f"{API_URL}/{endpoint}"
    # Implementation here
    pass
```

## 5. Lambda Functions

### 5.1 Lambda Function Basics

```python
# Lambda function syntax
# lambda arguments: expression

# Regular function
def square(x):
    return x ** 2

# Equivalent lambda function
square_lambda = lambda x: x ** 2

print(square(5))        # Output: 25
print(square_lambda(5)) # Output: 25

# Lambda with multiple arguments
multiply = lambda x, y: x * y
print(multiply(3, 4))   # Output: 12

# Lambda with default arguments
greet = lambda name, greeting="Hello": f"{greeting}, {name}!"
print(greet("Alice"))              # Output: Hello, Alice!
print(greet("Bob", "Hi"))          # Output: Hi, Bob!
```

### 5.2 Lambda with Built-in Functions

```python
# Using lambda with map()
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # Output: [1, 4, 9, 16, 25]

# Using lambda with filter()
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # Output: [2, 4, 6, 8, 10]

# Using lambda with sorted()
students = [('Alice', 85), ('Bob', 92), ('Charlie', 78)]
sorted_by_grade = sorted(students, key=lambda x: x[1])
print(sorted_by_grade)  # Output: [('Charlie', 78), ('Alice', 85), ('Bob', 92)]

# Using lambda with max() and min()
words = ['python', 'java', 'javascript', 'c++']
longest = max(words, key=lambda x: len(x))
shortest = min(words, key=lambda x: len(x))
print(f"Longest: {longest}")   # Output: Longest: javascript
print(f"Shortest: {shortest}") # Output: Shortest: c++
```

### 5.3 Lambda Limitations and Alternatives

```python
# Lambda limitations - only expressions, not statements
# This won't work:
# invalid_lambda = lambda x: if x > 0: return x

# Use regular function for complex logic
def process_value(x):
    if x > 0:
        return x * 2
    elif x < 0:
        return x / 2
    else:
        return 0

# Lambda can't have multiple statements
# This won't work:
# invalid_lambda = lambda x: print(x); return x * 2

# Use regular function for side effects
def log_and_square(x):
    print(f"Processing: {x}")
    return x ** 2

# Lambda best practices
# Good: Simple, single expression
numbers = [1, 2, 3, 4, 5]
doubled = list(map(lambda x: x * 2, numbers))

# Better: Use list comprehension for simple cases
doubled_better = [x * 2 for x in numbers]

# Good: Lambda for sorting key
people = [{'name': 'Alice', 'age': 25}, {'name': 'Bob', 'age': 30}]
sorted_people = sorted(people, key=lambda p: p['age'])
```

## 6. Advanced Function Concepts

### 6.1 Functions as First-Class Objects

```python
# Functions can be assigned to variables
def greet(name):
    return f"Hello, {name}!"

say_hello = greet
print(say_hello("Alice"))  # Output: Hello, Alice!

# Functions can be passed as arguments
def apply_operation(x, y, operation):
    return operation(x, y)

def add(a, b):
    return a + b

def multiply(a, b):
    return a * b

result1 = apply_operation(5, 3, add)
result2 = apply_operation(5, 3, multiply)
print(f"Addition: {result1}")      # Output: Addition: 8
print(f"Multiplication: {result2}") # Output: Multiplication: 15

# Functions can be returned from other functions
def create_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

double = create_multiplier(2)
triple = create_multiplier(3)

print(double(5))  # Output: 10
print(triple(5))  # Output: 15
```

### 6.2 Closures

```python
def create_counter(initial=0):
    """Create a counter function with closure"""
    count = initial
    
    def counter():
        nonlocal count
        count += 1
        return count
    
    return counter

# Each counter maintains its own state
counter1 = create_counter()
counter2 = create_counter(100)

print(counter1())  # Output: 1
print(counter1())  # Output: 2
print(counter2())  # Output: 101
print(counter1())  # Output: 3
print(counter2())  # Output: 102

def create_accumulator():
    """Create an accumulator with closure"""
    total = 0
    
    def accumulate(value):
        nonlocal total
        total += value
        return total
    
    return accumulate

acc = create_accumulator()
print(acc(10))  # Output: 10
print(acc(5))   # Output: 15
print(acc(3))   # Output: 18
```

### 6.3 Recursive Functions

```python
def factorial(n):
    """Calculate factorial using recursion"""
    if n <= 1:
        return 1
    return n * factorial(n - 1)

print(factorial(5))  # Output: 120 (5! = 5*4*3*2*1)

def fibonacci(n):
    """Calculate nth Fibonacci number"""
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# Generate Fibonacci sequence
fib_sequence = [fibonacci(i) for i in range(10)]
print(fib_sequence)  # Output: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

# Optimized recursive function with memoization
def fibonacci_memo(n, memo={}):
    """Optimized Fibonacci with memoization"""
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    memo[n] = fibonacci_memo(n - 1, memo) + fibonacci_memo(n - 2, memo)
    return memo[n]

print(fibonacci_memo(50))  # Much faster for large numbers
```

### 6.4 Generator Functions

```python
def count_up_to(max_count):
    """Generator function that yields numbers up to max_count"""
    count = 1
    while count <= max_count:
        yield count
        count += 1

# Using generator
counter = count_up_to(5)
for num in counter:
    print(num)  # Output: 1, 2, 3, 4, 5

def fibonacci_generator(max_terms):
    """Generate Fibonacci sequence"""
    a, b = 0, 1
    for _ in range(max_terms):
        yield a
        a, b = b, a + b

# Memory efficient iteration
for fib in fibonacci_generator(10):
    print(fib, end=" ")  # Output: 0 1 1 2 3 5 8 13 21 34
```

## 7. Function Documentation and Testing

### 7.1 Docstrings

```python
def calculate_bmi(weight, height):
    """
    Calculate Body Mass Index (BMI).
    
    Args:
        weight (float): Weight in kilograms
        height (float): Height in meters
    
    Returns:
        float: BMI value
    
    Raises:
        ValueError: If weight or height is negative or zero
    
    Examples:
        >>> calculate_bmi(70, 1.75)
        22.857142857142858
        >>> calculate_bmi(80, 1.8)
        24.691358024691358
    """
    if weight <= 0 or height <= 0:
        raise ValueError("Weight and height must be positive")
    
    return weight / (height ** 2)

# Access docstring
print(calculate_bmi.__doc__)

# Using help() function
help(calculate_bmi)
```

### 7.2 Type Hints

```python
from typing import List, Dict, Optional, Union

def process_scores(scores: List[float]) -> Dict[str, float]:
    """
    Process a list of scores and return statistics.
    
    Args:
        scores: List of float scores
    
    Returns:
        Dictionary with statistics
    """
    if not scores:
        return {"count": 0, "average": 0.0, "min": 0.0, "max": 0.0}
    
    return {
        "count": len(scores),
        "average": sum(scores) / len(scores),
        "min": min(scores),
        "max": max(scores)
    }

def find_user(users: List[Dict[str, Union[str, int]]], 
              user_id: int) -> Optional[Dict[str, Union[str, int]]]:
    """
    Find a user by ID.
    
    Args:
        users: List of user dictionaries
        user_id: User ID to search for
    
    Returns:
        User dictionary if found, None otherwise
    """
    for user in users:
        if user.get("id") == user_id:
            return user
    return None
```

### 7.3 Function Testing

```python
def is_prime(n):
    """Check if a number is prime"""
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

def test_is_prime():
    """Test function for is_prime"""
    # Test cases
    assert is_prime(2) == True
    assert is_prime(3) == True
    assert is_prime(4) == False
    assert is_prime(17) == True
    assert is_prime(1) == False
    assert is_prime(0) == False
    assert is_prime(-5) == False
    print("All tests passed!")

# Run tests
test_is_prime()

# Doctests
def add_numbers(a, b):
    """
    Add two numbers.
    
    >>> add_numbers(2, 3)
    5
    >>> add_numbers(-1, 1)
    0
    >>> add_numbers(0, 0)
    0
    """
    return a + b

# Run doctests
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

## 8. Built-in Functions

### 8.1 Mathematical Functions

```python
# abs() - absolute value
print(abs(-5))      # Output: 5
print(abs(3.14))    # Output: 3.14

# round() - rounding
print(round(3.7))       # Output: 4
print(round(3.14159, 2)) # Output: 3.14

# min() and max()
numbers = [3, 1, 4, 1, 5, 9, 2, 6]
print(min(numbers))     # Output: 1
print(max(numbers))     # Output: 9

# sum()
print(sum(numbers))     # Output: 31
print(sum(numbers, 10)) # Output: 41 (start value)

# divmod() - division and modulo
quotient, remainder = divmod(17, 5)
print(f"17 ÷ 5 = {quotient} remainder {remainder}")  # Output: 17 ÷ 5 = 3 remainder 2

# pow() - power
print(pow(2, 3))        # Output: 8
print(pow(2, 3, 5))     # Output: 3 (2^3 % 5)
```

### 8.2 Type Conversion Functions

```python
# int(), float(), str(), bool()
print(int("42"))        # Output: 42
print(int(3.7))         # Output: 3
print(int("1010", 2))   # Output: 10 (binary)

print(float("3.14"))    # Output: 3.14
print(float(42))        # Output: 42.0

print(str(42))          # Output: "42"
print(str(3.14))        # Output: "3.14"

print(bool(1))          # Output: True
print(bool(0))          # Output: False
print(bool(""))         # Output: False
print(bool("hello"))    # Output: True

# Complex type conversions
print(list("hello"))    # Output: ['h', 'e', 'l', 'l', 'o']
print(tuple([1, 2, 3])) # Output: (1, 2, 3)
print(set([1, 2, 2, 3])) # Output: {1, 2, 3}
```

### 8.3 Sequence Functions

```python
# len() - length
print(len("hello"))     # Output: 5
print(len([1, 2, 3]))   # Output: 3

# enumerate() - index and value
fruits = ["apple", "banana", "cherry"]
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")

# zip() - combine sequences
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
for name, age in zip(names, ages):
    print(f"{name} is {age} years old")

# range() - generate sequence
for i in range(5):
    print(i)  # Output: 0, 1, 2, 3, 4

# reversed() - reverse sequence
print(list(reversed([1, 2, 3, 4, 5])))  # Output: [5, 4, 3, 2, 1]

# sorted() - sort sequence
print(sorted([3, 1, 4, 1, 5]))  # Output: [1, 1, 3, 4, 5]
print(sorted("hello"))          # Output: ['e', 'h', 'l', 'l', 'o']
```

### 8.4 Functional Programming Functions

```python
# map() - apply function to sequence
numbers = [1, 2, 3, 4, 5]
squares = list(map(lambda x: x**2, numbers))
print(squares)  # Output: [1, 4, 9, 16, 25]

# filter() - filter sequence
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # Output: [2, 4]

# any() and all()
print(any([True, False, False]))   # Output: True
print(all([True, False, False]))   # Output: False
print(any([False, False, False]))  # Output: False
print(all([True, True, True]))     # Output: True

# Check conditions
scores = [85, 92, 78, 96, 88]
print(any(score >= 90 for score in scores))  # Output: True
print(all(score >= 80 for score in scores))  # Output: False
```

## 9. Error Handling in Functions

### 9.1 Handling Exceptions

```python
def safe_divide(a, b):
    """Safely divide two numbers"""
    try:
        result = a / b
        return result
    except ZeroDivisionError:
        print("Error: Division by zero")
        return None
    except TypeError:
        print("Error: Invalid input types")
        return None

# Usage
print(safe_divide(10, 2))   # Output: 5.0
print(safe_divide(10, 0))   # Output: Error: Division by zero, None
print(safe_divide("10", 2)) # Output: Error: Invalid input types, None

def validate_and_process(data):
    """Validate and process data with comprehensive error handling"""
    if not data:
        raise ValueError("Data cannot be empty")
    
    if not isinstance(data, list):
        raise TypeError("Data must be a list")
    
    try:
        # Process numeric data
        numbers = [float(item) for item in data]
        return sum(numbers) / len(numbers)
    except ValueError as e:
        raise ValueError(f"Invalid numeric data: {e}")
    except Exception as e:
        raise RuntimeError(f"Unexpected error: {e}")

# Usage with error handling
try:
    result = validate_and_process([1, 2, 3, 4, 5])
    print(f"Average: {result}")
except ValueError as e:
    print(f"Validation error: {e}")
except Exception as e:
    print(f"Error: {e}")
```

### 9.2 Custom Exceptions

```python
class ValidationError(Exception):
    """Custom exception for validation errors"""
    pass

class ProcessingError(Exception):
    """Custom exception for processing errors"""
    pass

def process_user_data(user_data):
    """Process user data with custom exceptions"""
    if not user_data:
        raise ValidationError("User data is required")
    
    required_fields = ['name', 'email', 'age']
    for field in required_fields:
        if field not in user_data:
            raise ValidationError(f"Missing required field: {field}")
    
    # Validate age
    try:
        age = int(user_data['age'])
        if age < 0 or age > 150:
            raise ValidationError("Age must be between 0 and 150")
    except ValueError:
        raise ValidationError("Age must be a valid integer")
    
    # Process data
    try:
        processed_data = {
            'name': user_data['name'].strip().title(),
            'email': user_data['email'].lower(),
            'age': age
        }
        return processed_data
    except Exception as e:
        raise ProcessingError(f"Failed to process data: {e}")

# Usage
try:
    user = {'name': 'john doe', 'email': 'JOHN@EXAMPLE.COM', 'age': '25'}
    result = process_user_data(user)
    print(result)
except ValidationError as e:
    print(f"Validation error: {e}")
except ProcessingError as e:
    print(f"Processing error: {e}")
```

## 10. Performance and Best Practices

### 10.1 Function Performance

```python
import time

def slow_function(n):
    """Inefficient function for demonstration"""
    result = []
    for i in range(n):
        result.append(i ** 2)
    return result

def fast_function(n):
    """Optimized version using list comprehension"""
    return [i ** 2 for i in range(n)]

def measure_performance(func, *args):
    """Measure function execution time"""
    start_time = time.time()
    result = func(*args)
    end_time = time.time()
    return result, end_time - start_time

# Compare performance
n = 100000
result1, time1 = measure_performance(slow_function, n)
result2, time2 = measure_performance(fast_function, n)

print(f"Slow function: {time1:.4f} seconds")
print(f"Fast function: {time2:.4f} seconds")
print(f"Speedup: {time1/time2:.2f}x")
```

### 10.2 Memory Efficiency

```python
def memory_inefficient_generator():
    """Memory inefficient - creates entire list in memory"""
    return [i for i in range(1000000)]

def memory_efficient_generator():
    """Memory efficient - generates values on demand"""
    for i in range(1000000):
        yield i

# Memory usage comparison
import sys

# List uses more memory
large_list = [i for i in range(1000)]
print(f"List memory usage: {sys.getsizeof(large_list)} bytes")

# Generator uses less memory
large_generator = (i for i in range(1000))
print(f"Generator memory usage: {sys.getsizeof(large_generator)} bytes")
```

### 10.3 Best Practices

```python
# 1. Pure functions (no side effects)
def pure_function(x, y):
    """Pure function - same input always produces same output"""
    return x + y

# 2. Single responsibility
def calculate_tax(price, tax_rate):
    """Function does one thing well"""
    return price * tax_rate

def format_currency(amount):
    """Another function with single responsibility"""
    return f"${amount:.2f}"

# 3. Descriptive names
def calculate_monthly_payment(principal, rate, years):
    """Clear, descriptive function name"""
    monthly_rate = rate / 12
    num_payments = years * 12
    return principal * (monthly_rate * (1 + monthly_rate) ** num_payments) / \
           ((1 + monthly_rate) ** num_payments - 1)

# 4. Default parameters for flexibility
def log_message(message, level="INFO", timestamp=None):
    """Flexible function with defaults"""
    if timestamp is None:
        timestamp = time.time()
    print(f"[{level}] {timestamp}: {message}")

# 5. Input validation
def calculate_square_root(number):
    """Validate input before processing"""
    if not isinstance(number, (int, float)):
        raise TypeError("Input must be a number")
    if number < 0:
        raise ValueError("Cannot calculate square root of negative number")
    return number ** 0.5

# 6. Consistent return types
def find_max_value(numbers):
    """Always return the same type"""
    if not numbers:
        return None  # Consistent return type
    return max(numbers)
```

## Review Questions

### Theoretical Questions

1. **Explain the difference between parameters and arguments in Python functions.**

**Answer:**
- **Parameters** are variables defined in the function definition
- **Arguments** are actual values passed to the function when called

```python
def greet(name, greeting="Hello"):  # name and greeting are parameters
    return f"{greeting}, {name}!"

result = greet("Alice", "Hi")  # "Alice" and "Hi" are arguments
```

2. **What is the LEGB rule in Python scope resolution?**

**Answer:**
LEGB stands for Local, Enclosing, Global, Built-in - the order Python searches for variable names:
- **Local**: Inside the current function
- **Enclosing**: In enclosing functions (closures)
- **Global**: At module level
- **Built-in**: Built-in names like `print`, `len`

```python
x = "global"  # Global

def outer():
    x = "enclosing"  # Enclosing
    
    def inner():
        x = "local"  # Local
        print(x)     # Prints "local"
    
    inner()

outer()
```

3. **When should you use lambda functions versus regular functions?**

**Answer:**
Use lambda functions for:
- Simple, one-line expressions
- Short-lived functions (e.g., with `map()`, `filter()`, `sorted()`)
- Functional programming patterns

Use regular functions for:
- Complex logic requiring multiple statements
- Functions that need docstrings
- Reusable code
- Better readability and debugging

### Practical Questions

4. **Write a function that takes a variable number of arguments and returns a dictionary with statistics.**

**Answer:**
```python
def calculate_statistics(*numbers):
    """Calculate statistics for any number of arguments."""
    if not numbers:
        return {"count": 0, "sum": 0, "avg": 0, "min": None, "max": None}
    
    return {
        "count": len(numbers),
        "sum": sum(numbers),
        "avg": sum(numbers) / len(numbers),
        "min": min(numbers),
        "max": max(numbers)
    }

# Usage
stats = calculate_statistics(1, 2, 3, 4, 5)
print(stats)  # {'count': 5, 'sum': 15, 'avg': 3.0, 'min': 1, 'max': 5}
```

5. **Create a function that demonstrates closure by maintaining state between calls.**

**Answer:**
```python
def create_bank_account(initial_balance=0):
    """Create a bank account with closure to maintain balance."""
    balance = initial_balance
    
    def account_operations(operation, amount=0):
        nonlocal balance
        
        if operation == "deposit":
            balance += amount
            return f"Deposited ${amount}. New balance: ${balance}"
        elif operation == "withdraw":
            if amount <= balance:
                balance -= amount
                return f"Withdrew ${amount}. New balance: ${balance}"
            else:
                return "Insufficient funds"
        elif operation == "balance":
            return f"Current balance: ${balance}"
        else:
            return "Invalid operation"
    
    return account_operations

# Usage
account = create_bank_account(100)
print(account("deposit", 50))   # Deposited $50. New balance: $150
print(account("withdraw", 30))  # Withdrew $30. New balance: $120
print(account("balance"))       # Current balance: $120
```

### Advanced Questions

6. **Implement a decorator function that measures execution time of other functions.**

**Answer:**
```python
import time
import functools

def timing_decorator(func):
    """Decorator to measure function execution time."""
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        execution_time = end_time - start_time
        print(f"{func.__name__} executed in {execution_time:.4f} seconds")
        return result
    return wrapper

@timing_decorator
def slow_function(n):
    """Function that takes some time to execute."""
    total = 0
    for i in range(n):
        total += i ** 2
    return total

# Usage
result = slow_function(100000)
# Output: slow_function executed in 0.0234 seconds
```

7. **Create a function that implements memoization for recursive functions.**

**Answer:**
```python
def memoize(func):
    """Decorator to add memoization to recursive functions."""
    cache = {}
    
    @functools.wraps(func)
    def wrapper(*args):
        if args in cache:
            return cache[args]
        
        result = func(*args)
        cache[args] = result
        return result
    
    return wrapper

@memoize
def fibonacci(n):
    """Calculate Fibonacci number with memoization."""
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# Usage - much faster for large numbers
print(fibonacci(50))  # Calculates quickly due to memoization
```

8. **Write a function that validates function arguments using type hints and raises appropriate errors.**

**Answer:**
```python
from typing import List, Union
import inspect

def validate_types(func):
    """Decorator to validate function arguments against type hints."""
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        # Get function signature
        sig = inspect.signature(func)
        bound_args = sig.bind(*args, **kwargs)
        bound_args.apply_defaults()
        
        # Check each argument against its type hint
        for param_name, param_value in bound_args.arguments.items():
            param = sig.parameters[param_name]
            
            if param.annotation != inspect.Parameter.empty:
                expected_type = param.annotation
                
                # Handle Union types
                if hasattr(expected_type, '__origin__') and expected_type.__origin__ is Union:
                    if not isinstance(param_value, expected_type.__args__):
                        raise TypeError(f"Argument '{param_name}' must be one of {expected_type.__args__}, got {type(param_value)}")
                else:
                    if not isinstance(param_value, expected_type):
                        raise TypeError(f"Argument '{param_name}' must be {expected_type}, got {type(param_value)}")
        
        return func(*args, **kwargs)
    
    return wrapper

@validate_types
def process_data(numbers: List[int], multiplier: Union[int, float] = 1.0) -> List[Union[int, float]]:
    """Process list of numbers with type validation."""
    return [num * multiplier for num in numbers]

# Usage
result = process_data([1, 2, 3, 4], 2.5)  # Works fine
# process_data("not a list", 2)  # Raises TypeError
```

## Practical Exercises

### Exercise 1: Function Library
Create a library of utility functions for common mathematical operations (factorial, prime checking, GCD, etc.).

### Exercise 2: Text Processing Functions
Build a set of functions for text analysis including word counting, sentiment analysis, and text cleaning.

### Exercise 3: Data Validation System
Create a comprehensive data validation system using functions with custom exceptions.

### Exercise 4: Function Composition
Implement function composition utilities that allow combining multiple functions into pipelines.

---

## Next Steps

In the next module, we'll explore Python's built-in data structures in detail, including lists, tuples, dictionaries, and sets. You'll learn how to choose the right data structure for different tasks and manipulate them efficiently.

## Additional Resources

- [Python Functions Documentation](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
- [Python Functional Programming HOWTO](https://docs.python.org/3/howto/functional.html)
- [PEP 257 - Docstring Conventions](https://www.python.org/dev/peps/pep-0257/)
- [Python Type Hints](https://docs.python.org/3/library/typing.html)
- [Python Decorators Guide](https://realpython.com/primer-on-python-decorators/)

---

*Functions are the building blocks of modular, reusable code. Master these concepts to write clean, efficient, and maintainable Python programs.*
