# Python Beginner 1.2: Basic Syntax & Data Types

## Table of Contents
1. [Learning Objectives](#learning-objectives)
2. [Python Syntax Fundamentals](#1-python-syntax-fundamentals)
3. [Variables and Assignment](#2-variables-and-assignment)
4. [Numeric Data Types](#3-numeric-data-types)
5. [String Data Type](#4-string-data-type)
6. [Boolean Data Type](#5-boolean-data-type)
7. [Comments and Documentation](#6-comments-and-documentation)
8. [Type Checking and Conversion](#7-type-checking-and-conversion)
9. [Advanced String Operations](#8-advanced-string-operations)
10. [Practical Examples and Use Cases](#9-practical-examples-and-use-cases)
11. [Performance Considerations](#10-performance-considerations)
12. [Review Questions](#review-questions)
13. [Practical Exercises](#practical-exercises)
14. [Next Steps](#next-steps)
15. [Additional Resources](#additional-resources)

## Learning Objectives
By the end of this module, you will be able to:
- Understand Python's syntax rules and conventions
- Work with variables and follow naming conventions
- Master Python's numeric data types and operations
- Manipulate strings effectively using various methods
- Use boolean logic and logical operations
- Write proper comments and documentation
- Apply best practices for code readability

## 1. Python Syntax Fundamentals

### 1.1 Basic Syntax Rules

Python's syntax is designed to be readable and intuitive. Key principles:

**Indentation:**
```python
# Correct indentation
if True:
    print("This is properly indented")
    if True:
        print("This is nested indentation")

# Incorrect indentation (will cause IndentationError)
if True:
print("This will cause an error")
```

**Line Structure:**
```python
# Single statement
x = 5

# Multiple statements (discouraged)
x = 5; y = 10; z = 15

# Line continuation
total = 1 + 2 + 3 + \
        4 + 5 + 6

# Implicit line continuation
total = (1 + 2 + 3 +
         4 + 5 + 6)
```

**Case Sensitivity:**
```python
# These are different variables
name = "John"
Name = "Jane"
NAME = "Bob"

print(name)  # John
print(Name)  # Jane
print(NAME)  # Bob
```

### 1.2 Python Keywords

Python has reserved words that cannot be used as variable names:

```python
import keyword
print(keyword.kwlist)
# ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 
#  'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 
#  'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 
#  'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 
#  'while', 'with', 'yield']
```

### 1.3 Identifiers and Naming Rules

**Valid Identifiers:**
- Must start with letter (a-z, A-Z) or underscore (_)
- Can contain letters, digits, and underscores
- Case-sensitive
- Cannot be a keyword

```python
# Valid identifiers
name = "John"
_private = "secret"
age2 = 25
firstName = "John"
first_name = "John"
__special__ = "magic"

# Invalid identifiers
# 2name = "John"      # Cannot start with digit
# first-name = "John"  # Cannot contain hyphen
# class = "Math"       # Cannot use keyword
```

## 2. Variables and Assignment

### 2.1 Variable Declaration and Assignment

```python
# Simple assignment
x = 10
name = "Alice"
is_active = True

# Multiple assignment
a, b, c = 1, 2, 3
x = y = z = 0

# Unpacking
coordinates = (10, 20)
x, y = coordinates

# Swapping variables
a, b = b, a
```

### 2.2 Dynamic Typing

Python is dynamically typed - variables can change type:

```python
# Dynamic typing demonstration
x = 42          # x is an integer
print(type(x))  # <class 'int'>

x = "Hello"     # x is now a string
print(type(x))  # <class 'str'>

x = [1, 2, 3]   # x is now a list
print(type(x))  # <class 'list'>
```

### 2.3 Variable Naming Conventions (PEP 8)

**Convention Types:**
```python
# snake_case for variables and functions
user_name = "john_doe"
total_amount = 1500.50

# PascalCase for classes
class UserAccount:
    pass

# UPPER_CASE for constants
MAX_SIZE = 100
PI = 3.14159

# Leading underscore for "private" variables
_internal_var = "private"

# Double leading underscore for name mangling
__private_var = "very private"

# Double underscore both sides for special methods
def __init__(self):
    pass
```

## 3. Numeric Data Types

### 3.1 Integer (int)

```python
# Integer literals
decimal = 42
binary = 0b101010      # Binary (42 in decimal)
octal = 0o52           # Octal (42 in decimal)
hexadecimal = 0x2A     # Hexadecimal (42 in decimal)

# Large integers (unlimited precision)
large_number = 123456789012345678901234567890
print(large_number)

# Integer operations
a = 10
b = 3

addition = a + b        # 13
subtraction = a - b     # 7
multiplication = a * b  # 30
division = a / b        # 3.3333...
floor_division = a // b # 3
modulo = a % b         # 1
exponentiation = a ** b # 1000
```

### 3.2 Floating Point (float)

```python
# Float literals
regular_float = 3.14
scientific_notation = 1.5e-3  # 0.0015
another_scientific = 2.5E2    # 250.0

# Float precision
import sys
print(sys.float_info.epsilon)  # Smallest representable positive number

# Common float issues
print(0.1 + 0.2)  # 0.30000000000000004 (not exactly 0.3)

# Solutions for precision
from decimal import Decimal
a = Decimal('0.1')
b = Decimal('0.2')
print(a + b)  # 0.3

# Rounding
pi = 3.14159265359
print(round(pi, 2))      # 3.14
print(round(pi, 4))      # 3.1416
```

### 3.3 Complex Numbers

```python
# Complex number creation
z1 = 3 + 4j
z2 = complex(3, 4)
z3 = complex('3+4j')

# Complex number operations
z1 = 3 + 4j
z2 = 1 + 2j

addition = z1 + z2      # (4+6j)
multiplication = z1 * z2 # (-5+10j)
conjugate = z1.conjugate() # (3-4j)

# Accessing real and imaginary parts
print(z1.real)  # 3.0
print(z1.imag)  # 4.0

# Magnitude
import math
magnitude = abs(z1)  # 5.0
```

### 3.4 Numeric Conversions

```python
# Type conversion functions
int_val = int(3.7)      # 3 (truncation)
float_val = float(5)    # 5.0
complex_val = complex(2) # (2+0j)

# String to number conversion
num_str = "42"
integer = int(num_str)   # 42
float_num = float(num_str) # 42.0

# Base conversion
binary_str = "1010"
decimal = int(binary_str, 2)  # 10

# Number to string
num = 42
str_num = str(num)      # "42"
```

## 4. String Data Type

### 4.1 String Creation and Literals

```python
# String literals
single_quotes = 'Hello, World!'
double_quotes = "Hello, World!"
triple_quotes = '''Multi-line
string with
line breaks'''

# Raw strings (escape sequences ignored)
raw_string = r"C:\Users\name\Documents"
regular_string = "C:\\Users\\name\\Documents"

# Formatted strings (f-strings)
name = "Alice"
age = 30
formatted = f"My name is {name} and I am {age} years old"
```

### 4.2 String Operations

```python
# String concatenation
first = "Hello"
second = "World"
greeting = first + " " + second  # "Hello World"

# String repetition
repeated = "Ha" * 3  # "HaHaHa"

# String indexing (0-based)
text = "Python"
first_char = text[0]    # 'P'
last_char = text[-1]    # 'n'

# String slicing
substring = text[1:4]   # 'yth'
first_three = text[:3]  # 'Pyt'
last_three = text[-3:]  # 'hon'
reversed_str = text[::-1] # 'nohtyP'
```

### 4.3 String Methods

```python
text = "  Hello, World!  "

# Case methods
print(text.upper())      # "  HELLO, WORLD!  "
print(text.lower())      # "  hello, world!  "
print(text.capitalize()) # "  hello, world!  "
print(text.title())      # "  Hello, World!  "
print(text.swapcase())   # "  hELLO, wORLD!  "

# Whitespace methods
print(text.strip())      # "Hello, World!"
print(text.lstrip())     # "Hello, World!  "
print(text.rstrip())     # "  Hello, World!"

# Search methods
print(text.find("World"))    # 9
print(text.index("World"))   # 9 (raises ValueError if not found)
print(text.count("l"))       # 3
print(text.startswith("  Hello")) # True
print(text.endswith("!  "))  # True

# Replacement methods
replaced = text.replace("World", "Python")
print(replaced)  # "  Hello, Python!  "
```

### 4.4 String Formatting

```python
# Old-style formatting (%)
name = "Alice"
age = 30
old_style = "Name: %s, Age: %d" % (name, age)

# str.format() method
formatted = "Name: {}, Age: {}".format(name, age)
named_format = "Name: {name}, Age: {age}".format(name=name, age=age)

# f-string (Python 3.6+) - Recommended
f_string = f"Name: {name}, Age: {age}"

# Advanced f-string formatting
pi = 3.14159
formatted_pi = f"Pi rounded to 2 decimal places: {pi:.2f}"
print(formatted_pi)  # "Pi rounded to 2 decimal places: 3.14"

# Alignment and padding
name = "Alice"
print(f"{name:>10}")   # "     Alice" (right-aligned)
print(f"{name:<10}")   # "Alice     " (left-aligned)
print(f"{name:^10}")   # "  Alice   " (center-aligned)
print(f"{name:*^10}")  # "**Alice***" (center with padding)
```

### 4.5 String Validation Methods

```python
# Character type checking
text = "Hello123"
print(text.isalnum())    # True (alphanumeric)
print(text.isalpha())    # False (contains digits)
print(text.isdigit())    # False (contains letters)
print(text.isspace())    # False (not whitespace)

# Case checking
print("HELLO".isupper())  # True
print("hello".islower())  # True
print("Hello".istitle())  # True

# Validation examples
def is_valid_email(email):
    return "@" in email and "." in email.split("@")[1]

def is_valid_phone(phone):
    return phone.replace("-", "").replace(" ", "").isdigit()
```

## 5. Boolean Data Type

### 5.1 Boolean Values and Operations

```python
# Boolean literals
true_value = True
false_value = False

# Boolean operations
a = True
b = False

# Logical operators
and_result = a and b    # False
or_result = a or b      # True
not_result = not a      # False

# Comparison operators
x = 5
y = 10

equal = x == y          # False
not_equal = x != y      # True
less_than = x < y       # True
greater_than = x > y    # False
less_equal = x <= y     # True
greater_equal = x >= y  # False
```

### 5.2 Truthiness and Falsiness

```python
# Falsy values
falsy_values = [
    False,
    0,
    0.0,
    0j,
    "",
    [],
    {},
    set(),
    None
]

# Testing truthiness
def test_truthiness(value):
    if value:
        print(f"{value} is truthy")
    else:
        print(f"{value} is falsy")

# Examples
test_truthiness(True)     # True is truthy
test_truthiness(False)    # False is falsy
test_truthiness(1)        # 1 is truthy
test_truthiness(0)        # 0 is falsy
test_truthiness("hello")  # hello is truthy
test_truthiness("")       # "" is falsy
```

### 5.3 Boolean Conversion

```python
# Converting to boolean
print(bool(1))        # True
print(bool(0))        # False
print(bool("hello"))  # True
print(bool(""))       # False
print(bool([1, 2]))   # True
print(bool([]))       # False

# Practical examples
def is_valid_input(user_input):
    return bool(user_input and user_input.strip())

def has_data(data_list):
    return bool(data_list)
```

## 6. Comments and Documentation

### 6.1 Single-line Comments

```python
# This is a single-line comment
x = 5  # This is an inline comment

# Use comments to explain complex logic
def calculate_compound_interest(principal, rate, time):
    # Convert percentage to decimal
    decimal_rate = rate / 100
    
    # Apply compound interest formula
    amount = principal * (1 + decimal_rate) ** time
    
    return amount
```

### 6.2 Multi-line Comments

```python
"""
This is a multi-line comment
using triple quotes.
It can span multiple lines.
"""

'''
You can also use single quotes
for multi-line comments.
'''
```

### 6.3 Docstrings

```python
def calculate_area(radius):
    """
    Calculate the area of a circle.
    
    Args:
        radius (float): The radius of the circle.
    
    Returns:
        float: The area of the circle.
    
    Raises:
        ValueError: If radius is negative.
    
    Example:
        >>> calculate_area(5)
        78.53981633974483
    """
    if radius < 0:
        raise ValueError("Radius cannot be negative")
    
    import math
    return math.pi * radius ** 2

# Accessing docstring
print(calculate_area.__doc__)
```

### 6.4 Documentation Best Practices

```python
class BankAccount:
    """
    A simple bank account class.
    
    This class represents a bank account with basic operations
    like deposit, withdrawal, and balance inquiry.
    
    Attributes:
        account_number (str): The account number.
        balance (float): The current balance.
        owner (str): The account owner's name.
    """
    
    def __init__(self, account_number, owner, initial_balance=0.0):
        """
        Initialize a new bank account.
        
        Args:
            account_number (str): Unique account identifier.
            owner (str): Name of the account owner.
            initial_balance (float, optional): Starting balance. Defaults to 0.0.
        """
        self.account_number = account_number
        self.owner = owner
        self.balance = initial_balance
    
    def deposit(self, amount):
        """
        Deposit money into the account.
        
        Args:
            amount (float): Amount to deposit.
        
        Returns:
            float: New balance after deposit.
        
        Raises:
            ValueError: If amount is negative or zero.
        """
        if amount <= 0:
            raise ValueError("Deposit amount must be positive")
        
        self.balance += amount
        return self.balance
```

## 7. Type Checking and Conversion

### 7.1 Type Checking Functions

```python
# Using type() function
x = 42
print(type(x))          # <class 'int'>
print(type(x) == int)   # True

# Using isinstance() function (preferred)
print(isinstance(x, int))        # True
print(isinstance(x, (int, float))) # True (multiple types)

# Type checking in functions
def safe_divide(a, b):
    if not isinstance(a, (int, float)) or not isinstance(b, (int, float)):
        raise TypeError("Arguments must be numbers")
    
    if b == 0:
        raise ValueError("Cannot divide by zero")
    
    return a / b
```

### 7.2 Type Conversion Functions

```python
# Explicit type conversion
string_number = "42"
integer = int(string_number)      # 42
float_number = float(string_number) # 42.0

# Conversion with different bases
binary_string = "1010"
decimal = int(binary_string, 2)   # 10

# List to string conversion
numbers = [1, 2, 3, 4, 5]
string_numbers = [str(n) for n in numbers]
result = ", ".join(string_numbers)  # "1, 2, 3, 4, 5"

# String to list conversion
text = "Hello"
char_list = list(text)  # ['H', 'e', 'l', 'l', 'o']
```

## 8. Advanced String Operations

### 8.1 String Splitting and Joining

```python
# Splitting strings
text = "apple,banana,cherry"
fruits = text.split(",")  # ['apple', 'banana', 'cherry']

# Splitting with different separators
sentence = "Hello world from Python"
words = sentence.split()  # ['Hello', 'world', 'from', 'Python']

# Limiting splits
limited = text.split(",", 1)  # ['apple', 'banana,cherry']

# Joining strings
fruits = ['apple', 'banana', 'cherry']
result = ", ".join(fruits)  # "apple, banana, cherry"

# Practical example: CSV processing
def process_csv_line(line):
    """Process a single CSV line."""
    fields = line.strip().split(',')
    return [field.strip() for field in fields]
```

### 8.2 String Encoding and Decoding

```python
# String encoding
text = "Hello, 世界"
encoded = text.encode('utf-8')  # b'Hello, \xe4\xb8\x96\xe7\x95\x8c'

# String decoding
decoded = encoded.decode('utf-8')  # "Hello, 世界"

# Handling encoding errors
try:
    bad_bytes = b'\xff\xfe'
    decoded = bad_bytes.decode('utf-8')
except UnicodeDecodeError as e:
    print(f"Encoding error: {e}")
    # Use error handling
    decoded = bad_bytes.decode('utf-8', errors='ignore')
```

## 9. Practical Examples and Use Cases

### 9.1 Input Validation

```python
def validate_user_input():
    """Demonstrate input validation techniques."""
    
    while True:
        age_input = input("Enter your age: ")
        
        # Check if input is numeric
        if not age_input.isdigit():
            print("Please enter a valid number.")
            continue
        
        age = int(age_input)
        
        # Check age range
        if age < 0 or age > 150:
            print("Please enter a realistic age.")
            continue
        
        return age

def validate_email(email):
    """Simple email validation."""
    if not isinstance(email, str):
        return False
    
    # Basic validation
    if "@" not in email:
        return False
    
    parts = email.split("@")
    if len(parts) != 2:
        return False
    
    local, domain = parts
    
    # Check local part
    if not local or not local.replace(".", "").replace("_", "").isalnum():
        return False
    
    # Check domain part
    if not domain or "." not in domain:
        return False
    
    return True
```

### 9.2 String Manipulation Examples

```python
def format_phone_number(phone):
    """Format a phone number."""
    # Remove all non-digit characters
    digits = ''.join(c for c in phone if c.isdigit())
    
    # Check if we have enough digits
    if len(digits) != 10:
        raise ValueError("Phone number must have 10 digits")
    
    # Format as (XXX) XXX-XXXX
    return f"({digits[:3]}) {digits[3:6]}-{digits[6:]}"

def create_username(first_name, last_name):
    """Create a username from first and last name."""
    # Convert to lowercase and remove spaces
    first = first_name.lower().strip()
    last = last_name.lower().strip()
    
    # Create username
    username = f"{first[0]}{last}"
    
    # Remove any non-alphanumeric characters
    username = ''.join(c for c in username if c.isalnum())
    
    return username

def password_strength(password):
    """Check password strength."""
    if len(password) < 8:
        return "Weak: Password must be at least 8 characters"
    
    has_upper = any(c.isupper() for c in password)
    has_lower = any(c.islower() for c in password)
    has_digit = any(c.isdigit() for c in password)
    has_special = any(c in "!@#$%^&*" for c in password)
    
    strength = sum([has_upper, has_lower, has_digit, has_special])
    
    if strength >= 3:
        return "Strong"
    elif strength >= 2:
        return "Medium"
    else:
        return "Weak"
```

## 10. Performance Considerations

### 10.1 String Concatenation Performance

```python
import time

def inefficient_concatenation(strings):
    """Inefficient string concatenation."""
    result = ""
    for s in strings:
        result += s  # Creates new string each time
    return result

def efficient_concatenation(strings):
    """Efficient string concatenation."""
    return "".join(strings)  # More efficient

# Performance comparison
strings = ["hello"] * 10000

# Measure inefficient method
start = time.time()
result1 = inefficient_concatenation(strings)
time1 = time.time() - start

# Measure efficient method
start = time.time()
result2 = efficient_concatenation(strings)
time2 = time.time() - start

print(f"Inefficient method: {time1:.4f} seconds")
print(f"Efficient method: {time2:.4f} seconds")
```

### 10.2 Memory Efficiency

```python
# Memory-efficient string operations
def process_large_text(text):
    """Process large text efficiently."""
    # Use generator expression for memory efficiency
    words = (word.lower().strip() for word in text.split())
    return [word for word in words if len(word) > 3]

# Using string methods vs regular expressions
import re

def clean_text_methods(text):
    """Clean text using string methods."""
    return text.strip().lower().replace("  ", " ")

def clean_text_regex(text):
    """Clean text using regex (when needed)."""
    return re.sub(r'\s+', ' ', text.strip().lower())
```

## Review Questions

### Theoretical Questions

1. **Explain the difference between `==` and `is` operators in Python.**

**Answer:** 
- `==` compares **values** (equality)
- `is` compares **identity** (same object in memory)

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)  # True (same values)
print(a is b)  # False (different objects)
print(a is c)  # True (same object)
```

2. **What is the difference between mutable and immutable data types? Give examples.**

**Answer:**
- **Immutable:** Cannot be changed after creation (int, float, str, tuple)
- **Mutable:** Can be modified after creation (list, dict, set)

```python
# Immutable example
x = "hello"
x[0] = "H"  # TypeError: 'str' object does not support item assignment

# Mutable example
lst = [1, 2, 3]
lst[0] = 10  # Works fine
```

3. **Explain Python's dynamic typing and its implications.**

**Answer:**
Dynamic typing means variable types are determined at runtime, not compile time:
- **Advantages:** Flexibility, faster development, less verbose code
- **Disadvantages:** Runtime errors, less IDE support, potential performance impact
- **Implications:** Need for careful testing, type hints for documentation

### Practical Questions

4. **How would you handle floating-point precision issues in Python?**

**Answer:**
```python
from decimal import Decimal, getcontext

# Set precision
getcontext().prec = 10

# Use Decimal for precise calculations
a = Decimal('0.1')
b = Decimal('0.2')
result = a + b  # Decimal('0.3')

# For comparison with tolerance
def float_equal(a, b, tolerance=1e-9):
    return abs(a - b) < tolerance
```

5. **Write a function that validates and formats a credit card number.**

**Answer:**
```python
def format_credit_card(number):
    """Format and validate credit card number."""
    # Remove all non-digit characters
    digits = ''.join(c for c in str(number) if c.isdigit())
    
    # Validate length
    if len(digits) not in [13, 14, 15, 16]:
        raise ValueError("Invalid credit card length")
    
    # Format with spaces every 4 digits
    formatted = ' '.join(digits[i:i+4] for i in range(0, len(digits), 4))
    return formatted

def luhn_check(number):
    """Validate credit card using Luhn algorithm."""
    digits = [int(d) for d in str(number) if d.isdigit()]
    
    for i in range(len(digits) - 2, -1, -2):
        digits[i] *= 2
        if digits[i] > 9:
            digits[i] -= 9
    
    return sum(digits) % 10 == 0
```

### Advanced Questions

6. **Explain the concept of string interning in Python and its performance implications.**

**Answer:**
String interning is Python's optimization where identical strings share the same memory location:
- **Automatic interning:** Small strings, identifiers, compile-time strings
- **Manual interning:** Using `sys.intern()`
- **Performance benefit:** Faster comparison (identity vs. value)
- **Memory benefit:** Reduced memory usage for duplicate strings

```python
import sys

# Automatic interning
a = "hello"
b = "hello"
print(a is b)  # True (usually)

# Manual interning
x = sys.intern("dynamic_string")
y = sys.intern("dynamic_string")
print(x is y)  # True
```

7. **How does Python handle Unicode strings, and what are the best practices?**

**Answer:**
Python 3 uses Unicode (UTF-8) by default:
- **Internal representation:** All strings are Unicode
- **Encoding/decoding:** Convert between strings and bytes
- **Best practices:**
  - Always specify encoding when reading/writing files
  - Use UTF-8 encoding when possible
  - Handle encoding errors gracefully
  - Use raw strings for regex patterns

```python
# Best practices
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()

# Error handling
try:
    decoded = bytes_data.decode('utf-8')
except UnicodeDecodeError:
    decoded = bytes_data.decode('utf-8', errors='replace')
```

8. **Explain the performance differences between different string formatting methods.**

**Answer:**
Performance ranking (fastest to slowest):
1. **f-strings (Python 3.6+):** Fastest, most readable
2. **str.format():** Good performance, flexible
3. **% formatting:** Slowest, legacy

```python
import timeit

name = "Alice"
age = 30

# f-string (fastest)
def f_string():
    return f"Name: {name}, Age: {age}"

# str.format()
def str_format():
    return "Name: {}, Age: {}".format(name, age)

# % formatting (slowest)
def percent_format():
    return "Name: %s, Age: %d" % (name, age)
```

## Practical Exercises

### Exercise 1: Text Analyzer
Create a program that analyzes text and provides statistics like word count, character count, average word length, and most common words.

### Exercise 2: Number Base Converter
Write a program that converts numbers between different bases (binary, octal, decimal, hexadecimal).

### Exercise 3: String Cipher
Implement a simple Caesar cipher that can encode and decode messages.

### Exercise 4: Data Validator
Create a comprehensive data validation system that can validate emails, phone numbers, credit cards, and passwords.

---

## Next Steps

In the next module, we'll explore control flow structures including conditional statements and loops, which will allow you to create more complex and interactive programs. You'll learn how to make decisions in your code and repeat operations efficiently.

## Additional Resources

- [Python Data Types Documentation](https://docs.python.org/3/library/stdtypes.html)
- [PEP 8 Style Guide](https://www.python.org/dev/peps/pep-0008/)
- [Python String Methods](https://docs.python.org/3/library/stdtypes.html#string-methods)
- [Unicode HOWTO](https://docs.python.org/3/howto/unicode.html)
- [Python Numeric Types](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)

---

*This module establishes the foundation for all Python programming concepts. Master these basics as they form the building blocks for more advanced topics.*
