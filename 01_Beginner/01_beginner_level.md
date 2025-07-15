# Python Beginner Level Course

## Course Overview
This beginner-level course is designed to take you from zero Python knowledge to building your first applications. By the end of this course, you'll be comfortable with Python syntax, basic programming concepts, and ready to tackle intermediate topics.

**Duration**: 4-8 weeks  
**Time Commitment**: 1-2 hours daily  
**Prerequisites**: Basic computer literacy and willingness to learn

---

## Module 1: Setting Up and Basic Syntax (Week 1-2)

### Lesson 1.1: Installation & Setup

#### Learning Objectives
- Install Python on your system
- Set up a development environment
- Understand the Python interpreter
- Create and manage virtual environments

#### Content

**Installing Python**
1. Visit [python.org](https://python.org) and download the latest Python version
2. Follow installation instructions for your operating system
3. Verify installation by opening terminal/command prompt and typing:
   ```bash
   python --version
   ```

**Setting up an IDE**
Choose one of these popular options:
- **VS Code**: Lightweight, extensible, great for beginners
- **PyCharm**: Full-featured, excellent for larger projects
- **IDLE**: Comes with Python, simple and straightforward

**Python Interpreter**
- Interactive mode: Type `python` in terminal
- Script mode: Save code in `.py` files and run with `python filename.py`
- Understanding the REPL (Read-Eval-Print Loop)

**Virtual Environments**
```bash
# Create virtual environment
python -m venv myenv

# Activate (Windows)
myenv\Scripts\activate

# Activate (Mac/Linux)
source myenv/bin/activate

# Deactivate
deactivate
```

#### Hands-on Exercise
1. Install Python and verify installation
2. Set up VS Code with Python extension
3. Create your first virtual environment
4. Write and run "Hello, World!" program

### Lesson 1.2: Basic Syntax & Data Types

#### Learning Objectives
- Understand Python variables and naming conventions
- Work with different data types
- Perform basic operations

#### Content

**Variables and Naming**
```python
# Valid variable names
name = "Alice"
age = 25
is_student = True
_private_var = "hidden"

# Naming conventions (PEP 8)
# snake_case for variables and functions
user_name = "john_doe"
calculate_total = lambda x, y: x + y

# UPPER_CASE for constants
PI = 3.14159
MAX_SIZE = 100
```

**Data Types**

**Numbers**
```python
# Integers
count = 42
negative = -17

# Floats
price = 19.99
temperature = -5.5

# Complex numbers
complex_num = 3 + 4j

# Type conversion
int("42")      # String to integer
float(42)      # Integer to float
str(42)        # Integer to string
```

**Strings**
```python
# String creation
single_quotes = 'Hello'
double_quotes = "World"
multiline = """This is a
multiline string"""

# String operations
greeting = "Hello" + " " + "World"  # Concatenation
repeated = "Ha" * 3                 # Repetition: "HaHaHa"
length = len("Python")              # Length: 6

# String methods
text = "Python Programming"
print(text.upper())        # PYTHON PROGRAMMING
print(text.lower())        # python programming
print(text.split())        # ['Python', 'Programming']
```

**Booleans**
```python
is_valid = True
is_complete = False

# Boolean operations
result = True and False    # False
result = True or False     # True
result = not True          # False

# Comparison operators
5 > 3          # True
10 == 10       # True
"hello" != "world"  # True
```

#### Hands-on Exercise
1. Create variables of different data types
2. Perform string operations and formatting
3. Practice type conversions
4. Write a program that calculates area of a rectangle

### Lesson 1.3: Input/Output and Comments

#### Learning Objectives
- Handle user input
- Format output effectively
- Write clear comments and documentation

#### Content

**Input and Output**
```python
# Getting user input
name = input("Enter your name: ")
age = int(input("Enter your age: "))

# Formatted output
print(f"Hello, {name}! You are {age} years old.")
print("Hello, {}! You are {} years old.".format(name, age))
print("Hello, %s! You are %d years old." % (name, age))
```

**Comments**
```python
# Single line comment
print("Hello, World!")  # End of line comment

"""
Multi-line comment
or docstring
"""

def calculate_area(length, width):
    """
    Calculate the area of a rectangle.
    
    Args:
        length (float): The length of the rectangle
        width (float): The width of the rectangle
    
    Returns:
        float: The area of the rectangle
    """
    return length * width
```

#### Hands-on Exercise
1. Create an interactive program that asks for user information
2. Practice different string formatting methods
3. Add meaningful comments to your code

---

## Module 2: Control Flow and Functions (Week 3-4)

### Lesson 2.1: Conditional Statements

#### Learning Objectives
- Use if/elif/else statements
- Understand boolean logic
- Handle multiple conditions

#### Content

**Basic Conditional Statements**
```python
# Simple if statement
age = 18
if age >= 18:
    print("You are an adult")

# if-else statement
temperature = 25
if temperature > 30:
    print("It's hot outside")
else:
    print("It's not too hot")

# if-elif-else statement
score = 85
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"
print(f"Your grade is: {grade}")
```

**Complex Conditions**
```python
# Logical operators
age = 25
has_license = True

if age >= 18 and has_license:
    print("You can drive")

# Multiple conditions
weather = "sunny"
temperature = 22

if weather == "sunny" and temperature > 20:
    print("Great day for a picnic!")
elif weather == "rainy" or temperature < 10:
    print("Better stay inside")
```

#### Hands-on Exercise
1. Create a program that determines if a year is a leap year
2. Build a simple calculator that handles different operations
3. Make a program that categorizes ages into different groups

### Lesson 2.2: Loops

#### Learning Objectives
- Use for loops for iteration
- Implement while loops
- Control loop execution with break and continue

#### Content

**For Loops**
```python
# Iterating over a range
for i in range(5):
    print(i)  # Prints 0, 1, 2, 3, 4

# Iterating over a list
fruits = ["apple", "banana", "orange"]
for fruit in fruits:
    print(fruit)

# Iterating with index
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# Nested loops
for i in range(3):
    for j in range(3):
        print(f"({i}, {j})")
```

**While Loops**
```python
# Basic while loop
count = 0
while count < 5:
    print(count)
    count += 1

# While loop with user input
password = ""
while password != "secret":
    password = input("Enter password: ")
print("Access granted!")
```

**Loop Control**
```python
# Break statement
for i in range(10):
    if i == 5:
        break
    print(i)  # Prints 0, 1, 2, 3, 4

# Continue statement
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)  # Prints 1, 3, 5, 7, 9
```

#### Hands-on Exercise
1. Create a multiplication table using nested loops
2. Build a number guessing game using while loops
3. Process a list of numbers and skip negative values

### Lesson 2.3: Functions

#### Learning Objectives
- Define and call functions
- Understand parameters and arguments
- Work with return values and scope

#### Content

**Basic Functions**
```python
# Function definition
def greet():
    print("Hello, World!")

# Function call
greet()

# Function with parameters
def greet_person(name):
    print(f"Hello, {name}!")

greet_person("Alice")

# Function with return value
def add_numbers(a, b):
    return a + b

result = add_numbers(5, 3)
print(result)  # 8
```

**Advanced Function Features**
```python
# Default parameters
def greet_with_title(name, title="Mr."):
    print(f"Hello, {title} {name}!")

greet_with_title("Smith")           # Hello, Mr. Smith!
greet_with_title("Johnson", "Dr.")  # Hello, Dr. Johnson!

# Variable arguments
def sum_all(*numbers):
    return sum(numbers)

result = sum_all(1, 2, 3, 4, 5)  # 15

# Keyword arguments
def create_profile(name, age, city="Unknown"):
    return f"Name: {name}, Age: {age}, City: {city}"

profile = create_profile(name="Alice", age=30, city="New York")
```

**Scope and Variables**
```python
# Global vs Local scope
global_var = "I'm global"

def my_function():
    local_var = "I'm local"
    print(global_var)  # Accessible
    print(local_var)   # Accessible

my_function()
print(global_var)    # Accessible
# print(local_var)   # Error! Not accessible outside function
```

**Lambda Functions**
```python
# Lambda function (anonymous function)
square = lambda x: x ** 2
print(square(5))  # 25

# Using lambda with built-in functions
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # [1, 4, 9, 16, 25]
```

#### Hands-on Exercise
1. Create a function that calculates the factorial of a number
2. Build a function that determines if a number is prime
3. Write a function that converts temperature between Celsius and Fahrenheit

---

## Module 3: Data Structures (Week 5-6)

### Lesson 3.1: Lists

#### Learning Objectives
- Create and manipulate lists
- Understand list methods and operations
- Work with nested lists

#### Content

**List Basics**
```python
# Creating lists
fruits = ["apple", "banana", "orange"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]
empty_list = []

# List indexing (0-based)
print(fruits[0])     # apple
print(fruits[-1])    # orange (last element)

# List slicing
print(numbers[1:4])  # [2, 3, 4]
print(numbers[:3])   # [1, 2, 3]
print(numbers[2:])   # [3, 4, 5]
```

**List Methods**
```python
# Adding elements
fruits.append("grape")          # Add to end
fruits.insert(1, "strawberry")  # Insert at index 1

# Removing elements
fruits.remove("banana")         # Remove first occurrence
last_fruit = fruits.pop()       # Remove and return last element
first_fruit = fruits.pop(0)     # Remove and return element at index 0

# Other useful methods
fruits.sort()                   # Sort in place
fruits.reverse()                # Reverse in place
count = fruits.count("apple")   # Count occurrences
index = fruits.index("orange")  # Find index of element
```

**List Operations**
```python
# List concatenation
list1 = [1, 2, 3]
list2 = [4, 5, 6]
combined = list1 + list2        # [1, 2, 3, 4, 5, 6]

# List repetition
repeated = [0] * 5              # [0, 0, 0, 0, 0]

# List comprehension (preview)
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

#### Hands-on Exercise
1. Create a shopping list program with add/remove functionality
2. Build a program that finds the largest and smallest numbers in a list
3. Implement a simple gradebook system

### Lesson 3.2: Tuples and Sets

#### Learning Objectives
- Understand tuple immutability and use cases
- Work with sets for unique elements
- Perform set operations

#### Content

**Tuples**
```python
# Creating tuples
coordinates = (10, 20)
colors = ("red", "green", "blue")
single_item = (42,)  # Note the comma for single-item tuple

# Tuple unpacking
x, y = coordinates
print(f"x: {x}, y: {y}")

# Tuple as function return
def get_name_age():
    return "Alice", 25

name, age = get_name_age()

# Named tuples (from collections)
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)
```

**Sets**
```python
# Creating sets
fruits = {"apple", "banana", "orange"}
numbers = {1, 2, 3, 4, 5}
empty_set = set()  # Note: {} creates an empty dictionary

# Set operations
fruits.add("grape")
fruits.remove("banana")  # Raises error if not found
fruits.discard("kiwi")   # Safe removal (no error if not found)

# Set operations
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

union = set1 | set2          # {1, 2, 3, 4, 5, 6}
intersection = set1 & set2    # {3, 4}
difference = set1 - set2      # {1, 2}
symmetric_diff = set1 ^ set2  # {1, 2, 5, 6}
```

#### Hands-on Exercise
1. Create a program that removes duplicates from a list using sets
2. Build a program that finds common elements between two lists
3. Implement a simple voting system that prevents duplicate votes

### Lesson 3.3: Dictionaries

#### Learning Objectives
- Create and manipulate dictionaries
- Understand key-value relationships
- Iterate through dictionaries

#### Content

**Dictionary Basics**
```python
# Creating dictionaries
student = {
    "name": "Alice",
    "age": 20,
    "grade": "A",
    "courses": ["Math", "Science"]
}

# Accessing values
print(student["name"])           # Alice
print(student.get("age"))        # 20
print(student.get("phone", "N/A"))  # N/A (default value)

# Modifying dictionaries
student["age"] = 21              # Update existing key
student["phone"] = "123-456-7890"  # Add new key-value pair
del student["grade"]             # Remove key-value pair
```

**Dictionary Methods**
```python
# Dictionary methods
keys = student.keys()            # Get all keys
values = student.values()        # Get all values
items = student.items()          # Get key-value pairs

# Iterating through dictionaries
for key in student:
    print(f"{key}: {student[key]}")

for key, value in student.items():
    print(f"{key}: {value}")

# Dictionary comprehension
squares = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

**Nested Dictionaries**
```python
# Nested dictionary
school = {
    "students": {
        "alice": {"age": 20, "grade": "A"},
        "bob": {"age": 19, "grade": "B"}
    },
    "teachers": {
        "mr_smith": {"subject": "Math", "room": "101"}
    }
}

# Accessing nested values
alice_grade = school["students"]["alice"]["grade"]
```

#### Hands-on Exercise
1. Create a phonebook program with add/search/delete functionality
2. Build a simple inventory management system
3. Implement a word frequency counter

---

## Module 4: Error Handling and Modules (Week 7-8)

### Lesson 4.1: Exception Handling

#### Learning Objectives
- Understand different types of exceptions
- Use try/except/finally blocks
- Handle multiple exceptions

#### Content

**Basic Exception Handling**
```python
# Basic try-except
try:
    number = int(input("Enter a number: "))
    result = 10 / number
    print(f"Result: {result}")
except ValueError:
    print("Invalid input! Please enter a valid number.")
except ZeroDivisionError:
    print("Cannot divide by zero!")
```

**Advanced Exception Handling**
```python
# Multiple exceptions
try:
    file = open("data.txt", "r")
    data = file.read()
    number = int(data)
except FileNotFoundError:
    print("File not found!")
except ValueError:
    print("Invalid data in file!")
except Exception as e:
    print(f"An unexpected error occurred: {e}")
finally:
    try:
        file.close()
    except:
        pass

# Raising exceptions
def divide_numbers(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero!")
    return a / b
```

**Custom Exceptions**
```python
class CustomError(Exception):
    def __init__(self, message):
        self.message = message
        super().__init__(self.message)

def validate_age(age):
    if age < 0:
        raise CustomError("Age cannot be negative!")
    if age > 150:
        raise CustomError("Age seems unrealistic!")
    return True

try:
    validate_age(-5)
except CustomError as e:
    print(f"Validation error: {e}")
```

#### Hands-on Exercise
1. Create a robust calculator that handles various input errors
2. Build a file reader with proper error handling
3. Implement input validation for a user registration system

### Lesson 4.2: File I/O Operations

#### Learning Objectives
- Read from and write to files
- Work with different file formats
- Handle file operations safely

#### Content

**Basic File Operations**
```python
# Writing to a file
with open("output.txt", "w") as file:
    file.write("Hello, World!\n")
    file.write("This is a test file.\n")

# Reading from a file
with open("output.txt", "r") as file:
    content = file.read()
    print(content)

# Reading line by line
with open("output.txt", "r") as file:
    for line in file:
        print(line.strip())
```

**File Modes**
```python
# Different file modes
# "r" - Read (default)
# "w" - Write (overwrites existing file)
# "a" - Append
# "r+" - Read and write
# "b" - Binary mode (e.g., "rb", "wb")

# Appending to a file
with open("log.txt", "a") as file:
    file.write("New log entry\n")

# Working with binary files
with open("image.jpg", "rb") as file:
    data = file.read()
    print(f"File size: {len(data)} bytes")
```

**Working with CSV Files**
```python
import csv

# Writing CSV
data = [
    ["Name", "Age", "City"],
    ["Alice", "25", "New York"],
    ["Bob", "30", "London"]
]

with open("people.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerows(data)

# Reading CSV
with open("people.csv", "r") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
```

#### Hands-on Exercise
1. Create a simple text-based diary application
2. Build a program that processes and analyzes text files
3. Implement a CSV-based gradebook system

### Lesson 4.3: Modules and Packages

#### Learning Objectives
- Import and use built-in modules
- Create custom modules
- Understand package structure

#### Content

**Built-in Modules**
```python
# Math module
import math
print(math.pi)
print(math.sqrt(16))
print(math.factorial(5))

# Random module
import random
print(random.randint(1, 10))
print(random.choice(["apple", "banana", "orange"]))

# Datetime module
from datetime import datetime, timedelta
now = datetime.now()
print(now.strftime("%Y-%m-%d %H:%M:%S"))

# OS module
import os
print(os.getcwd())  # Current working directory
os.makedirs("new_folder", exist_ok=True)
```

**Creating Custom Modules**
```python
# File: my_utils.py
def calculate_area(length, width):
    """Calculate area of a rectangle."""
    return length * width

def greet(name):
    """Greet a person."""
    return f"Hello, {name}!"

PI = 3.14159

# Using the custom module
# File: main.py
import my_utils
from my_utils import calculate_area

area = calculate_area(5, 3)
greeting = my_utils.greet("Alice")
print(f"Area: {area}")
print(greeting)
```

**Package Management with pip**
```bash
# Installing packages
pip install requests
pip install pandas numpy

# Listing installed packages
pip list

# Installing from requirements.txt
pip install -r requirements.txt

# Creating requirements.txt
pip freeze > requirements.txt
```

#### Hands-on Exercise
1. Create a utility module with mathematical functions
2. Build a package for string manipulation utilities
3. Install and use external packages for a small project

---

## Projects

### Project 1: Calculator
Create a command-line calculator that handles basic arithmetic operations with proper error handling.

**Requirements:**
- Support for +, -, *, / operations
- Handle division by zero
- Input validation
- Continuous operation until user quits

### Project 2: To-Do List Manager
Build a file-based task management system.

**Requirements:**
- Add, remove, and list tasks
- Mark tasks as complete
- Save/load tasks from file
- Search functionality

### Project 3: Number Guessing Game
Create an interactive guessing game.

**Requirements:**
- Generate random number
- Provide hints (too high/low)
- Track number of attempts
- Option to play again

### Project 4: Simple Password Generator
Build a password generator with customizable options.

**Requirements:**
- Variable length passwords
- Include/exclude character types
- Multiple password generation
- Strength validation

### Project 5: Basic File Organizer
Create a program that organizes files in a directory.

**Requirements:**
- Sort files by extension
- Create folders for different file types
- Handle duplicate files
- Logging of operations

---

## Assessment

### Knowledge Check Questions
1. What is the difference between a list and a tuple?
2. How do you handle exceptions in Python?
3. What is the purpose of the `if __name__ == "__main__":` construct?
4. Explain the difference between local and global scope.
5. How do you create and use a virtual environment?

### Practical Assessment
Complete a mini-project that incorporates:
- Functions and proper code organization
- Error handling
- File I/O operations
- At least two data structures
- User interaction

### Next Steps
Upon completion, you should be ready to:
- Move to the Intermediate Level course
- Start contributing to simple open-source projects
- Build more complex applications
- Explore specialized Python libraries

---

## Additional Resources

### Recommended Reading
- "Python Crash Course" by Eric Matthes
- "Automate the Boring Stuff with Python" by Al Sweigart
- Python.org official tutorial

### Online Resources
- Python.org documentation
- Real Python tutorials
- Codecademy Python course
- freeCodeCamp Python course

### Practice Platforms
- HackerRank (Python domain)
- LeetCode (Easy problems)
- Codewars
- Python.org's Python Challenge

### Community
- r/learnpython (Reddit)
- Python Discord servers
- Local Python meetups
- Stack Overflow for questions

---

*Remember: Programming is learned by doing. Make sure to code along with every example and complete all exercises!*
