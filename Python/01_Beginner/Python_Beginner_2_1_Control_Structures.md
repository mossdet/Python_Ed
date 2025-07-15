# Python Beginner 2.1: Control Structures

## Table of Contents
1. [Learning Objectives](#learning-objectives)
2. [Conditional Statements](#1-conditional-statements)
3. [Loops](#2-loops)
4. [Nested Loops and Conditions](#3-nested-loops-and-conditions)
5. [Advanced Control Flow Patterns](#4-advanced-control-flow-patterns)
6. [Practical Examples](#5-practical-examples)
7. [Performance and Best Practices](#6-performance-and-best-practices)
8. [Error Handling in Control Structures](#7-error-handling-in-control-structures)
9. [Review Questions](#review-questions)
10. [Practical Exercises](#practical-exercises)
11. [Next Steps](#next-steps)
12. [Additional Resources](#additional-resources)

## Learning Objectives
By the end of this module, you will be able to:
- Use conditional statements (if/elif/else) to make decisions in your code
- Implement various types of loops (for and while) for repetitive tasks
- Control loop execution with break and continue statements
- Create nested loops and conditional structures
- Apply best practices for writing clean, efficient control flow code
- Handle edge cases and avoid common pitfalls in control structures

## 1. Conditional Statements

### 1.1 Basic if Statement

The `if` statement allows your program to make decisions based on conditions:

```python
# Basic if statement
age = 18

if age >= 18:
    print("You are an adult")
    print("You can vote")

# The code here runs regardless of the condition
print("Program continues...")
```

**Key Points:**
- Condition must evaluate to `True` or `False`
- Code block is indented (4 spaces recommended)
- Colon (`:`) is required after the condition

### 1.2 if-else Statement

```python
# if-else statement
temperature = 25

if temperature > 30:
    print("It's hot outside")
    print("Wear light clothing")
else:
    print("It's not too hot")
    print("Normal clothing is fine")

# Ternary operator (conditional expression)
weather = "sunny" if temperature > 25 else "cloudy"
print(f"Weather: {weather}")
```

### 1.3 if-elif-else Statement

```python
# Multiple conditions with elif
grade = 85

if grade >= 90:
    letter_grade = "A"
    message = "Excellent!"
elif grade >= 80:
    letter_grade = "B"
    message = "Good job!"
elif grade >= 70:
    letter_grade = "C"
    message = "Satisfactory"
elif grade >= 60:
    letter_grade = "D"
    message = "Needs improvement"
else:
    letter_grade = "F"
    message = "Failed"

print(f"Grade: {letter_grade} - {message}")
```

### 1.4 Complex Conditions

```python
# Logical operators in conditions
age = 25
has_license = True
has_car = False

# AND operator
if age >= 18 and has_license:
    print("You can drive")

# OR operator
if has_car or age >= 25:
    print("You might be able to rent a car")

# NOT operator
if not has_car:
    print("You don't have a car")

# Complex condition
if (age >= 18 and has_license) or (age >= 25 and not has_car):
    print("Complex condition met")
```

### 1.5 Membership and Identity Operators

```python
# Membership operators (in, not in)
fruits = ["apple", "banana", "orange"]
user_choice = "apple"

if user_choice in fruits:
    print(f"{user_choice} is available")

if "grape" not in fruits:
    print("Grape is not available")

# Identity operators (is, is not)
x = None
if x is None:
    print("x is None")

y = [1, 2, 3]
z = [1, 2, 3]
if y is not z:
    print("y and z are different objects")
```

## 2. Loops

### 2.1 for Loop

The `for` loop is used to iterate over sequences:

```python
# Iterating over a list
fruits = ["apple", "banana", "orange"]
for fruit in fruits:
    print(f"I like {fruit}")

# Iterating over a string
word = "Python"
for letter in word:
    print(letter)

# Iterating over a range
for i in range(5):
    print(f"Number: {i}")

# Range with start, stop, and step
for i in range(1, 10, 2):
    print(f"Odd number: {i}")
```

### 2.2 while Loop

The `while` loop continues as long as a condition is true:

```python
# Basic while loop
count = 0
while count < 5:
    print(f"Count: {count}")
    count += 1

# User input validation
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input.lower() == 'quit':
        break
    print(f"You entered: {user_input}")

# Countdown example
countdown = 5
while countdown > 0:
    print(f"Countdown: {countdown}")
    countdown -= 1
print("Blast off!")
```

### 2.3 Loop Control Statements

#### 2.3.1 break Statement

```python
# break - exit the loop immediately
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for number in numbers:
    if number == 7:
        print("Found 7, breaking the loop")
        break
    print(number)

# Search example
search_list = ["apple", "banana", "orange", "grape"]
target = "orange"

for item in search_list:
    if item == target:
        print(f"Found {target}!")
        break
else:
    print(f"{target} not found")
```

#### 2.3.2 continue Statement

```python
# continue - skip the rest of current iteration
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for number in numbers:
    if number % 2 == 0:
        continue  # Skip even numbers
    print(f"Odd number: {number}")

# Processing with conditions
data = [1, -2, 3, -4, 5, 0, 6]

for value in data:
    if value <= 0:
        continue  # Skip non-positive values
    print(f"Processing positive value: {value}")
```

### 2.4 else Clause in Loops

```python
# else clause executes if loop completes normally (no break)
numbers = [2, 4, 6, 8, 10]

for number in numbers:
    if number % 2 == 1:
        print("Found odd number")
        break
else:
    print("All numbers are even")

# Search with else clause
students = ["Alice", "Bob", "Charlie", "David"]
search_name = "Eve"

for student in students:
    if student == search_name:
        print(f"Found {search_name}")
        break
else:
    print(f"{search_name} not found in the list")
```

## 3. Nested Loops and Conditions

### 3.1 Nested Loops

```python
# Nested for loops
for i in range(3):
    for j in range(3):
        print(f"({i}, {j})")

# Multiplication table
for i in range(1, 6):
    for j in range(1, 6):
        result = i * j
        print(f"{i} × {j} = {result:2d}", end="  ")
    print()  # New line after each row

# Matrix processing
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for row in matrix:
    for element in row:
        print(element, end=" ")
    print()
```

### 3.2 Nested Conditions

```python
# Nested if statements
age = 25
has_job = True
salary = 50000

if age >= 18:
    print("Adult")
    if has_job:
        print("Employed")
        if salary >= 40000:
            print("Good salary")
        else:
            print("Low salary")
    else:
        print("Unemployed")
else:
    print("Minor")

# Alternative approach with logical operators
if age >= 18 and has_job and salary >= 40000:
    print("Adult with good job and salary")
```

### 3.3 Loop Control in Nested Structures

```python
# Break from nested loops
found = False
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

target = 5

for i, row in enumerate(matrix):
    for j, element in enumerate(row):
        if element == target:
            print(f"Found {target} at position ({i}, {j})")
            found = True
            break
    if found:
        break

# Using functions to break from nested loops
def find_in_matrix(matrix, target):
    for i, row in enumerate(matrix):
        for j, element in enumerate(row):
            if element == target:
                return (i, j)
    return None

position = find_in_matrix(matrix, 5)
if position:
    print(f"Found at position: {position}")
```

## 4. Advanced Control Flow Patterns

### 4.1 Loop Patterns

```python
# Enumerate - getting index and value
fruits = ["apple", "banana", "orange"]
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# Zip - iterating over multiple sequences
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
cities = ["New York", "London", "Tokyo"]

for name, age, city in zip(names, ages, cities):
    print(f"{name} is {age} years old and lives in {city}")

# Reversed iteration
for i in reversed(range(5)):
    print(f"Countdown: {i}")

# Sorted iteration
names = ["Charlie", "Alice", "Bob"]
for name in sorted(names):
    print(name)
```

### 4.2 List Comprehensions (Preview)

```python
# Basic list comprehension
numbers = [1, 2, 3, 4, 5]
squares = [x**2 for x in numbers]
print(squares)  # [1, 4, 9, 16, 25]

# List comprehension with condition
even_squares = [x**2 for x in numbers if x % 2 == 0]
print(even_squares)  # [4, 16]

# Nested list comprehension
matrix = [[i*j for j in range(1, 4)] for i in range(1, 4)]
print(matrix)  # [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

## 5. Practical Examples

### 5.1 Menu System

```python
def display_menu():
    print("\n--- Main Menu ---")
    print("1. Add item")
    print("2. View items")
    print("3. Remove item")
    print("4. Exit")

def main():
    items = []
    
    while True:
        display_menu()
        choice = input("Enter your choice (1-4): ")
        
        if choice == '1':
            item = input("Enter item to add: ")
            items.append(item)
            print(f"Added: {item}")
        
        elif choice == '2':
            if items:
                print("Items:")
                for i, item in enumerate(items, 1):
                    print(f"{i}. {item}")
            else:
                print("No items found")
        
        elif choice == '3':
            if items:
                print("Items:")
                for i, item in enumerate(items, 1):
                    print(f"{i}. {item}")
                
                try:
                    index = int(input("Enter item number to remove: ")) - 1
                    if 0 <= index < len(items):
                        removed = items.pop(index)
                        print(f"Removed: {removed}")
                    else:
                        print("Invalid item number")
                except ValueError:
                    print("Please enter a valid number")
            else:
                print("No items to remove")
        
        elif choice == '4':
            print("Goodbye!")
            break
        
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
```

### 5.2 Number Guessing Game

```python
import random

def guessing_game():
    print("Welcome to the Number Guessing Game!")
    print("I'm thinking of a number between 1 and 100.")
    
    secret_number = random.randint(1, 100)
    attempts = 0
    max_attempts = 7
    
    while attempts < max_attempts:
        try:
            guess = int(input(f"Attempt {attempts + 1}/{max_attempts}: Enter your guess: "))
            attempts += 1
            
            if guess < 1 or guess > 100:
                print("Please enter a number between 1 and 100.")
                continue
            
            if guess == secret_number:
                print(f"Congratulations! You guessed it in {attempts} attempts!")
                break
            elif guess < secret_number:
                print("Too low!")
            else:
                print("Too high!")
            
            remaining = max_attempts - attempts
            if remaining > 0:
                print(f"You have {remaining} attempts left.")
        
        except ValueError:
            print("Please enter a valid number.")
    
    else:
        print(f"Game over! The number was {secret_number}.")
    
    # Ask to play again
    play_again = input("Do you want to play again? (y/n): ")
    if play_again.lower() == 'y':
        guessing_game()

if __name__ == "__main__":
    guessing_game()
```

### 5.3 Pattern Generator

```python
def print_triangle(height):
    """Print a triangle pattern"""
    for i in range(1, height + 1):
        spaces = ' ' * (height - i)
        stars = '*' * (2 * i - 1)
        print(spaces + stars)

def print_diamond(size):
    """Print a diamond pattern"""
    # Upper half
    for i in range(size):
        spaces = ' ' * (size - i - 1)
        stars = '*' * (2 * i + 1)
        print(spaces + stars)
    
    # Lower half
    for i in range(size - 2, -1, -1):
        spaces = ' ' * (size - i - 1)
        stars = '*' * (2 * i + 1)
        print(spaces + stars)

def print_multiplication_table(n):
    """Print multiplication table"""
    print(f"Multiplication Table for {n}:")
    print("-" * 30)
    for i in range(1, 11):
        result = n * i
        print(f"{n} × {i:2d} = {result:3d}")

# Usage examples
print_triangle(5)
print("\n" + "="*20 + "\n")
print_diamond(5)
print("\n" + "="*20 + "\n")
print_multiplication_table(7)
```

## 6. Performance and Best Practices

### 6.1 Efficient Loop Patterns

```python
# Avoid modifying list while iterating
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Wrong way - modifying list while iterating
# for number in numbers:
#     if number % 2 == 0:
#         numbers.remove(number)  # This can skip elements

# Correct way - iterate over copy
for number in numbers[:]:  # Create a copy
    if number % 2 == 0:
        numbers.remove(number)

# Better way - use list comprehension
numbers = [num for num in numbers if num % 2 != 0]

# Or use filter
numbers = list(filter(lambda x: x % 2 != 0, numbers))
```

### 6.2 Memory-Efficient Iteration

```python
# Use generators for large datasets
def fibonacci_generator(n):
    a, b = 0, 1
    count = 0
    while count < n:
        yield a
        a, b = b, a + b
        count += 1

# Memory efficient - generates values on demand
for fib in fibonacci_generator(10):
    print(fib)

# Instead of creating large lists
def process_large_data():
    # Don't do this for large datasets
    # data = [expensive_function(i) for i in range(1000000)]
    
    # Do this instead
    for i in range(1000000):
        result = expensive_function(i)
        process_result(result)

def expensive_function(x):
    return x ** 2

def process_result(result):
    print(result)
```

### 6.3 Avoiding Common Pitfalls

```python
# Pitfall 1: Infinite loops
def safe_while_loop():
    count = 0
    max_iterations = 1000
    
    while count < max_iterations:
        # Your loop logic here
        count += 1
        
        # Safety check
        if count >= max_iterations:
            print("Warning: Maximum iterations reached")
            break

# Pitfall 2: Off-by-one errors
def print_numbers(n):
    # Print numbers from 1 to n (inclusive)
    for i in range(1, n + 1):  # Note: n + 1 to include n
        print(i)

# Pitfall 3: Nested loop performance
def efficient_nested_search(matrix, target):
    # Early termination
    for i, row in enumerate(matrix):
        for j, element in enumerate(row):
            if element == target:
                return (i, j)
    return None

# Pitfall 4: Mutable default arguments
def process_items(items, processed=None):
    if processed is None:
        processed = []  # Create new list each time
    
    for item in items:
        processed.append(item.upper())
    
    return processed
```

## 7. Error Handling in Control Structures

### 7.1 Input Validation

```python
def get_valid_integer(prompt, min_val=None, max_val=None):
    """Get valid integer input with optional range validation"""
    while True:
        try:
            value = int(input(prompt))
            
            if min_val is not None and value < min_val:
                print(f"Value must be at least {min_val}")
                continue
            
            if max_val is not None and value > max_val:
                print(f"Value must be at most {max_val}")
                continue
            
            return value
        
        except ValueError:
            print("Please enter a valid integer")

def get_valid_choice(prompt, valid_choices):
    """Get valid choice from a list of options"""
    while True:
        choice = input(prompt).lower()
        if choice in valid_choices:
            return choice
        print(f"Please enter one of: {', '.join(valid_choices)}")

# Usage
age = get_valid_integer("Enter your age: ", min_val=0, max_val=120)
choice = get_valid_choice("Choose (yes/no): ", ['yes', 'no', 'y', 'n'])
```

### 7.2 Robust Loop Structures

```python
def safe_file_processor(filenames):
    """Process multiple files with error handling"""
    successful = 0
    failed = 0
    
    for filename in filenames:
        try:
            with open(filename, 'r') as file:
                content = file.read()
                # Process content
                print(f"Processed {filename}: {len(content)} characters")
                successful += 1
        
        except FileNotFoundError:
            print(f"Error: File '{filename}' not found")
            failed += 1
        
        except PermissionError:
            print(f"Error: Permission denied for '{filename}'")
            failed += 1
        
        except Exception as e:
            print(f"Unexpected error processing '{filename}': {e}")
            failed += 1
    
    print(f"\nSummary: {successful} successful, {failed} failed")
```

## Review Questions

### Theoretical Questions

1. **What is the difference between `break` and `continue` statements? Provide examples.**

**Answer:**
- `break`: Exits the loop completely
- `continue`: Skips the rest of the current iteration and moves to the next

```python
# break example
for i in range(10):
    if i == 5:
        break  # Exits loop when i is 5
    print(i)  # Prints 0, 1, 2, 3, 4

# continue example
for i in range(10):
    if i % 2 == 0:
        continue  # Skips even numbers
    print(i)  # Prints 1, 3, 5, 7, 9
```

2. **Explain the purpose of the `else` clause in loops. When does it execute?**

**Answer:**
The `else` clause in loops executes when the loop completes normally (without encountering a `break`):

```python
# else executes (no break)
for i in range(5):
    print(i)
else:
    print("Loop completed normally")

# else doesn't execute (break encountered)
for i in range(5):
    if i == 3:
        break
    print(i)
else:
    print("This won't print")
```

3. **What are the differences between `for` and `while` loops? When should you use each?**

**Answer:**
- **for loop**: Used when you know the number of iterations or iterating over a sequence
- **while loop**: Used when the number of iterations depends on a condition

```python
# for loop - known iterations
for i in range(10):
    print(i)

# while loop - condition-based
user_input = ""
while user_input != "quit":
    user_input = input("Enter command: ")
```

### Practical Questions

4. **Write a function that finds the first duplicate in a list. If no duplicates exist, return None.**

**Answer:**
```python
def find_first_duplicate(lst):
    """Find the first duplicate element in a list."""
    seen = set()
    for item in lst:
        if item in seen:
            return item
        seen.add(item)
    return None

# Test cases
print(find_first_duplicate([1, 2, 3, 2, 4]))  # 2
print(find_first_duplicate([1, 2, 3, 4, 5]))  # None
print(find_first_duplicate(['a', 'b', 'c', 'a']))  # 'a'
```

5. **Create a function that prints a right-angled triangle of a specified height using asterisks.**

**Answer:**
```python
def print_right_triangle(height):
    """Print a right-angled triangle of specified height."""
    for i in range(1, height + 1):
        print('*' * i)

def print_inverted_triangle(height):
    """Print an inverted right-angled triangle."""
    for i in range(height, 0, -1):
        print('*' * i)

# Usage
print_right_triangle(5)
print()
print_inverted_triangle(5)
```

### Advanced Questions

6. **Implement a function that checks if a number is prime using efficient loop control.**

**Answer:**
```python
def is_prime(n):
    """Check if a number is prime."""
    if n < 2:
        return False
    
    if n == 2:
        return True
    
    if n % 2 == 0:
        return False
    
    # Check odd divisors up to sqrt(n)
    for i in range(3, int(n**0.5) + 1, 2):
        if n % i == 0:
            return False
    
    return True

def find_primes_up_to(limit):
    """Find all prime numbers up to a limit."""
    primes = []
    for num in range(2, limit + 1):
        if is_prime(num):
            primes.append(num)
    return primes

# Test
print(find_primes_up_to(50))
```

7. **Create a nested loop that generates a multiplication table for numbers 1-10, but skips multiples of 5.**

**Answer:**
```python
def multiplication_table_skip_five():
    """Generate multiplication table skipping multiples of 5."""
    print("Multiplication Table (skipping multiples of 5):")
    print("-" * 50)
    
    for i in range(1, 11):
        for j in range(1, 11):
            result = i * j
            if result % 5 == 0:
                continue
            print(f"{i:2d} × {j:2d} = {result:3d}", end="  ")
        print()

multiplication_table_skip_five()
```

8. **Write a function that validates a password based on multiple criteria using control structures.**

**Answer:**
```python
def validate_password(password):
    """Validate password based on multiple criteria."""
    errors = []
    
    # Check length
    if len(password) < 8:
        errors.append("Password must be at least 8 characters long")
    
    # Check for uppercase letter
    has_upper = False
    for char in password:
        if char.isupper():
            has_upper = True
            break
    if not has_upper:
        errors.append("Password must contain at least one uppercase letter")
    
    # Check for lowercase letter
    has_lower = False
    for char in password:
        if char.islower():
            has_lower = True
            break
    if not has_lower:
        errors.append("Password must contain at least one lowercase letter")
    
    # Check for digit
    has_digit = False
    for char in password:
        if char.isdigit():
            has_digit = True
            break
    if not has_digit:
        errors.append("Password must contain at least one digit")
    
    # Check for special character
    special_chars = "!@#$%^&*()_+-=[]{}|;:,.<>?"
    has_special = False
    for char in password:
        if char in special_chars:
            has_special = True
            break
    if not has_special:
        errors.append("Password must contain at least one special character")
    
    return len(errors) == 0, errors

# Test
password = "MyPassword123!"
is_valid, errors = validate_password(password)
if is_valid:
    print("Password is valid!")
else:
    print("Password validation failed:")
    for error in errors:
        print(f"- {error}")
```

## Practical Exercises

### Exercise 1: Rock, Paper, Scissors Game
Create a complete Rock, Paper, Scissors game with score tracking and multiple rounds.

### Exercise 2: Prime Number Sieve
Implement the Sieve of Eratosthenes algorithm to find all prime numbers up to a given limit.

### Exercise 3: ASCII Art Generator
Create a program that generates various ASCII art patterns using nested loops.

### Exercise 4: Simple Calculator
Build a calculator that can handle multiple operations in sequence with proper error handling.

---

## Next Steps

In the next module, we'll explore functions in detail, learning how to create reusable code blocks, handle parameters and return values, and understand variable scope. Functions are crucial for writing modular, maintainable code.

## Additional Resources

- [Python Control Flow Documentation](https://docs.python.org/3/tutorial/controlflow.html)
- [Python Loop Techniques](https://docs.python.org/3/tutorial/datastructures.html#looping-techniques)
- [PEP 8 Programming Recommendations](https://www.python.org/dev/peps/pep-0008/#programming-recommendations)
- [Python Performance Tips](https://wiki.python.org/moin/PythonSpeed/PerformanceTips)

---

*Control structures are the backbone of program logic. Master these concepts to create dynamic, responsive programs that can handle complex decision-making and repetitive tasks efficiently.*
