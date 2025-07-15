# Python Beginner 4.1: Error Handling and Exception Management

## Table of Contents
- [1. Introduction to Error Handling](#1-introduction-to-error-handling)
- [2. Understanding Exceptions](#2-understanding-exceptions)
- [3. Types of Errors](#3-types-of-errors)
- [4. The try/except Block](#4-the-tryexcept-block)
- [5. Handling Specific Exceptions](#5-handling-specific-exceptions)
- [6. The else and finally Clauses](#6-the-else-and-finally-clauses)
- [7. Raising Custom Exceptions](#7-raising-custom-exceptions)
- [8. Exception Hierarchy](#8-exception-hierarchy)
- [9. Best Practices for Error Handling](#9-best-practices-for-error-handling)
- [10. Debugging Techniques](#10-debugging-techniques)
- [11. Logging for Error Tracking](#11-logging-for-error-tracking)
- [12. Common Error Patterns and Solutions](#12-common-error-patterns-and-solutions)
- [13. Practical Examples](#13-practical-examples)
- [14. Review Questions](#14-review-questions)
- [15. Answers to Review Questions](#15-answers-to-review-questions)

---

## 1. Introduction to Error Handling

Error handling is a crucial aspect of programming that allows your programs to gracefully handle unexpected situations and continue running or fail gracefully. In Python, error handling is implemented through exceptions and the try/except mechanism.

### 1.1 Why Error Handling Matters

```python
# Without error handling - program crashes
def divide_numbers(a, b):
    return a / b

result = divide_numbers(10, 0)  # ZeroDivisionError: division by zero
print(result)  # This line never executes
```

```python
# With error handling - program continues
def divide_numbers(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        print("Error: Cannot divide by zero!")
        return None

result = divide_numbers(10, 0)
print(f"Result: {result}")  # This line executes
print("Program continues...")
```

### 1.2 Benefits of Proper Error Handling

1. **Program Stability**: Prevents crashes from unexpected inputs
2. **User Experience**: Provides meaningful error messages
3. **Debugging**: Helps identify and fix issues
4. **Robustness**: Makes programs more reliable
5. **Maintainability**: Easier to track and fix problems

---

## 2. Understanding Exceptions

An exception is an event that occurs during program execution that disrupts the normal flow of instructions. When an exception occurs, Python creates an exception object containing information about the error.

### 2.1 What Happens When an Exception Occurs

```python
# Exception propagation example
def function_c():
    print("Entering function_c")
    result = 10 / 0  # ZeroDivisionError occurs here
    print("This won't execute")
    return result

def function_b():
    print("Entering function_b")
    value = function_c()
    print("This won't execute either")
    return value

def function_a():
    print("Entering function_a")
    result = function_b()
    print("This won't execute")
    return result

# Call the function
try:
    function_a()
except ZeroDivisionError as e:
    print(f"Caught exception: {e}")
    print("Exception type:", type(e).__name__)
```

### 2.2 Exception Objects

```python
try:
    number = int("not_a_number")
except ValueError as e:
    print(f"Exception message: {e}")
    print(f"Exception type: {type(e).__name__}")
    print(f"Exception args: {e.args}")
```

---

## 3. Types of Errors

### 3.1 Syntax Errors

These occur when Python cannot parse your code due to incorrect syntax.

```python
# Syntax errors - caught before execution
# print("Hello World"  # SyntaxError: missing closing parenthesis
# if x == 5  # SyntaxError: missing colon
# def my_function()  # SyntaxError: missing colon
```

### 3.2 Runtime Errors (Exceptions)

These occur during program execution when syntactically correct code encounters an error.

```python
# Runtime error examples
try:
    # NameError: variable not defined
    print(undefined_variable)
except NameError as e:
    print(f"NameError: {e}")

try:
    # TypeError: unsupported operand types
    result = "5" + 3
except TypeError as e:
    print(f"TypeError: {e}")

try:
    # IndexError: list index out of range
    my_list = [1, 2, 3]
    print(my_list[10])
except IndexError as e:
    print(f"IndexError: {e}")
```

### 3.3 Logic Errors

These don't cause exceptions but produce incorrect results.

```python
# Logic error example - no exception raised
def calculate_average(numbers):
    # Logic error: should divide by len(numbers), not len(numbers) - 1
    return sum(numbers) / (len(numbers) - 1)

# This will give wrong result but won't crash
numbers = [10, 20, 30]
avg = calculate_average(numbers)
print(f"Average: {avg}")  # Should be 20, but prints 30
```

---

## 4. The try/except Block

The try/except block is the fundamental mechanism for handling exceptions in Python.

### 4.1 Basic Syntax

```python
try:
    # Code that might raise an exception
    risky_operation()
except ExceptionType:
    # Code to handle the exception
    handle_error()
```

### 4.2 Simple Exception Handling

```python
def safe_division(a, b):
    try:
        result = a / b
        return result
    except ZeroDivisionError:
        print("Cannot divide by zero!")
        return None

# Test the function
print(safe_division(10, 2))   # Output: 5.0
print(safe_division(10, 0))   # Output: Cannot divide by zero! \n None
```

### 4.3 Handling Multiple Exceptions

```python
def process_user_input(user_input):
    try:
        # Try to convert to integer
        number = int(user_input)
        # Try to perform division
        result = 100 / number
        return result
    except ValueError:
        print("Invalid input: Please enter a valid number")
        return None
    except ZeroDivisionError:
        print("Cannot divide by zero!")
        return None

# Test with different inputs
print(process_user_input("5"))      # Output: 20.0
print(process_user_input("abc"))    # Output: Invalid input message
print(process_user_input("0"))      # Output: Cannot divide by zero message
```

### 4.4 Catching Multiple Exceptions in One Block

```python
def robust_calculation(a, b, operation):
    try:
        if operation == "add":
            return a + b
        elif operation == "divide":
            return a / b
        elif operation == "index":
            return a[b]  # Assuming 'a' is a list
    except (TypeError, ValueError, ZeroDivisionError) as e:
        print(f"Calculation error: {e}")
        return None
    except IndexError as e:
        print(f"Index error: {e}")
        return None

# Test cases
print(robust_calculation(10, 2, "divide"))        # Output: 5.0
print(robust_calculation(10, 0, "divide"))        # Output: Calculation error
print(robust_calculation([1, 2], 5, "index"))     # Output: Index error
```

---

## 5. Handling Specific Exceptions

### 5.1 Common Built-in Exceptions

```python
# ValueError: Invalid value for the operation
def get_positive_number():
    try:
        num = float(input("Enter a positive number: "))
        if num <= 0:
            raise ValueError("Number must be positive")
        return num
    except ValueError as e:
        print(f"ValueError: {e}")
        return None

# TypeError: Wrong type for the operation
def concatenate_strings(str1, str2):
    try:
        return str1 + str2
    except TypeError as e:
        print(f"TypeError: Both arguments must be strings - {e}")
        return None

# KeyError: Key not found in dictionary
def get_user_info(user_dict, key):
    try:
        return user_dict[key]
    except KeyError as e:
        print(f"KeyError: Key {e} not found in user information")
        return None

# FileNotFoundError: File doesn't exist
def read_file_content(filename):
    try:
        with open(filename, 'r') as file:
            return file.read()
    except FileNotFoundError as e:
        print(f"FileNotFoundError: {e}")
        return None
```

### 5.2 Exception Information

```python
import sys
import traceback

def detailed_exception_handling():
    try:
        # Simulate various exceptions
        data = {"name": "John", "age": 30}
        print(data["email"])  # KeyError
    except Exception as e:
        print(f"Exception type: {type(e).__name__}")
        print(f"Exception message: {str(e)}")
        print(f"Exception args: {e.args}")
        
        # Get detailed traceback information
        exc_type, exc_value, exc_traceback = sys.exc_info()
        print("\nDetailed traceback:")
        traceback.print_exception(exc_type, exc_value, exc_traceback)

detailed_exception_handling()
```

---

## 6. The else and finally Clauses

### 6.1 The else Clause

The else clause executes only if no exception occurs in the try block.

```python
def file_processor(filename):
    try:
        file = open(filename, 'r')
    except FileNotFoundError:
        print(f"Error: File '{filename}' not found")
    else:
        # This runs only if file was opened successfully
        print(f"File '{filename}' opened successfully")
        content = file.read()
        print(f"File content length: {len(content)} characters")
        file.close()
        return content
    finally:
        print("File processing attempt completed")

# Test the function
content = file_processor("existing_file.txt")
content = file_processor("nonexistent_file.txt")
```

### 6.2 The finally Clause

The finally clause always executes, regardless of whether an exception occurs.

```python
def database_operation():
    connection = None
    try:
        # Simulate database connection
        connection = "Database connection established"
        print(connection)
        
        # Simulate database operation that might fail
        result = 10 / 0  # This will raise ZeroDivisionError
        return result
    except ZeroDivisionError:
        print("Database operation failed: Division by zero")
        return None
    finally:
        # Cleanup always happens
        if connection:
            print("Closing database connection")
            connection = None

result = database_operation()
print(f"Operation result: {result}")
```

### 6.3 Complete try/except/else/finally Example

```python
def comprehensive_file_operation(filename, data_to_write):
    file_handle = None
    try:
        print(f"Attempting to open file: {filename}")
        file_handle = open(filename, 'w')
        print("File opened successfully")
        
        # This might raise an exception if data_to_write is not a string
        file_handle.write(data_to_write)
        print("Data written successfully")
        
    except FileNotFoundError:
        print(f"Error: Could not find or create file {filename}")
        return False
    except TypeError as e:
        print(f"Error: Invalid data type - {e}")
        return False
    except Exception as e:
        print(f"Unexpected error: {e}")
        return False
    else:
        print("File operation completed without errors")
        return True
    finally:
        if file_handle:
            print("Closing file handle")
            file_handle.close()
        print("File operation cleanup completed")

# Test the function
success = comprehensive_file_operation("test.txt", "Hello, World!")
print(f"Operation successful: {success}")
```

---

## 7. Raising Custom Exceptions

### 7.1 Using the raise Statement

```python
def validate_age(age):
    if not isinstance(age, int):
        raise TypeError("Age must be an integer")
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 150:
        raise ValueError("Age seems unrealistic")
    return age

# Test the function
try:
    validate_age(25)      # OK
    validate_age(-5)      # Raises ValueError
except ValueError as e:
    print(f"Validation error: {e}")

try:
    validate_age("25")    # Raises TypeError
except TypeError as e:
    print(f"Type error: {e}")
```

### 7.2 Creating Custom Exception Classes

```python
class BankAccountError(Exception):
    """Base exception class for bank account operations"""
    pass

class InsufficientFundsError(BankAccountError):
    """Raised when trying to withdraw more money than available"""
    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        super().__init__(f"Insufficient funds: Balance {balance}, Attempted withdrawal {amount}")

class InvalidAccountError(BankAccountError):
    """Raised when account number is invalid"""
    pass

class BankAccount:
    def __init__(self, account_number, initial_balance=0):
        if not account_number or len(account_number) < 5:
            raise InvalidAccountError("Account number must be at least 5 characters")
        self.account_number = account_number
        self.balance = initial_balance
    
    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError(self.balance, amount)
        self.balance -= amount
        return self.balance
    
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit amount must be positive")
        self.balance += amount
        return self.balance

# Test custom exceptions
try:
    account = BankAccount("12345", 100)
    account.withdraw(150)  # Should raise InsufficientFundsError
except InsufficientFundsError as e:
    print(f"Transaction failed: {e}")
    print(f"Current balance: {e.balance}")
    print(f"Attempted amount: {e.amount}")
except InvalidAccountError as e:
    print(f"Account error: {e}")
```

---

## 8. Exception Hierarchy

Understanding Python's exception hierarchy helps you write more effective error handling code.

### 8.1 Built-in Exception Hierarchy

```python
# Exception hierarchy demonstration
def demonstrate_exception_hierarchy():
    exceptions_to_test = [
        (lambda: 1/0, "ZeroDivisionError"),
        (lambda: int("abc"), "ValueError"),
        (lambda: [1, 2, 3][10], "IndexError"),
        (lambda: {"a": 1}["b"], "KeyError"),
        (lambda: open("nonexistent.txt"), "FileNotFoundError")
    ]
    
    for exception_func, exception_name in exceptions_to_test:
        try:
            exception_func()
        except ArithmeticError as e:
            print(f"{exception_name} is a type of ArithmeticError: {e}")
        except LookupError as e:
            print(f"{exception_name} is a type of LookupError: {e}")
        except OSError as e:
            print(f"{exception_name} is a type of OSError: {e}")
        except ValueError as e:
            print(f"{exception_name} is a ValueError: {e}")
        except Exception as e:
            print(f"{exception_name} is some other Exception: {e}")

demonstrate_exception_hierarchy()
```

### 8.2 Exception Inheritance

```python
# Custom exception hierarchy
class ValidationError(Exception):
    """Base class for validation errors"""
    pass

class EmailValidationError(ValidationError):
    """Raised when email validation fails"""
    pass

class PasswordValidationError(ValidationError):
    """Raised when password validation fails"""
    pass

class WeakPasswordError(PasswordValidationError):
    """Raised when password is too weak"""
    pass

def validate_user_data(email, password):
    # Email validation
    if "@" not in email:
        raise EmailValidationError("Email must contain @ symbol")
    
    # Password validation
    if len(password) < 8:
        raise WeakPasswordError("Password must be at least 8 characters")
    
    if password.isdigit():
        raise PasswordValidationError("Password cannot be only numbers")
    
    return True

# Test exception hierarchy
def test_validation(email, password):
    try:
        validate_user_data(email, password)
        print("Validation successful!")
    except WeakPasswordError as e:
        print(f"Weak password: {e}")
    except PasswordValidationError as e:
        print(f"Password validation failed: {e}")
    except EmailValidationError as e:
        print(f"Email validation failed: {e}")
    except ValidationError as e:
        print(f"General validation error: {e}")

# Test cases
test_validation("user@example.com", "StrongPass123")  # Success
test_validation("invalid-email", "password123")       # Email error
test_validation("user@example.com", "12345678")       # Password error
test_validation("user@example.com", "weak")           # Weak password
```

---

## 9. Best Practices for Error Handling

### 9.1 Be Specific with Exception Handling

```python
# Bad: Catching all exceptions
def bad_example():
    try:
        result = risky_operation()
    except:  # Don't do this!
        print("Something went wrong")

# Good: Specific exception handling
def good_example():
    try:
        result = risky_operation()
    except ValueError as e:
        print(f"Invalid value: {e}")
    except TypeError as e:
        print(f"Type error: {e}")
    except Exception as e:
        print(f"Unexpected error: {e}")
        raise  # Re-raise if you can't handle it
```

### 9.2 Use Exception Chaining

```python
def process_data(data):
    try:
        # Process the data
        processed = complex_processing(data)
        return processed
    except ValueError as e:
        # Chain exceptions to preserve context
        raise RuntimeError("Data processing failed") from e

def complex_processing(data):
    if not isinstance(data, list):
        raise ValueError("Data must be a list")
    return [x * 2 for x in data]

# Test exception chaining
try:
    result = process_data("not a list")
except RuntimeError as e:
    print(f"Main error: {e}")
    print(f"Original cause: {e.__cause__}")
```

### 9.3 Don't Ignore Exceptions

```python
# Bad: Silently ignoring exceptions
def bad_file_reader(filename):
    try:
        with open(filename, 'r') as file:
            return file.read()
    except:
        pass  # Don't do this!

# Good: Proper exception handling
def good_file_reader(filename):
    try:
        with open(filename, 'r') as file:
            return file.read()
    except FileNotFoundError:
        print(f"File {filename} not found")
        return None
    except PermissionError:
        print(f"Permission denied for file {filename}")
        return None
    except Exception as e:
        print(f"Unexpected error reading {filename}: {e}")
        raise
```

### 9.4 Clean Up Resources

```python
# Using context managers for automatic cleanup
def safe_file_processing(filename):
    try:
        with open(filename, 'r') as file:
            # File is automatically closed even if exception occurs
            data = file.read()
            return process_data(data)
    except FileNotFoundError:
        print(f"File {filename} not found")
        return None

# Manual cleanup with try/finally
def manual_cleanup_example():
    resource = None
    try:
        resource = acquire_resource()
        return use_resource(resource)
    except Exception as e:
        print(f"Error using resource: {e}")
        raise
    finally:
        if resource:
            release_resource(resource)
```

---

## 10. Debugging Techniques

### 10.1 Print Debugging

```python
def debug_function(numbers):
    print(f"Input: {numbers}")  # Debug print
    
    try:
        total = 0
        for i, num in enumerate(numbers):
            print(f"Processing item {i}: {num}")  # Debug print
            total += num
        
        average = total / len(numbers)
        print(f"Calculated average: {average}")  # Debug print
        return average
    
    except TypeError as e:
        print(f"Debug: TypeError occurred - {e}")
        print(f"Debug: Type of numbers: {type(numbers)}")
        return None
    except ZeroDivisionError as e:
        print(f"Debug: ZeroDivisionError - {e}")
        return None

# Test with debug output
result = debug_function([1, 2, 3, 4, 5])
result = debug_function([])  # Empty list
result = debug_function("not a list")  # Wrong type
```

### 10.2 Using the traceback Module

```python
import traceback

def debug_with_traceback():
    try:
        # Simulate a complex call stack
        function_a()
    except Exception as e:
        print("Exception occurred:")
        print(f"Type: {type(e).__name__}")
        print(f"Message: {str(e)}")
        print("\nFull traceback:")
        traceback.print_exc()
        
        print("\nFormatted traceback:")
        tb_lines = traceback.format_exception(type(e), e, e.__traceback__)
        for line in tb_lines:
            print(line.strip())

def function_a():
    function_b()

def function_b():
    function_c()

def function_c():
    raise ValueError("Something went wrong in function_c")

debug_with_traceback()
```

### 10.3 Using assert for Debugging

```python
def calculate_factorial(n):
    # Assertions for debugging
    assert isinstance(n, int), f"Expected int, got {type(n)}"
    assert n >= 0, f"Expected non-negative number, got {n}"
    
    if n == 0 or n == 1:
        return 1
    
    result = 1
    for i in range(2, n + 1):
        result *= i
        # Debug assertion
        assert result > 0, f"Result became non-positive: {result}"
    
    return result

# Test with assertions
try:
    print(calculate_factorial(5))   # Should work
    print(calculate_factorial(-1))  # Should raise AssertionError
except AssertionError as e:
    print(f"Assertion failed: {e}")
```

---

## 11. Logging for Error Tracking

### 11.1 Basic Logging

```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

def logged_division(a, b):
    logger.info(f"Attempting division: {a} / {b}")
    
    try:
        result = a / b
        logger.info(f"Division successful: {result}")
        return result
    except ZeroDivisionError as e:
        logger.error(f"Division by zero error: {e}")
        return None
    except Exception as e:
        logger.exception(f"Unexpected error in division: {e}")
        return None

# Test logging
result = logged_division(10, 2)
result = logged_division(10, 0)
```

### 11.2 Advanced Logging with Exception Information

```python
import logging
import traceback

# Create a custom logger
logger = logging.getLogger('error_handler')
logger.setLevel(logging.DEBUG)

# Create handlers
file_handler = logging.FileHandler('errors.log')
console_handler = logging.StreamHandler()

# Create formatters
formatter = logging.Formatter(
    '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
file_handler.setFormatter(formatter)
console_handler.setFormatter(formatter)

# Add handlers to logger
logger.addHandler(file_handler)
logger.addHandler(console_handler)

class ErrorLogger:
    def __init__(self):
        self.logger = logger
    
    def safe_execute(self, func, *args, **kwargs):
        try:
            self.logger.info(f"Executing {func.__name__} with args: {args}, kwargs: {kwargs}")
            result = func(*args, **kwargs)
            self.logger.info(f"Successfully executed {func.__name__}")
            return result
        except Exception as e:
            self.logger.error(f"Error in {func.__name__}: {str(e)}")
            self.logger.error(f"Exception type: {type(e).__name__}")
            self.logger.error(f"Traceback: {traceback.format_exc()}")
            return None

# Test error logging
error_logger = ErrorLogger()

def problematic_function(x, y):
    return x / y

result = error_logger.safe_execute(problematic_function, 10, 2)
result = error_logger.safe_execute(problematic_function, 10, 0)
```

---

## 12. Common Error Patterns and Solutions

### 12.1 File Operations

```python
def robust_file_operations():
    """Demonstrate robust file handling patterns"""
    
    # Reading files safely
    def safe_read_file(filename):
        try:
            with open(filename, 'r', encoding='utf-8') as file:
                return file.read()
        except FileNotFoundError:
            print(f"File {filename} not found")
            return None
        except PermissionError:
            print(f"Permission denied for file {filename}")
            return None
        except UnicodeDecodeError as e:
            print(f"Encoding error reading {filename}: {e}")
            return None
    
    # Writing files safely
    def safe_write_file(filename, content):
        try:
            with open(filename, 'w', encoding='utf-8') as file:
                file.write(content)
            return True
        except PermissionError:
            print(f"Permission denied writing to {filename}")
            return False
        except OSError as e:
            print(f"OS error writing to {filename}: {e}")
            return False
    
    # Test file operations
    content = safe_read_file("test.txt")
    if content:
        success = safe_write_file("backup.txt", content)
        print(f"Backup created: {success}")

robust_file_operations()
```

### 12.2 Network Operations

```python
import urllib.request
import urllib.error
import json

def robust_web_request(url):
    """Demonstrate robust web request handling"""
    try:
        with urllib.request.urlopen(url, timeout=10) as response:
            data = response.read().decode('utf-8')
            return json.loads(data)
    except urllib.error.HTTPError as e:
        print(f"HTTP Error {e.code}: {e.reason}")
        return None
    except urllib.error.URLError as e:
        print(f"URL Error: {e.reason}")
        return None
    except json.JSONDecodeError as e:
        print(f"JSON Decode Error: {e}")
        return None
    except Exception as e:
        print(f"Unexpected error: {e}")
        return None

# Test web request
data = robust_web_request("https://api.github.com/users/octocat")
if data:
    print(f"User: {data.get('name', 'Unknown')}")
```

### 12.3 User Input Validation

```python
def get_valid_integer(prompt, min_value=None, max_value=None):
    """Get valid integer input from user with error handling"""
    while True:
        try:
            value = int(input(prompt))
            
            if min_value is not None and value < min_value:
                print(f"Value must be at least {min_value}")
                continue
            
            if max_value is not None and value > max_value:
                print(f"Value must be at most {max_value}")
                continue
            
            return value
        except ValueError:
            print("Please enter a valid integer")
        except KeyboardInterrupt:
            print("\nOperation cancelled by user")
            return None
        except EOFError:
            print("\nEnd of input reached")
            return None

# Test input validation
# age = get_valid_integer("Enter your age (0-150): ", 0, 150)
# if age is not None:
#     print(f"Your age is: {age}")
```

---

## 13. Practical Examples

### 13.1 Simple Calculator with Error Handling

```python
class Calculator:
    def __init__(self):
        self.operations = {
            '+': self.add,
            '-': self.subtract,
            '*': self.multiply,
            '/': self.divide,
            '**': self.power,
            '%': self.modulo
        }
    
    def add(self, a, b):
        return a + b
    
    def subtract(self, a, b):
        return a - b
    
    def multiply(self, a, b):
        return a * b
    
    def divide(self, a, b):
        if b == 0:
            raise ZeroDivisionError("Cannot divide by zero")
        return a / b
    
    def power(self, a, b):
        try:
            return a ** b
        except OverflowError:
            raise OverflowError("Result too large to calculate")
    
    def modulo(self, a, b):
        if b == 0:
            raise ZeroDivisionError("Cannot compute modulo with zero")
        return a % b
    
    def calculate(self, a, operator, b):
        try:
            if operator not in self.operations:
                raise ValueError(f"Unsupported operator: {operator}")
            
            return self.operations[operator](a, b)
        except (TypeError, ValueError) as e:
            raise ValueError(f"Invalid operands or operator: {e}")

# Test calculator
calc = Calculator()

test_cases = [
    (10, '+', 5),
    (10, '/', 0),
    (2, '**', 1000),
    (10, '%', 3),
    (10, '^', 3),  # Invalid operator
]

for a, op, b in test_cases:
    try:
        result = calc.calculate(a, op, b)
        print(f"{a} {op} {b} = {result}")
    except Exception as e:
        print(f"Error calculating {a} {op} {b}: {e}")
```

### 13.2 Configuration File Reader

```python
import json
import os

class ConfigReader:
    def __init__(self, config_file):
        self.config_file = config_file
        self.config = {}
        self.load_config()
    
    def load_config(self):
        """Load configuration from file with comprehensive error handling"""
        try:
            if not os.path.exists(self.config_file):
                raise FileNotFoundError(f"Configuration file {self.config_file} not found")
            
            with open(self.config_file, 'r') as file:
                self.config = json.load(file)
            
            self.validate_config()
            print(f"Configuration loaded successfully from {self.config_file}")
            
        except FileNotFoundError as e:
            print(f"File Error: {e}")
            self.load_default_config()
        except json.JSONDecodeError as e:
            print(f"JSON Error: Invalid JSON in {self.config_file} - {e}")
            self.load_default_config()
        except PermissionError:
            print(f"Permission Error: Cannot read {self.config_file}")
            self.load_default_config()
        except Exception as e:
            print(f"Unexpected error loading config: {e}")
            self.load_default_config()
    
    def validate_config(self):
        """Validate configuration structure"""
        required_keys = ['database_url', 'api_key', 'debug']
        
        for key in required_keys:
            if key not in self.config:
                raise ValueError(f"Missing required configuration key: {key}")
        
        if not isinstance(self.config['debug'], bool):
            raise ValueError("Debug setting must be boolean")
    
    def load_default_config(self):
        """Load default configuration when file loading fails"""
        print("Loading default configuration...")
        self.config = {
            'database_url': 'sqlite:///default.db',
            'api_key': '',
            'debug': True
        }
    
    def get(self, key, default=None):
        """Get configuration value with error handling"""
        try:
            return self.config[key]
        except KeyError:
            if default is not None:
                return default
            raise KeyError(f"Configuration key '{key}' not found")
    
    def save_config(self):
        """Save current configuration to file"""
        try:
            with open(self.config_file, 'w') as file:
                json.dump(self.config, file, indent=2)
            print(f"Configuration saved to {self.config_file}")
        except PermissionError:
            print(f"Permission Error: Cannot write to {self.config_file}")
        except Exception as e:
            print(f"Error saving configuration: {e}")

# Test configuration reader
config_reader = ConfigReader('app_config.json')
try:
    db_url = config_reader.get('database_url')
    debug_mode = config_reader.get('debug')
    print(f"Database URL: {db_url}")
    print(f"Debug mode: {debug_mode}")
except KeyError as e:
    print(f"Configuration error: {e}")
```

---

## 14. Review Questions

1. **What is the difference between syntax errors and runtime errors? Provide examples of each.**

2. **Explain the purpose of the `try`, `except`, `else`, and `finally` clauses. When does each execute?**

3. **What is the difference between catching `Exception` and catching specific exception types? Which approach is better and why?**

4. **How do you create and raise custom exceptions? What are the benefits of creating custom exception classes?**

5. **What is exception chaining and how do you implement it using the `from` keyword?**

6. **Explain the concept of exception hierarchy. How does it help in writing better exception handling code?**

7. **What are some best practices for error handling in Python? Provide examples of good and bad practices.**

8. **How can you use the `traceback` module to get more information about exceptions?**

9. **What is the difference between `logging.error()` and `logging.exception()`? When would you use each?**

10. **Write a function that safely converts a string to an integer with proper error handling for various edge cases.**

11. **How do you handle multiple exceptions that might occur in the same try block? Show different approaches.**

12. **What is the purpose of the `assert` statement in debugging? How does it differ from regular exception handling?**

13. **Explain how to use context managers (`with` statements) for resource management and error handling.**

14. **What happens when an exception is not caught? How does Python handle unhandled exceptions?**

15. **How can you re-raise an exception after catching it? When might this be useful?**

---

## 15. Answers to Review Questions

### 1. Syntax Errors vs Runtime Errors

**Syntax Errors:**
- Occur when Python cannot parse the code due to incorrect syntax
- Detected before program execution begins
- Examples:
```python
# Missing colon
if x == 5
    print("x is 5")

# Unmatched parentheses
print("Hello World"

# Invalid indentation
def my_function():
print("This is not properly indented")
```

**Runtime Errors (Exceptions):**
- Occur during program execution when syntactically correct code encounters an error
- Examples:
```python
# NameError: variable not defined
print(undefined_variable)

# ZeroDivisionError: division by zero
result = 10 / 0

# TypeError: unsupported operand types
result = "5" + 3
```

### 2. Purpose of try/except/else/finally Clauses

```python
try:
    # Code that might raise an exception
    # Executes first
    risky_operation()
except SpecificException:
    # Handles specific exceptions
    # Executes only if matching exception occurs in try block
    handle_specific_error()
else:
    # Executes only if NO exception occurs in try block
    # Good for code that should only run on success
    success_operation()
finally:
    # Always executes, regardless of exceptions
    # Used for cleanup (closing files, connections, etc.)
    cleanup_resources()
```

### 3. Exception vs Specific Exception Types

**Catching Exception (broader):**
```python
try:
    risky_operation()
except Exception as e:
    print(f"Some error occurred: {e}")
```

**Catching specific exceptions (better):**
```python
try:
    risky_operation()
except ValueError as e:
    print(f"Invalid value: {e}")
except TypeError as e:
    print(f"Type error: {e}")
```

**Specific exception handling is better because:**
- More precise error handling
- Prevents masking unexpected errors
- Allows different handling for different error types
- Makes debugging easier

### 4. Custom Exceptions

```python
# Creating custom exceptions
class ValidationError(Exception):
    """Base exception for validation errors"""
    pass

class EmailValidationError(ValidationError):
    """Raised when email validation fails"""
    def __init__(self, email, message="Invalid email format"):
        self.email = email
        super().__init__(f"{message}: {email}")

# Raising custom exceptions
def validate_email(email):
    if "@" not in email:
        raise EmailValidationError(email, "Missing @ symbol")
    return True

# Benefits:
# - More descriptive error messages
# - Better error categorization
# - Allows specific handling of different error types
# - Improves code readability and maintainability
```

### 5. Exception Chaining

```python
def process_data(data):
    try:
        result = complex_operation(data)
        return result
    except ValueError as e:
        # Chain exceptions to preserve original error context
        raise ProcessingError("Data processing failed") from e

# Benefits:
# - Preserves original error information
# - Provides context about where error occurred
# - Helps with debugging complex call stacks
```

### 6. Exception Hierarchy

Python's exception hierarchy allows you to catch groups of related exceptions:

```python
try:
    risky_operation()
except LookupError:
    # Catches IndexError, KeyError, etc.
    handle_lookup_error()
except ArithmeticError:
    # Catches ZeroDivisionError, OverflowError, etc.
    handle_arithmetic_error()
except Exception:
    # Catches any other exception
    handle_other_errors()
```

### 7. Best Practices

**Good practices:**
```python
# Be specific with exceptions
try:
    operation()
except ValueError as e:
    handle_value_error(e)
except TypeError as e:
    handle_type_error(e)

# Use finally for cleanup
try:
    resource = acquire_resource()
    use_resource(resource)
finally:
    release_resource(resource)

# Don't ignore exceptions
try:
    risky_operation()
except ImportantError:
    log_error()
    raise  # Re-raise if you can't handle it
```

**Bad practices:**
```python
# Don't catch all exceptions blindly
try:
    operation()
except:  # Don't do this!
    pass

# Don't ignore exceptions
try:
    operation()
except Exception:
    pass  # Don't do this!
```

### 8. Using traceback Module

```python
import traceback

try:
    problematic_function()
except Exception as e:
    # Print full traceback
    traceback.print_exc()
    
    # Get traceback as string
    tb_str = traceback.format_exc()
    
    # Get specific traceback information
    tb_lines = traceback.format_exception(type(e), e, e.__traceback__)
```

### 9. logging.error() vs logging.exception()

```python
import logging

try:
    1/0
except ZeroDivisionError as e:
    # logging.error() - logs error message only
    logging.error(f"Division error: {e}")
    
    # logging.exception() - logs error message with full traceback
    logging.exception("Division error occurred")
```

**Use logging.error()** when you want to log just the error message.
**Use logging.exception()** when you want the full traceback information.

### 10. Safe String to Integer Conversion

```python
def safe_string_to_int(value, default=None):
    """
    Safely convert string to integer with comprehensive error handling
    """
    try:
        # Handle None input
        if value is None:
            if default is not None:
                return default
            raise ValueError("Cannot convert None to integer")
        
        # Handle non-string types
        if not isinstance(value, str):
            raise TypeError(f"Expected string, got {type(value).__name__}")
        
        # Handle empty string
        if not value.strip():
            if default is not None:
                return default
            raise ValueError("Cannot convert empty string to integer")
        
        # Convert to integer
        return int(value.strip())
    
    except ValueError as e:
        if "invalid literal" in str(e):
            if default is not None:
                return default
            raise ValueError(f"'{value}' is not a valid integer")
        raise
    except Exception as e:
        raise RuntimeError(f"Unexpected error converting '{value}' to integer: {e}")
```

### 11. Handling Multiple Exceptions

**Method 1: Multiple except blocks**
```python
try:
    operation()
except ValueError:
    handle_value_error()
except TypeError:
    handle_type_error()
except KeyError:
    handle_key_error()
```

**Method 2: Tuple of exceptions**
```python
try:
    operation()
except (ValueError, TypeError, KeyError) as e:
    handle_multiple_errors(e)
```

**Method 3: Exception hierarchy**
```python
try:
    operation()
except LookupError:  # Catches KeyError, IndexError
    handle_lookup_error()
except Exception:    # Catches everything else
    handle_other_errors()
```

### 12. Assert Statement

```python
def calculate_average(numbers):
    # Assert for debugging - can be disabled with -O flag
    assert isinstance(numbers, list), "Input must be a list"
    assert len(numbers) > 0, "List cannot be empty"
    
    total = sum(numbers)
    average = total / len(numbers)
    
    # Post-condition assertion
    assert average >= min(numbers), "Average cannot be less than minimum"
    assert average <= max(numbers), "Average cannot be greater than maximum"
    
    return average

# Differs from exception handling:
# - Assertions are for debugging, not error handling
# - Can be disabled in production
# - Should not be used for user input validation
```

### 13. Context Managers for Resource Management

```python
# Using with statement for automatic resource cleanup
with open('file.txt', 'r') as file:
    content = file.read()
    # File is automatically closed even if exception occurs

# Custom context manager
class DatabaseConnection:
    def __enter__(self):
        self.connection = create_connection()
        return self.connection
    
    def __exit__(self, exc_type, exc_value, traceback):
        if self.connection:
            self.connection.close()
        # Return False to propagate exceptions
        return False

# Usage
with DatabaseConnection() as conn:
    conn.execute("SELECT * FROM users")
    # Connection automatically closed
```

### 14. Unhandled Exceptions

When an exception is not caught:
1. Python prints the traceback to stderr
2. The program terminates with a non-zero exit code
3. The exception propagates up the call stack
4. If it reaches the top level, the program crashes

```python
# Unhandled exception example
def function_a():
    function_b()

def function_b():
    raise ValueError("Something went wrong")

function_a()  # Program will crash with traceback
```

### 15. Re-raising Exceptions

```python
def process_data(data):
    try:
        result = complex_processing(data)
        return result
    except ValueError as e:
        # Log the error
        logging.error(f"Processing failed: {e}")
        # Re-raise to let caller handle it
        raise
    except Exception as e:
        # Convert to more specific exception
        raise ProcessingError("Data processing failed") from e

# Useful when you want to:
# - Log errors but let caller handle them
# - Add context to exceptions
# - Perform cleanup before propagating
```

---

This comprehensive guide covers error handling and exception management in Python at a master's degree level. The content includes theoretical concepts, practical examples, best practices, and detailed explanations that will help you write robust, maintainable Python code.
