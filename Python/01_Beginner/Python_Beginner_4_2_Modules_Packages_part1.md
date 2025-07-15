# Python Beginner 4.2: Modules and Packages Part 1

## Table of Contents
- [1. Introduction to Modules](Python_Beginner_4_2_Modules_Packages_part1.md#1-introduction-to-modules)
- [2. Understanding Python's Module System](Python_Beginner_4_2_Modules_Packages_part1.md#2-understanding-pythons-module-system)
- [3. Importing Modules](Python_Beginner_4_2_Modules_Packages_part1.md#3-importing-modules)
- [4. Creating Custom Modules](Python_Beginner_4_2_Modules_Packages_part1.md#4-creating-custom-modules)
- [5. Module Search Path and PYTHONPATH](Python_Beginner_4_2_Modules_Packages_part1.md#5-module-search-path-and-pythonpath)
- [6. Package Structure and Organization](Python_Beginner_4_2_Modules_Packages_part1.md#6-package-structure-and-organization)
- [7. The __init__.py File](Python_Beginner_4_2_Modules_Packages_part2.md#7-the-__init__py-file)
- [8. Relative and Absolute Imports](Python_Beginner_4_2_Modules_Packages_part2.md#8-relative-and-absolute-imports)
- [9. Built-in Modules](Python_Beginner_4_2_Modules_Packages_part2.md#9-built-in-modules)
- [10. Third-Party Package Management with pip](Python_Beginner_4_2_Modules_Packages_part2.md#10-third-party-package-management-with-pip)
- [11. Virtual Environments](Python_Beginner_4_2_Modules_Packages_part2.md#11-virtual-environments)
- [12. Module Documentation and Introspection](Python_Beginner_4_2_Modules_Packages_part2.md#12-module-documentation-and-introspection)
- [13. Best Practices for Module Design](Python_Beginner_4_2_Modules_Packages_part2.md#13-best-practices-for-module-design)
- [14. Common Pitfalls and Solutions](Python_Beginner_4_2_Modules_Packages_part2.md#14-common-pitfalls-and-solutions)
- [15. Advanced Module Concepts](Python_Beginner_4_2_Modules_Packages_part2.md#15-advanced-module-concepts)
- [16. Practical Examples](Python_Beginner_4_2_Modules_Packages_part2.md#16-practical-examples)
- [17. Review Questions](Python_Beginner_4_2_Modules_Packages_part2.md#17-review-questions)
- [18. Answers to Review Questions](Python_Beginner_4_2_Modules_Packages_part2.md#18-answers-to-review-questions)

---

## 1. Introduction to Modules

A module in Python is a file containing Python definitions, statements, and executable code. It serves as a fundamental building block for organizing and structuring Python applications. Modules enable developers to break down complex programs into smaller, manageable components that can be reused across multiple projects.

### 1.1 What is a Module?

A module is essentially a Python file (with a `.py` extension) that contains:
- Function definitions
- Class definitions
- Variable declarations
- Executable statements

```python
# Example: math_operations.py (a comprehensive module)
"""
Mathematical operations module.
This module provides basic arithmetic operations and mathematical constants.
"""

import math

# Constants
PI = 3.14159265359
E = 2.71828182846

# Basic arithmetic operations
def add(a, b):
    """Add two numbers and return the result."""
    return a + b

def subtract(a, b):
    """Subtract b from a and return the result."""
    return a - b

def multiply(a, b):
    """Multiply two numbers and return the result."""
    return a * b

def divide(a, b):
    """Divide a by b and return the result. Raises ZeroDivisionError if b is zero."""
    if b == 0:
        raise ZeroDivisionError("Division by zero is not allowed")
    return a / b

# Advanced operations
def power(base, exponent):
    """Calculate base raised to the power of exponent."""
    return base ** exponent

def square_root(number):
    """Calculate the square root of a number."""
    if number < 0:
        raise ValueError("Cannot calculate square root of negative number")
    return math.sqrt(number)

# Utility functions
def is_even(number):
    """Check if a number is even."""
    return number % 2 == 0

def factorial(n):
    """Calculate the factorial of a non-negative integer."""
    if n < 0:
        raise ValueError("Factorial is not defined for negative numbers")
    if n == 0 or n == 1:
        return 1
    return n * factorial(n - 1)

# Module-level executable code
print(f"Math operations module loaded. PI = {PI}")
```

### 1.2 Benefits of Using Modules

Modules provide numerous advantages in software development:

#### 1.2.1 Code Organization
- **Logical Grouping**: Related functions and classes are kept together
- **Separation of Concerns**: Different aspects of functionality are isolated
- **Cleaner Structure**: Large applications become more manageable

#### 1.2.2 Reusability
- **DRY Principle**: Don't Repeat Yourself - write once, use everywhere
- **Library Development**: Create reusable components for multiple projects
- **Code Sharing**: Share modules between team members and projects

#### 1.2.3 Namespace Management
- **Avoid Naming Conflicts**: Functions with the same name can coexist in different modules
- **Explicit Imports**: Clear indication of where functionality comes from
- **Controlled Scope**: Variables and functions are contained within their module

#### 1.2.4 Maintainability
- **Easier Debugging**: Isolate issues to specific modules
- **Simplified Testing**: Test modules independently
- **Version Control**: Track changes to specific functionality

#### 1.2.5 Performance Benefits
- **Lazy Loading**: Modules are loaded only when needed
- **Compiled Bytecode**: Python compiles modules to `.pyc` files for faster execution
- **Memory Efficiency**: Modules are loaded once and shared across imports

### 1.3 Types of Modules

Python supports several types of modules:

#### 1.3.1 Built-in Modules
Modules that are part of the Python standard library:
```python
import sys      # System-specific parameters and functions
import os       # Operating system interface
import datetime # Date and time handling
import json     # JSON encoder and decoder
```

#### 1.3.2 User-defined Modules
Custom modules created by developers:
```python
import my_calculator    # Your custom module
import data_processor   # Another custom module
```

#### 1.3.3 Third-party Modules
Modules developed by the Python community:
```python
import requests  # HTTP library
import numpy     # Numerical computing
import pandas    # Data analysis
```

### 1.4 Module Compilation and Caching

When a module is imported for the first time, Python:
1. Compiles the source code to bytecode
2. Stores the bytecode in `__pycache__` directory
3. Creates a `.pyc` file for faster subsequent imports

```python
# Example of module caching behavior
import sys
import importlib

# First import - compiles and caches
import math_operations

# Subsequent imports use cached bytecode
import math_operations  # Faster execution

# Force reload if module changes
importlib.reload(math_operations)
```

---

## 2. Understanding Python's Module System

Python's module system is built on a sophisticated framework that manages how code is organized, imported, and executed. Understanding this system is crucial for writing maintainable and scalable Python applications.

### 2.1 The Module Object

When Python imports a module, it creates a module object that contains:
- The module's namespace (all defined names)
- Metadata about the module
- References to the module's code

```python
# Exploring module objects
import math
import sys

# Module attributes
print(f"Module name: {math.__name__}")
print(f"Module file: {math.__file__}")
print(f"Module doc: {math.__doc__[:50]}...")

# Module namespace
print(f"Module attributes: {dir(math)[:10]}")  # First 10 attributes

# Module in sys.modules
print(f"Math module in sys.modules: {'math' in sys.modules}")
```

### 2.2 Module Loading Process

The module loading process involves several steps:

#### 2.2.1 Finding the Module
Python searches for modules in the following order:
1. Built-in modules
2. Current directory
3. PYTHONPATH directories
4. Standard library directories
5. Site-packages directories

#### 2.2.2 Loading and Compilation
```python
# Understanding module loading
import sys
import importlib.util

def load_module_info(module_name):
    """Display information about module loading."""
    try:
        # Check if module is built-in
        if module_name in sys.builtin_module_names:
            print(f"{module_name} is a built-in module")
            return
        
        # Find module specification
        spec = importlib.util.find_spec(module_name)
        if spec is None:
            print(f"Module {module_name} not found")
            return
        
        print(f"Module name: {spec.name}")
        print(f"Module origin: {spec.origin}")
        print(f"Module is package: {spec.submodule_search_locations is not None}")
        
    except Exception as e:
        print(f"Error loading module info: {e}")

# Example usage
load_module_info("math")
load_module_info("os")
load_module_info("numpy")  # May not be available
```

#### 2.2.3 Module Execution
When a module is imported:
1. Python creates a new module object
2. Executes the module's code in its namespace
3. Adds the module to `sys.modules`
4. Returns the module object

### 2.3 The sys.modules Cache

Python maintains a cache of imported modules in `sys.modules`:

```python
import sys

# Examine sys.modules
print(f"Number of loaded modules: {len(sys.modules)}")
print(f"First 10 modules: {list(sys.modules.keys())[:10]}")

# Check if a module is cached
import json
print(f"JSON module cached: {'json' in sys.modules}")

# Remove a module from cache (rarely needed)
if 'json' in sys.modules:
    del sys.modules['json']
    print("JSON module removed from cache")
```

### 2.4 Module Attributes and Introspection

Every module has special attributes that provide metadata:

```python
# Create a sample module for demonstration
sample_module_code = '''
"""
Sample module for demonstration.
This module shows various module attributes.
"""

__version__ = "1.0.0"
__author__ = "Python Student"

def greet(name):
    """Greet a person by name."""
    return f"Hello, {name}!"

class Calculator:
    """A simple calculator class."""
    def add(self, a, b):
        return a + b
'''

# Write and import the module
with open('sample_module.py', 'w') as f:
    f.write(sample_module_code)

import sample_module

# Explore module attributes
print(f"Module name: {sample_module.__name__}")
print(f"Module file: {sample_module.__file__}")
print(f"Module docstring: {sample_module.__doc__}")
print(f"Module version: {getattr(sample_module, '__version__', 'Not specified')}")
print(f"Module author: {getattr(sample_module, '__author__', 'Not specified')}")
print(f"Module directory: {dir(sample_module)}")
```

### 2.5 Module Search Path

Python uses the module search path to locate modules:

```python
import sys
import os

# Display current search path
print("Python module search path:")
for i, path in enumerate(sys.path):
    print(f"{i+1}. {path}")

# Add a custom directory to search path
custom_path = "/path/to/custom/modules"
if custom_path not in sys.path:
    sys.path.append(custom_path)

# Create a function to search for modules
def find_module_path(module_name):
    """Find the path where a module would be loaded from."""
    import importlib.util
    
    spec = importlib.util.find_spec(module_name)
    if spec and spec.origin:
        return spec.origin
    return None

# Example usage
print(f"Math module path: {find_module_path('math')}")
print(f"OS module path: {find_module_path('os')}")
```

### 2.6 Import Hooks and Customization

Python allows customization of the import process through import hooks:

```python
import sys
import importlib.util

class CustomImportHook:
    """A simple import hook for demonstration."""
    
    def find_spec(self, fullname, path, target=None):
        """Find module specification."""
        print(f"Looking for module: {fullname}")
        return None  # Let default import mechanism handle it
    
    def find_module(self, fullname, path=None):
        """Legacy method for finding modules."""
        print(f"Legacy find_module called for: {fullname}")
        return None

# Install custom import hook
hook = CustomImportHook()
sys.meta_path.insert(0, hook)

# Now imports will trigger our hook
try:
    import example_module  # This will trigger our hook
except ImportError:
    print("Module not found (expected)")

# Remove the hook
sys.meta_path.remove(hook)
```

---

## 3. Importing Modules

Importing is the mechanism by which Python makes code from one module available in another. Python provides several import statements and techniques to control how modules are imported and used.

### 3.1 Basic Import Statement

The basic `import` statement loads a module and makes it available in the current namespace:

```python
import math

# Access module contents using dot notation
result = math.sqrt(16)  # 4.0
print(math.pi)         # 3.141592653589793
print(math.e)          # 2.718281828459045

# Module is an object with attributes
print(type(math))      # <class 'module'>
print(dir(math)[:10])  # First 10 attributes
```

### 3.2 From Import Statement

The `from` statement allows importing specific names from a module:

```python
from math import sqrt, pi, e, sin, cos

# Use imported names directly (without module prefix)
result = sqrt(16)      # 4.0
angle = pi / 4         # π/4 radians
sine_value = sin(angle)
cosine_value = cos(angle)

print(f"sin(π/4) = {sine_value:.4f}")
print(f"cos(π/4) = {cosine_value:.4f}")
```

### 3.3 Import with Aliases

Aliases make module names shorter or more descriptive:

```python
import math as m
import numpy as np  # Common convention
import pandas as pd # Common convention

# Use aliases
result = m.sqrt(16)
# array = np.array([1, 2, 3])  # If numpy is available
# df = pd.DataFrame(data)      # If pandas is available

# Alias specific imports
from math import sqrt as square_root
from collections import defaultdict as dd

number = 25
print(f"Square root of {number} is {square_root(number)}")
```

### 3.4 Wildcard Imports

Wildcard imports bring all public names from a module into the current namespace:

```python
from math import *

# All math functions are now available directly
result = sqrt(16)
angle = pi / 2
sine_value = sin(angle)

# WARNING: This can lead to namespace pollution
# Better to be explicit about what you import
```

**Best Practice**: Avoid wildcard imports in production code as they can:
- Pollute the namespace
- Make code harder to understand
- Lead to naming conflicts
- Make debugging more difficult

### 3.5 Conditional Imports

Sometimes you need to import modules conditionally:

```python
import sys

# Import based on Python version
if sys.version_info >= (3, 8):
    from functools import cached_property
else:
    # Fallback for older Python versions
    def cached_property(func):
        return property(func)

# Import based on availability
try:
    import numpy as np
    HAS_NUMPY = True
except ImportError:
    HAS_NUMPY = False
    print("NumPy not available")

# Platform-specific imports
if sys.platform == "win32":
    import winsound
elif sys.platform.startswith("linux"):
    import subprocess
    
# Use conditional imports
def play_sound(frequency, duration):
    """Play a sound with platform-specific implementation."""
    if sys.platform == "win32" and 'winsound' in locals():
        winsound.Beep(frequency, duration)
    elif sys.platform.startswith("linux"):
        # Linux implementation using subprocess
        subprocess.run(['beep', '-f', str(frequency), '-l', str(duration)])
    else:
        print(f"Playing sound: {frequency}Hz for {duration}ms")
```

### 3.6 Dynamic Imports

Python allows importing modules dynamically at runtime:

```python
import importlib
import sys

def dynamic_import(module_name):
    """Dynamically import a module."""
    try:
        module = importlib.import_module(module_name)
        return module
    except ImportError as e:
        print(f"Failed to import {module_name}: {e}")
        return None

# Example usage
module_name = "json"
json_module = dynamic_import(module_name)
if json_module:
    data = {"name": "Python", "version": "3.9"}
    json_string = json_module.dumps(data)
    print(f"JSON string: {json_string}")

# Import from string with error handling
def safe_import(module_name, attribute_name=None):
    """Safely import a module or attribute."""
    try:
        module = importlib.import_module(module_name)
        if attribute_name:
            return getattr(module, attribute_name)
        return module
    except (ImportError, AttributeError) as e:
        print(f"Import error: {e}")
        return None

# Examples
datetime_module = safe_import("datetime")
datetime_class = safe_import("datetime", "datetime")
```

### 3.7 Lazy Imports

Lazy imports defer module loading until the module is actually used:

```python
import sys

class LazyImport:
    """A lazy import wrapper."""
    
    def __init__(self, module_name):
        self.module_name = module_name
        self._module = None
    
    def __getattr__(self, name):
        if self._module is None:
            self._module = __import__(self.module_name)
        return getattr(self._module, name)

# Usage
json = LazyImport('json')
# json module is not loaded yet

# Module is loaded only when first accessed
data = json.dumps({"key": "value"})  # Now json is loaded
```

### 3.8 Import Performance Considerations

Understanding import performance helps optimize application startup time:

```python
import time
import sys

def measure_import_time(module_name):
    """Measure the time taken to import a module."""
    start_time = time.time()
    try:
        __import__(module_name)
        end_time = time.time()
        return end_time - start_time
    except ImportError:
        return None

# Measure import times
modules_to_test = ['json', 'os', 'sys', 'math', 'collections']
for module in modules_to_test:
    if module not in sys.modules:
        import_time = measure_import_time(module)
        if import_time is not None:
            print(f"{module}: {import_time:.4f} seconds")
```

### 3.9 Import Best Practices

#### 3.9.1 Import Order
Follow PEP 8 guidelines for import ordering:

```python
# 1. Standard library imports
import os
import sys
import json
from collections import defaultdict

# 2. Related third-party imports
import requests
import numpy as np
import pandas as pd

# 3. Local application/library imports
from my_package import my_module
from . import sibling_module
```

#### 3.9.2 Explicit Imports
Be explicit about what you import:

```python
# Good: Explicit and clear
from collections import defaultdict, Counter
from datetime import datetime, timedelta

# Avoid: Unclear what's being imported
from collections import *
```

#### 3.9.3 Import at Module Level
Place imports at the top of the module:

```python
# Good: Imports at module level
import json
import os

def process_data(data):
    # Function implementation
    pass

# Avoid: Imports inside functions (unless necessary)
def process_data_bad(data):
    import json  # Avoid unless conditionally needed
    return json.dumps(data)
```

### 3.10 Common Import Patterns

#### 3.10.1 Optional Dependencies
```python
# Handle optional dependencies gracefully
try:
    import matplotlib.pyplot as plt
    MATPLOTLIB_AVAILABLE = True
except ImportError:
    MATPLOTLIB_AVAILABLE = False

def plot_data(data):
    if not MATPLOTLIB_AVAILABLE:
        raise ImportError("matplotlib is required for plotting")
    # Plotting code here
```

#### 3.10.2 Version-Specific Imports
```python
import sys

# Python 3.8+ has typing.Protocol
if sys.version_info >= (3, 8):
    from typing import Protocol
else:
    try:
        from typing_extensions import Protocol
    except ImportError:
        # Fallback for older Python without typing_extensions
        class Protocol:
            pass
```

#### 3.10.3 Namespace Packages
```python
# For namespace packages (PEP 420)
try:
    import namespace_package.module1
    import namespace_package.module2
except ImportError:
    print("Namespace package not available")
```

---

## 4. Creating Custom Modules

Creating custom modules is fundamental to organizing Python code effectively. A well-designed module should be focused, reusable, and maintainable.

### 4.1 Basic Module Creation

Creating a module is as simple as writing Python code in a `.py` file:

```python
# calculator.py - A comprehensive calculator module
"""
Calculator Module

This module provides mathematical operations and utilities.
"""

import math
from typing import Union, List, Tuple

# Module-level constants
PI = math.pi
E = math.e
GOLDEN_RATIO = (1 + math.sqrt(5)) / 2

# Basic arithmetic operations
def add(x: Union[int, float], y: Union[int, float]) -> Union[int, float]:
    """Add two numbers."""
    return x + y

def subtract(x: Union[int, float], y: Union[int, float]) -> Union[int, float]:
    """Subtract y from x."""
    return x - y

def multiply(x: Union[int, float], y: Union[int, float]) -> Union[int, float]:
    """Multiply two numbers."""
    return x * y

def divide(x: Union[int, float], y: Union[int, float]) -> float:
    """Divide x by y. Raises ZeroDivisionError if y is zero."""
    if y == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return x / y

# Advanced operations
def power(base: Union[int, float], exponent: Union[int, float]) -> Union[int, float]:
    """Calculate base raised to the power of exponent."""
    return base ** exponent

def square_root(x: Union[int, float]) -> float:
    """Calculate the square root of x."""
    if x < 0:
        raise ValueError("Cannot calculate square root of negative number")
    return math.sqrt(x)

def factorial(n: int) -> int:
    """Calculate factorial of n."""
    if n < 0:
        raise ValueError("Factorial is not defined for negative numbers")
    if n == 0 or n == 1:
        return 1
    return n * factorial(n - 1)

# Statistical functions
def mean(numbers: List[Union[int, float]]) -> float:
    """Calculate the arithmetic mean of a list of numbers."""
    if not numbers:
        raise ValueError("Cannot calculate mean of empty list")
    return sum(numbers) / len(numbers)

def median(numbers: List[Union[int, float]]) -> float:
    """Calculate the median of a list of numbers."""
    if not numbers:
        raise ValueError("Cannot calculate median of empty list")
    sorted_numbers = sorted(numbers)
    n = len(sorted_numbers)
    if n % 2 == 0:
        return (sorted_numbers[n//2 - 1] + sorted_numbers[n//2]) / 2
    else:
        return sorted_numbers[n//2]

# Utility functions
def is_prime(n: int) -> bool:
    """Check if a number is prime."""
    if n < 2:
        return False
    for i in range(2, int(math.sqrt(n)) + 1):
        if n % i == 0:
            return False
    return True

def fibonacci(n: int) -> int:
    """Calculate the nth Fibonacci number."""
    if n < 0:
        raise ValueError("Fibonacci is not defined for negative numbers")
    if n == 0:
        return 0
    if n == 1:
        return 1
    return fibonacci(n-1) + fibonacci(n-2)

# Classes for complex calculations
class ComplexCalculator:
    """A calculator for complex mathematical operations."""
    
    def __init__(self):
        self.history = []
    
    def calculate(self, expression: str) -> float:
        """Evaluate a mathematical expression safely."""
        # Note: In production, use a proper expression parser
        # This is a simplified example
        try:
            # Only allow safe operations
            allowed_names = {
                "abs": abs, "round": round, "min": min, "max": max,
                "sum": sum, "pow": pow, "sqrt": math.sqrt,
                "sin": math.sin, "cos": math.cos, "tan": math.tan,
                "log": math.log, "pi": math.pi, "e": math.e
            }
            result = eval(expression, {"__builtins__": {}}, allowed_names)
            self.history.append(f"{expression} = {result}")
            return result
        except Exception as e:
            raise ValueError(f"Invalid expression: {e}")
    
    def get_history(self) -> List[str]:
        """Get calculation history."""
        return self.history.copy()
    
    def clear_history(self):
        """Clear calculation history."""
        self.history.clear()

# Module initialization code
if __name__ == "__main__":
    # This code runs only when the module is executed directly
    print("Calculator module loaded successfully!")
    print(f"Available functions: {[name for name in dir() if not name.startswith('_')]}")
```

### 4.2 Using Custom Modules

Once created, use your custom module like any other module:

```python
# main.py - Using the calculator module
import calculator

# Use basic functions
result1 = calculator.add(10, 5)
result2 = calculator.multiply(3, 4)
result3 = calculator.square_root(16)

print(f"10 + 5 = {result1}")
print(f"3 * 4 = {result2}")
print(f"√16 = {result3}")

# Use constants
print(f"π = {calculator.PI}")
print(f"Golden ratio = {calculator.GOLDEN_RATIO}")

# Use statistical functions
numbers = [1, 2, 3, 4, 5]
avg = calculator.mean(numbers)
med = calculator.median(numbers)
print(f"Mean of {numbers}: {avg}")
print(f"Median of {numbers}: {med}")

# Use the calculator class
calc = calculator.ComplexCalculator()
result = calc.calculate("sqrt(16) + pow(2, 3)")
print(f"Complex calculation result: {result}")
print(f"History: {calc.get_history()}")
```

### 4.3 Module Documentation

Good documentation is crucial for module usability:

```python
# documented_module.py
"""
Documented Module Example

This module demonstrates proper documentation practices for Python modules.
It includes functions, classes, and proper docstring formats.

Example:
    >>> import documented_module
    >>> result = documented_module.greet("Alice")
    >>> print(result)
    Hello, Alice!

Author: Python Student
Date: 2024-01-01
Version: 1.0.0
"""

from typing import Optional, List, Dict, Any
import logging

# Configure logging for the module
logger = logging.getLogger(__name__)

def greet(name: str, greeting: str = "Hello") -> str:
    """
    Greet a person with a customizable greeting.
    
    Args:
        name (str): The name of the person to greet.
        greeting (str, optional): The greeting to use. Defaults to "Hello".
    
    Returns:
        str: A formatted greeting message.
    
    Raises:
        ValueError: If name is empty or None.
    
    Example:
        >>> greet("Alice")
        'Hello, Alice!'
        >>> greet("Bob", "Hi")
        'Hi, Bob!'
    """
    if not name:
        raise ValueError("Name cannot be empty")
    
    logger.info(f"Greeting {name} with '{greeting}'")
    return f"{greeting}, {name}!"

class DataProcessor:
    """
    A class for processing data with various operations.
    
    This class provides methods for data validation, transformation,
    and analysis.
    
    Attributes:
        data (List[Any]): The data to process.
        processed (bool): Whether the data has been processed.
    
    Example:
        >>> processor = DataProcessor([1, 2, 3, 4, 5])
        >>> processor.filter_even()
        >>> print(processor.data)
        [2, 4]
    """
    
    def __init__(self, data: List[Any]):
        """
        Initialize the DataProcessor.
        
        Args:
            data (List[Any]): Initial data to process.
        """
        self.data = data.copy() if data else []
        self.processed = False
    
    def filter_even(self) -> None:
        """
        Filter the data to keep only even numbers.
        
        Raises:
            TypeError: If data contains non-numeric values.
        """
        try:
            self.data = [x for x in self.data if isinstance(x, (int, float)) and x % 2 == 0]
            self.processed = True
        except TypeError as e:
            raise TypeError(f"Data contains non-numeric values: {e}")
    
    def get_summary(self) -> Dict[str, Any]:
        """
        Get a summary of the processed data.
        
        Returns:
            Dict[str, Any]: Summary statistics including count, min, max, and mean.
        """
        if not self.data:
            return {"count": 0, "min": None, "max": None, "mean": None}
        
        numeric_data = [x for x in self.data if isinstance(x, (int, float))]
        return {
            "count": len(numeric_data),
            "min": min(numeric_data) if numeric_data else None,
            "max": max(numeric_data) if numeric_data else None,
            "mean": sum(numeric_data) / len(numeric_data) if numeric_data else None
        }
```

### 4.4 Module Testing

Include tests to ensure your module works correctly:

```python
# test_calculator.py
"""
Test module for calculator.py

This module contains unit tests for the calculator module.
Run with: python -m pytest test_calculator.py
"""

import pytest
import calculator

class TestBasicOperations:
    """Test basic arithmetic operations."""
    
    def test_add(self):
        assert calculator.add(2, 3) == 5
        assert calculator.add(-1, 1) == 0
        assert calculator.add(0.1, 0.2) == pytest.approx(0.3)
    
    def test_subtract(self):
        assert calculator.subtract(5, 3) == 2
        assert calculator.subtract(0, 5) == -5
    
    def test_multiply(self):
        assert calculator.multiply(3, 4) == 12
        assert calculator.multiply(-2, 3) == -6
    
    def test_divide(self):
        assert calculator.divide(6, 2) == 3
        assert calculator.divide(1, 3) == pytest.approx(0.3333, rel=1e-3)
        
        with pytest.raises(ZeroDivisionError):
            calculator.divide(1, 0)

class TestAdvancedOperations:
    """Test advanced mathematical operations."""
    
    def test_power(self):
        assert calculator.power(2, 3) == 8
        assert calculator.power(4, 0.5) == 2
    
    def test_square_root(self):
        assert calculator.square_root(16) == 4
        assert calculator.square_root(2) == pytest.approx(1.414, rel=1e-3)
        
        with pytest.raises(ValueError):
            calculator.square_root(-1)
    
    def test_factorial(self):
        assert calculator.factorial(0) == 1
        assert calculator.factorial(5) == 120
        
        with pytest.raises(ValueError):
            calculator.factorial(-1)

class TestStatisticalFunctions:
    """Test statistical functions."""
    
    def test_mean(self):
        assert calculator.mean([1, 2, 3, 4, 5]) == 3
        assert calculator.mean([10]) == 10
        
        with pytest.raises(ValueError):
            calculator.mean([])
    
    def test_median(self):
        assert calculator.median([1, 2, 3, 4, 5]) == 3
        assert calculator.median([1, 2, 3, 4]) == 2.5

if __name__ == "__main__":
    pytest.main([__file__])
```

### 4.5 Module Configuration

Use configuration files or environment variables for module settings:

```python
# config_module.py
"""
Configuration module with settings management.
"""

import os
import json
from typing import Dict, Any

class Config:
    """Configuration manager for the module."""
    
    def __init__(self, config_file: str = None):
        self.config_file = config_file or "config.json"
        self.settings = self._load_config()
    
    def _load_config(self) -> Dict[str, Any]:
        """Load configuration from file or environment."""
        config = {}
        
        # Try to load from file
        if os.path.exists(self.config_file):
            try:
                with open(self.config_file, 'r') as f:
                    config = json.load(f)
            except (json.JSONDecodeError, IOError):
                pass
        
        # Override with environment variables
        env_vars = {
            'DEBUG': os.getenv('DEBUG', 'False').lower() == 'true',
            'LOG_LEVEL': os.getenv('LOG_LEVEL', 'INFO'),
            'MAX_RETRIES': int(os.getenv('MAX_RETRIES', '3')),
        }
        
        config.update(env_vars)
        return config
    
    def get(self, key: str, default: Any = None) -> Any:
        """Get configuration value."""
        return self.settings.get(key, default)
    
    def set(self, key: str, value: Any) -> None:
        """Set configuration value."""
        self.settings[key] = value
    
    def save(self) -> None:
        """Save configuration to file."""
        with open(self.config_file, 'w') as f:
            json.dump(self.settings, f, indent=2)

# Global configuration instance
config = Config()

# Usage in other parts of the module
DEBUG = config.get('DEBUG', False)
LOG_LEVEL = config.get('LOG_LEVEL', 'INFO')
```

### 4.6 Module Versioning and Metadata

Include version information and metadata in your modules:

```python
# versioned_module.py
"""
Versioned Module Example

This module demonstrates how to include version information
and metadata in Python modules.
"""

# Version information
__version__ = "1.2.0"
__author__ = "Python Student"
__email__ = "student@example.com"
__license__ = "MIT"
__status__ = "Development"  # Development, Production, Deprecated

# Metadata
__all__ = ["public_function", "PublicClass"]  # Defines what's exported with "from module import *"

def public_function():
    """A public function that will be exported."""
    return "This is a public function"

def _private_function():
    """A private function (not exported)."""
    return "This is private"

class PublicClass:
    """A public class that will be exported."""
    pass

class _PrivateClass:
    """A private class (not exported)."""
    pass

# Version checking utility
def check_version(required_version: str) -> bool:
    """Check if the current version meets the requirement."""
    from packaging import version
    return version.parse(__version__) >= version.parse(required_version)
```

---

## 5. Module Search Path and PYTHONPATH

Understanding how Python locates modules is crucial for developing robust applications. The module search path determines where Python looks for modules when you import them.

### 5.1 Understanding sys.path

Python uses the `sys.path` list to determine where to search for modules:

```python
import sys
import os

# Display the current module search path
print("Current Python module search path:")
for i, path in enumerate(sys.path):
    print(f"{i+1:2d}. {path}")

# Add information about each path
print("\nPath analysis:")
for path in sys.path:
    if os.path.isdir(path):
        print(f"✓ {path} (exists)")
    else:
        print(f"✗ {path} (does not exist)")
```

### 5.2 Default Search Path Components

The default search path includes several components:

#### 5.2.1 Script Directory
```python
# The directory containing the script being run
import os
import sys

script_dir = os.path.dirname(os.path.abspath(__file__))
print(f"Script directory: {script_dir}")
print(f"First sys.path entry: {sys.path[0]}")
print(f"Match: {script_dir == sys.path[0]}")
```

#### 5.2.2 PYTHONPATH Environment Variable
```python
# Check PYTHONPATH environment variable
import os

pythonpath = os.environ.get('PYTHONPATH')
if pythonpath:
    print(f"PYTHONPATH: {pythonpath}")
    print("PYTHONPATH directories:")
    for path in pythonpath.split(os.pathsep):
        print(f"  - {path}")
else:
    print("PYTHONPATH not set")
```

#### 5.2.3 Standard Library Directories
```python
# Find standard library location
import sys
import os

# Standard library is typically in the Python installation directory
for path in sys.path:
    if 'lib/python' in path and 'site-packages' not in path:
        print(f"Standard library path: {path}")
        break
```

#### 5.2.4 Site-packages Directory
```python
# Find site-packages directory
import site
import sys

# Get site-packages directories
site_packages = site.getsitepackages()
print("Site-packages directories:")
for path in site_packages:
    print(f"  - {path}")

# User site-packages directory
user_site = site.getusersitepackages()
print(f"User site-packages: {user_site}")
```

### 5.3 Modifying sys.path

You can modify `sys.path` to include additional directories:

```python
import sys
import os

# Add a directory to the beginning of sys.path
custom_path = "/path/to/custom/modules"
if custom_path not in sys.path:
    sys.path.insert(0, custom_path)

# Add a directory to the end of sys.path
another_path = "/path/to/another/directory"
if another_path not in sys.path:
    sys.path.append(another_path)

# Remove a directory from sys.path
if custom_path in sys.path:
    sys.path.remove(custom_path)

# Example: Add project root to path
project_root = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
if project_root not in sys.path:
    sys.path.insert(0, project_root)
```

### 5.4 Using PYTHONPATH Environment Variable

PYTHONPATH allows you to add directories to the module search path:

```bash
# Setting PYTHONPATH in bash
export PYTHONPATH="/path/to/modules:/another/path/to/modules:$PYTHONPATH"

# Setting PYTHONPATH in Windows
set PYTHONPATH=C:\path\to\modules;C:\another\path\to\modules;%PYTHONPATH%

# Running Python with PYTHONPATH
PYTHONPATH="/custom/modules" python script.py
```

```python
# pythonpath_example.py
"""
Example of using PYTHONPATH for module discovery.
"""

import os
import sys

def show_pythonpath_info():
    """Display information about PYTHONPATH."""
    pythonpath = os.environ.get('PYTHONPATH')
    
    if pythonpath:
        print("PYTHONPATH is set:")
        paths = pythonpath.split(os.pathsep)
        for i, path in enumerate(paths, 1):
            exists = "✓" if os.path.exists(path) else "✗"
            print(f"  {i}. {exists} {path}")
    else:
        print("PYTHONPATH is not set")
    
    print(f"\nFirst few sys.path entries:")
    for i, path in enumerate(sys.path[:5], 1):
        print(f"  {i}. {path}")

if __name__ == "__main__":
    show_pythonpath_info()
```

### 5.5 Module Resolution Algorithm

Python follows a specific algorithm to resolve module imports:

```python
# module_resolution.py
"""
Demonstrates the module resolution process.
"""

import sys
import os
import importlib.util

def find_module_location(module_name):
    """Find where a module would be loaded from."""
    print(f"Searching for module: {module_name}")
    
    # Check if it's a built-in module
    if module_name in sys.builtin_module_names:
        print(f"  -> Built-in module")
        return
    
    # Try to find the module spec
    try:
        spec = importlib.util.find_spec(module_name)
        if spec is None:
            print(f"  -> Module not found")
            return
        
        print(f"✓ Module spec found:")
        print(f"   Name: {spec.name}")
        print(f"   Origin: {spec.origin}")
        print(f"   Package: {spec.parent}")
        
        # Show which directory in sys.path contains the module
        if spec.origin:
            module_dir = os.path.dirname(spec.origin)
            for i, path in enumerate(sys.path):
                if os.path.commonpath([module_dir, path]) == path:
                    print(f"  -> From sys.path[{i}]: {path}")
                    break

    except Exception as e:
        print(f"✗ Error finding module spec: {e}")
        traceback.print_exc()

# Test with various modules
test_modules = ['os', 'sys', 'math', 'json', 'numpy', 'requests']
for module in test_modules:
    find_module_location(module)
    print()
```

### 5.6 Package Discovery

Python searches for packages (directories with `__init__.py`) in the module search path:

```python
# package_discovery.py
"""
Demonstrates package discovery mechanisms.
"""

import os
import sys
import importlib.util

def discover_packages_in_path(search_path):
    """Discover packages in a given path."""
    if not os.path.exists(search_path):
        return []
    
    packages = []
    for item in os.listdir(search_path):
        if item.endswith('.py') and not item.startswith('_'):
            module_name = item[:-3]  # Remove .py extension
            try:
                module = importlib.import_module(f'{__package__}.plugins.{module_name}')
                # Look for plugin registration function
                if hasattr(module, 'register'):
                    module.register()
            except ImportError as e:
                print(f"Failed to load plugin {module_name}: {e}")

# Load plugins on package import
load_plugins()
```

### 5.7 Virtual Environments and Module Paths

Virtual environments modify the module search path:

```python
# virtual_env_info.py
"""
Display information about virtual environment and module paths.
"""

import sys
import os

def check_virtual_env():
    """Check if running in a virtual environment."""
    # Check for virtual environment
    if hasattr(sys, 'real_prefix'):
        # virtualenv
        print("Running in virtualenv")
        print(f"Virtual environment: {sys.prefix}")
        print(f"Real prefix: {sys.real_prefix}")
    elif hasattr(sys, 'base_prefix') and sys.base_prefix != sys.prefix:
        # venv (Python 3.3+)
        print("Running in venv")
        print(f"Virtual environment: {sys.prefix}")
        print(f"Base prefix: {sys.base_prefix}")
    else:
        print("Not running in virtual environment")
        print(f"Python prefix: {sys.prefix}")

def show_site_packages():
    """Show site-packages directories."""
    import site
    
    print("\nSite-packages directories:")
    for path in site.getsitepackages():
        exists = "✓" if os.path.exists(path) else "✗"
        print(f"  {exists} {path}")
    
    user_site = site.getusersitepackages()
    user_exists = "✓" if os.path.exists(user_site) else "✗"
    print(f"  {user_exists} {user_site} (user)")

if __name__ == "__main__":
    check_virtual_env()
    show_site_packages()
```

### 5.8 Debugging Module Import Issues

Tools and techniques for debugging import problems:

```python
# import_debugger.py
"""
Tools for debugging module import issues.
"""

import sys
import os
import importlib.util
import traceback

def debug_import(module_name):
    """Debug module import issues."""
    print(f"Debugging import of '{module_name}'")
    print("=" * 50)
    
    # Check if module is already imported
    if module_name in sys.modules:
        print(f"✓ Module already imported: {sys.modules[module_name]}")
        return
    
    # Check if it's a built-in module
    if module_name in sys.builtin_module_names:
        print(f"✓ Built-in module: {module_name}")
        return
    
    # Try to find the module spec
    try:
        spec = importlib.util.find_spec(module_name)
        if spec is None:
            print(f"✗ Module spec not found")
            print("Searching in sys.path:")
            for i, path in enumerate(sys.path):
                potential_file = os.path.join(path, f"{module_name}.py")
                potential_dir = os.path.join(path, module_name)
                print(f"  {i+1}. {path}")
                print(f"     File: {potential_file} {'✓' if os.path.exists(potential_file) else '✗'}")
                print(f"     Dir:  {potential_dir} {'✓' if os.path.exists(potential_dir) else '✗'}")
            return
        
        print(f"✓ Module spec found:")
        print(f"   Name: {spec.name}")
        print(f"   Origin: {spec.origin}")
        print(f"   Package: {spec.parent}")
        
        # Try to import the module
        try:
            module = importlib.util.module_from_spec(spec)
            spec.loader.exec_module(module)
            print(f"✓ Module loaded successfully")
        except Exception as e:
            print(f"✗ Error loading module: {e}")
            traceback.print_exc()
    
    except Exception as e:
        print(f"✗ Error finding module spec: {e}")
        traceback.print_exc()

def check_import_path():
    """Check the current import path configuration."""
    print("Import Path Configuration")
    print("=" * 30)
    
    print(f"Python executable: {sys.executable}")
    print(f"Python version: {sys.version}")
    print(f"Current working directory: {os.getcwd()}")
    
    pythonpath = os.environ.get('PYTHONPATH')
    if pythonpath:
        print(f"PYTHONPATH: {pythonpath}")
    else:
        print("PYTHONPATH: Not set")
    
    print(f"\nsys.path ({len(sys.path)} entries):")
    for i, path in enumerate(sys.path):
        exists = "✓" if os.path.exists(path) else "✗"
        print(f"  {i+1:2d}. {exists} {path}")

if __name__ == "__main__":
    # Example usage
    check_import_path()
    print("\n")
    debug_import("nonexistent_module")
```

### 5.9 Best Practices for Module Paths

#### 5.9.1 Project Structure
```
project/
├── src/
│   └── my_package/
│       ├── __init__.py
│       └── module.py
├── tests/
├── setup.py
└── main.py
```

#### 5.9.2 Path Management
```python
# path_management.py
"""
Best practices for managing module paths in projects.
"""

import os
import sys
from pathlib import Path

def setup_project_path():
    """Set up the project path for imports."""
    # Get the directory containing this script
    current_dir = Path(__file__).parent
    
    # Find the project root (directory containing setup.py or pyproject.toml)
    project_root = current_dir
    while project_root.parent != project_root:
        if (project_root / 'setup.py').exists() or (project_root / 'pyproject.toml').exists():
            break
        project_root = project_root.parent
    
    # Add project root to sys.path if not already there
    project_root_str = str(project_root)
    if project_root_str not in sys.path:
        sys.path.insert(0, project_root_str)
    
    # Add src directory if it exists
    src_dir = project_root / 'src'
    if src_dir.exists():
        src_dir_str = str(src_dir)
        if src_dir_str not in sys.path:
            sys.path.insert(0, src_dir_str)
    
    return project_root

# Call at module level
PROJECT_ROOT = setup_project_path()
```

---

## 6. Package Structure and Organization

A package is a directory containing multiple Python modules organized in a hierarchical structure. Packages provide a way to organize related modules and create namespaces to avoid naming conflicts.

### 6.1 What is a Package?

A package is a directory that contains:
- An `__init__.py` file (can be empty)
- One or more Python modules (`.py` files)
- Optionally, subpackages (subdirectories with their own `__init__.py`)

```
my_package/
├── __init__.py          # Makes it a package
├── module1.py           # Module in the package
├── module2.py           # Another module
├── subpackage/          # Subpackage
│   ├── __init__.py      # Makes subpackage a package
│   └── submodule.py     # Module in subpackage
└── data/                # Non-Python files
    └── config.json
```

### 6.2 Creating a Simple Package

Let's create a comprehensive example package:

```python
# my_math_package/__init__.py
"""
My Math Package

A comprehensive mathematical operations package.
"""

# Package metadata
__version__ = "1.0.0"
__author__ = "Python Student"
__email__ = "student@example.com"

# Package-level initialization
print(f"My Math Package v{__version__} initializing...")

# Set up default logging configuration
import logging
import sys
from datetime import datetime

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.StreamHandler(sys.stdout),
        logging.FileHandler('package.log')
    ]
)

# Create package logger
logger = logging.getLogger(__name__)
logger.info(f"Package initialized at {datetime.now()}")

# Package-level constants
LOG_LEVELS = {
    'DEBUG': logging.DEBUG,
    'INFO': logging.INFO,
    'WARNING': logging.WARNING,
    'ERROR': logging.ERROR,
    'CRITICAL': logging.CRITICAL
}

# Package-level variables
_initialized = True
_init_time = datetime.now()

def get_init_info():
    """Get package initialization information."""
    return {
        'initialized': _initialized,
        'init_time': _init_time,
        'version': __version__
    }
```

```python
# my_math_package/basic_operations.py
"""
Basic mathematical operations module.
"""

from typing import Union

Number = Union[int, float]

def add(a: Number, b: Number) -> Number:
    """Add two numbers."""
    return a + b

def subtract(a: Number, b: Number) -> Number:
    """Subtract b from a."""
    return a - b

def multiply(a: Number, b: Number) -> Number:
    """Multiply two numbers."""
    return a * b

def divide(a: Number, b: Number) -> float:
    """Divide a by b."""
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return a / b
```

```python
# my_math_package/advanced_operations.py
"""
Advanced mathematical operations module.
"""

import math
from typing import Union

Number = Union[int, float]

def power(base: Number, exponent: Number) -> Number:
    """Calculate base raised to the power of exponent."""
    return base ** exponent

def logarithm(x: Number, base: Number = math.e) -> float:
    """Calculate logarithm of x with given base."""
    if x <= 0:
        raise ValueError("Logarithm undefined for non-positive numbers")
    if base <= 0 or base == 1:
        raise ValueError("Logarithm base must be positive and not equal to 1")
    
    if base == math.e:
        return math.log(x)
    return math.log(x) / math.log(base)

class trigonometric:
    """Trigonometric functions."""
    
    @staticmethod
    def sin(x: Number) -> float:
        """Calculate sine of x (in radians)."""
        return math.sin(x)
    
    @staticmethod
    def cos(x: Number) -> float:
        """Calculate cosine of x (in radians)."""
        return math.cos(x)
    
    @staticmethod
    def tan(x: Number) -> float:
        """Calculate tangent of x (in radians)."""
        return math.tan(x)
    
    @staticmethod
    def degrees_to_radians(degrees: Number) -> float:
        """Convert degrees to radians."""
        return degrees * math.pi / 180
    
    @staticmethod
    def radians_to_degrees(radians: Number) -> float:
        """Convert radians to degrees."""
        return radians * 180 / math.pi
```

```python
# my_math_package/statistics.py
"""
Statistical functions module.
"""

from typing import List, Union
import math

Number = Union[int, float]

def mean(data: List[Number]) -> float:
    """Calculate the arithmetic mean."""
    if not data:
        raise ValueError("Cannot calculate mean of empty list")
    return sum(data) / len(data)

def median(data: List[Number]) -> float:
    """Calculate the median."""
    if not data:
        raise ValueError("Cannot calculate median of empty list")
    
    sorted_data = sorted(data)
    n = len(sorted_data)
    
    if n % 2 == 0:
        return (sorted_data[n//2 - 1] + sorted_data[n//2]) / 2
    else:
        return sorted_data[n//2]

def mode(data: List[Number]) -> List[Number]:
    """Calculate the mode(s)."""
    if not data:
        raise ValueError("Cannot calculate mode of empty list")
    
    frequency = {}
    for value in data:
        frequency[value] = frequency.get(value, 0) + 1
    
    max_frequency = max(frequency.values())
    modes = [value for value, freq in frequency.items() if freq == max_frequency]
    
    return modes

def standard_deviation(data: List[Number], sample: bool = True) -> float:
    """Calculate standard deviation."""
    if not data:
        raise ValueError("Cannot calculate standard deviation of empty list")
    
    if len(data) == 1 and sample:
        raise ValueError("Sample standard deviation requires at least 2 data points")
    
    avg = mean(data)
    variance = sum((x - avg) ** 2 for x in data) / (len(data) - (1 if sample else 0))
    return math.sqrt(variance)
```

### 6.3 Creating Subpackages

Subpackages provide deeper organization:

```python
# my_math_package/geometry/__init__.py
"""
Geometry subpackage for 2D and 3D geometric calculations.
"""

from .shapes_2d import Circle, Rectangle, Triangle
from .shapes_3d import Sphere, Cube, Cylinder

__all__ = ['Circle', 'Rectangle', 'Triangle', 'Sphere', 'Cube', 'Cylinder']
```

```python
# my_math_package/geometry/shapes_2d.py
"""
2D geometric shapes.
"""

import math
from typing import Union

Number = Union[int, float]

class Circle:
    """A circle shape."""
    
    def __init__(self, radius: Number):
        if radius <= 0:
            raise ValueError("Radius must be positive")
        self.radius = radius
    
    def area(self) -> float:
        """Calculate area of the circle."""
        return math.pi * self.radius ** 2
    
    def circumference(self) -> float:
        """Calculate circumference of the circle."""
        return 2 * math.pi * self.radius
    
    def __repr__(self):
        return f"Circle(radius={self.radius})"

class Rectangle:
    """A rectangle shape."""
    
    def __init__(self, width: Number, height: Number):
        if width <= 0 or height <= 0:
            raise ValueError("Width and height must be positive")
        self.width = width
        self.height = height
    
    def area(self) -> float:
        """Calculate area of the rectangle."""
        return self.width * self.height
    
    def perimeter(self) -> float:
        """Calculate perimeter of the rectangle."""
        return 2 * (self.width + self.height)
    
    def __repr__(self):
        return f"Rectangle(width={self.width}, height={self.height})"

class Triangle:
    """A triangle shape."""
    
    def __init__(self, side_a: Number, side_b: Number, side_c: Number):
        if side_a <= 0 or side_b <= 0 or side_c <= 0:
            raise ValueError("All sides must be positive")
        
        # Check triangle inequality
        if (side_a + side_b <= side_c or 
            side_a + side_c <= side_b or 
            side_b + side_c <= side_a):
            raise ValueError("Sides do not form a valid triangle")
        
        self.side_a = side_a
        self.side_b = side_b
        self.side_c = side_c
    
    def area(self) -> float:
        """Calculate area using Heron's formula."""
        s = (self.side_a + self.side_b + self.side_c) / 2
        return math.sqrt(s * (s - self.side_a) * (s - self.side_b) * (s - self.side_c))
    
    def perimeter(self) -> float:
        """Calculate perimeter of the triangle."""
        return self.side_a + self.side_b + self.side_c
    
    def __repr__(self):
        return f"Triangle(sides={self.side_a}, {self.side_b}, {self.side_c})"
```

### 6.4 Package Import Examples

Using the package we created:

```python
# example_usage.py
"""
Examples of using the my_math_package.
"""

# Import entire package
import my_math_package

# Use package-level functions
result1 = my_math_package.add(5, 3)
result2 = my_math_package.power(2, 3)

print(f"5 + 3 = {result1}")
print(f"2^3 = {result2}")

# Import specific modules
from my_math_package import basic_operations, statistics

# Use module functions
numbers = [1, 2, 3, 4, 5]
avg = statistics.mean(numbers)
total = basic_operations.add(10, 20)

print(f"Average of {numbers}: {avg}")
print(f"10 + 20 = {total}")

# Import from subpackage
from my_math_package.geometry import Circle, Rectangle

# Use subpackage classes
circle = Circle(5)
rectangle = Rectangle(4, 6)

print(f"Circle area: {circle.area():.2f}")
print(f"Rectangle area: {rectangle.area()}")

# Import with aliases
from my_math_package.advanced_operations import trigonometric as trig

# Use aliased imports
angle = trig.degrees_to_radians(45)
sine_value = trig.sin(angle)
print(f"sin(45°) = {sine_value:.4f}")
```

### 6.5 Package Organization Patterns

#### 6.5.1 Flat Package Structure
```
simple_package/
├── __init__.py
├── module1.py
├── module2.py
└── module3.py
```

#### 6.5.2 Layered Package Structure
```
web_framework/
├── __init__.py
├── core/
│   ├── __init__.py
│   ├── server.py
│   └── routing.py
├── middleware/
│   ├── __init__.py
│   ├── auth.py
│   └── cors.py
└── utils/
    ├── __init__.py
    ├── helpers.py
    └── validators.py
```

#### 6.5.3 Feature-Based Structure
```
ecommerce/
├── __init__.py
├── products/
│   ├── __init__.py
│   ├── models.py
│   ├── views.py
│   └── serializers.py
├── orders/
│   ├── __init__.py
│   ├── models.py
│   ├── views.py
│   └── serializers.py
└── users/
    ├── __init__.py
    ├── models.py
    ├── views.py
    └── serializers.py
```

### 6.6 Package Documentation

Document your package structure:

```python
# documented_package/__init__.py
"""
Documented Package

A well-documented Python package demonstrating best practices.

Modules:
    core: Core functionality
    utils: Utility functions
    exceptions: Custom exceptions

Classes:
    DataProcessor: Main data processing class
    ConfigManager: Configuration management

Functions:
    process_data: Main data processing function
    validate_input: Input validation

Example:
    >>> import documented_package
    >>> processor = documented_package.DataProcessor()
    >>> result = processor.process([1, 2, 3, 4, 5])
"""

from .core import DataProcessor, process_data
from .utils import validate_input, format_output
from .exceptions import ProcessingError, ValidationError

__version__ = "1.0.0"
__author__ = "Package Author"
__license__ = "MIT"

__all__ = [
    'DataProcessor', 'process_data', 'validate_input', 'format_output',
    'ProcessingError', 'ValidationError'
]
```

### 6.7 Package Testing Structure

Organize tests for your package:

```
my_package/
├── my_package/
│   ├── __init__.py
│   ├── core.py
│   └── utils.py
├── tests/
│   ├── __init__.py
│   ├── test_core.py
│   ├── test_utils.py
│   └── test_integration.py
├── setup.py
└── README.md
```

```python
# tests/test_core.py
"""
Tests for the core module.
"""

import pytest
import sys
import os

# Add parent directory to path for imports
sys.path.insert(0, os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

from my_package.core import DataProcessor

class TestDataProcessor:
    """Test cases for DataProcessor class."""
    
    def setup_method(self):
        """Set up test fixtures."""
        self.processor = DataProcessor()
    
    def test_process_valid_data(self):
        """Test processing with valid data."""
        data = [1, 2, 3, 4, 5]
        result = self.processor.process(data)
        assert result is not None
        assert len(result) == len(data)
    
    def test_process_empty_data(self):
        """Test processing with empty data."""
        with pytest.raises(ValueError):
            self.processor.process([])
    
    def teardown_method(self):
        """Clean up after tests."""
        pass
```

### 6.8 Package Configuration

Include configuration files in your package:

```python
# my_package/config.py
"""
Configuration module for the package.
"""

import os
import json
from typing import Dict, Any

class Config:
    """Package configuration manager."""
    
    def __init__(self, config_path: str = None):
        self.config_path = config_path or self._find_config_file()
        self.config = self._load_config()
    
    def _find_config_file(self) -> str:
        """Find the configuration file."""
        possible_paths = [
            os.path.join(os.path.dirname(__file__), 'config.json'),
            os.path.expanduser('~/.my_package/config.json'),
            '/etc/my_package/config.json'
        ]
        
        for path in possible_paths:
            if os.path.exists(path):
                return path
        
        # Return default path if none found
        return possible_paths[0]
    
    def _load_config(self) -> Dict[str, Any]:
        """Load configuration from various sources."""
        default_config = {
            'debug': False,
            'log_level': 'INFO',
            'max_workers': 4,
            'timeout': 30,
            'cache_enabled': True
        }
        
        # Load from environment variables
        for key in default_config:
            env_key = f"PACKAGE_{key.upper()}"
            if env_key in os.environ:
                default_config[key] = self._convert_value(os.environ[env_key])
        
        # Load from config file
        config_file = os.path.join(os.path.dirname(__file__), 'config.json')
        if os.path.exists(config_file):
            try:
                with open(config_file, 'r') as f:
                    file_config = json.load(f)
                    default_config.update(file_config)
            except (json.JSONDecodeError, IOError):
                pass
        
        return default_config
    
    def _convert_value(self, value: str) -> Any:
        """Convert string value to appropriate type."""
        if value.lower() in ('true', 'false'):
            return value.lower() == 'true'
        try:
            return int(value)
        except ValueError:
            try:
                return float(value)
            except ValueError:
                return value
    
    def get(self, key: str, default: Any = None) -> Any:
        """Get configuration value."""
        return self.config.get(key, default)
    
    def set(self, key: str, value: Any) -> None:
        """Set configuration value."""
        self.config[key] = value
    
    def update(self, config: Dict[str, Any]) -> None:
        """Update configuration with dictionary."""
        self.config.update(config)

# Global configuration instance
config = Config()

# Configure logging based on package configuration
import logging
logging.basicConfig(
    level=getattr(logging, config.get('log_level', 'INFO')),
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

# Import modules based on configuration
if config.get('cache_enabled', True):
    from .cache import CacheManager
    cache = CacheManager()
else:
    cache = None

# Export configuration
__all__ = ['config', 'cache']
```

### 6.9 Error Handling in __init__.py

Handle errors gracefully during package initialization:

```python
# robust_package/__init__.py
"""
Package with robust error handling.
"""

import sys
import logging
from typing import Optional

__version__ = "1.0.0"
__all__ = []

# Set up logging for initialization
logger = logging.getLogger(__name__)

# Track initialization status
_initialization_errors = []
_successfully_imported = []

def safe_import(module_name: str, attr_name: str = None) -> Optional[object]:
    """Safely import a module or attribute."""
    try:
        if attr_name:
            module = __import__(f"{__package__}.{module_name}", fromlist=[attr_name])
            result = getattr(module, attr_name)
        else:
            result = __import__(f"{__package__}.{module_name}")
        
        _successfully_imported.append(module_name)
        return result
    except ImportError as e:
        error_msg = f"Failed to import {module_name}: {e}"
        _initialization_errors.append(error_msg)
        logger.warning(error_msg)
        return None
    except Exception as e:
        error_msg = f"Unexpected error importing {module_name}: {e}"
        _initialization_errors.append(error_msg)
        logger.error(error_msg)
        return None

# Core functionality (must be available)
try:
    from .core import CoreClass, core_function
    __all__.extend(['CoreClass', 'core_function'])
except ImportError as e:
    logger.critical(f"Failed to import core functionality: {e}")
    raise ImportError(f"Package cannot function without core module: {e}")

# Optional functionality
statistics = safe_import('statistics')
if statistics:
    from .statistics import mean, median
    __all__.extend(['mean', 'median'])

# Advanced features (may not be available)
advanced = safe_import('advanced_features')
if advanced:
    from .advanced_features import AdvancedProcessor
    __all__.append('AdvancedProcessor')

# Third-party integrations
numpy_support = safe_import('numpy_integration', 'NumpyProcessor')
if numpy_support:
    globals()['NumpyProcessor'] = numpy_support
    __all__.append('NumpyProcessor')

def get_initialization_status():
    """Get package initialization status."""
    return {
        'version': __version__,
        'successfully_imported': _successfully_imported,
        'errors': _initialization_errors,
        'available_features': __all__
    }

# Add status function to exports
__all__.append('get_initialization_status')
```

### 6.10 Testing __init__.py

Test your package initialization:

```python
# test_init.py
"""
Tests for package initialization.
"""

import pytest
import sys
import importlib

def test_package_imports():
    """Test that package imports successfully."""
    import my_package
    assert hasattr(my_package, '__version__')

def test_package_all():
    """Test __all__ definition."""
    import my_package
    assert hasattr(my_package, '__all__')
    assert isinstance(my_package.__all__, list)

def test_package_functions_available():
    """Test that package functions are available."""
    import my_package
    for func_name in my_package.__all__:
        assert hasattr(my_package, func_name)

def test_package_reload():
    """Test package reloading."""
    import my_package
    original_version = my_package.__version__
    
    # Reload package
    importlib.reload(my_package)
    
    # Check that version is still the same
    assert my_package.__version__ == original_version

def test_package_initialization_idempotent():
    """Test that multiple imports don't cause issues."""
    import my_package
    first_import_time = my_package.get_init_info()['init_time']
    
    # Import again
    importlib.reload(my_package)
    second_import_time = my_package.get_init_info()['init_time']
    
    # Should be the same (cached)
    assert first_import_time == second_import_time
```

---

## 7. The __init__.py File

The `__init__.py` file is a crucial component of Python packages. It serves multiple purposes and provides fine-grained control over package behavior and imports.

### 7.1 Purpose and Functionality

The `__init__.py` file serves several important functions:

1. **Package Recognition**: Makes Python treat a directory as a package
2. **Initialization Code**: Runs when the package is first imported
3. **API Control**: Controls what gets imported with `from package import *`
4. **Namespace Management**: Provides a clean interface to package contents

### 7.2 Empty __init__.py Files

The simplest `__init__.py` file is empty:

```python
# my_package/__init__.py
# Empty file - just makes the directory a package
```

Even an empty `__init__.py` file is useful because it:
- Allows the directory to be treated as a package
- Enables imports like `import my_package`
- Prevents accidental imports of directories without `__init__.py`

### 7.3 Package Initialization Code

`__init__.py` can contain initialization code that runs when the package is imported:

```python
# logger_package/__init__.py
"""
Logger Package

A comprehensive logging package with multiple handlers.
"""

import logging
import sys
from datetime import datetime

# Package metadata
__version__ = "1.0.0"
__author__ = "Python Student"

# Package-level initialization
print(f"Logger Package v{__version__} initializing...")

# Set up default logging configuration
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.StreamHandler(sys.stdout),
        logging.FileHandler('package.log')
    ]
)

# Create package logger
logger = logging.getLogger(__name__)
logger.info(f"Package initialized at {datetime.now()}")

# Package-level constants
LOG_LEVELS = {
    'DEBUG': logging.DEBUG,
    'INFO': logging.INFO,
    'WARNING': logging.WARNING,
    'ERROR': logging.ERROR,
    'CRITICAL': logging.CRITICAL
}

# Package-level variables
_initialized = True
_init_time = datetime.now()

def get_init_info():
    """Get package initialization information."""
    return {
        'initialized': _initialized,
        'init_time': _init_time,
        'version': __version__
    }
```

### 7.4 Controlling Package Imports

Use `__init__.py` to control what gets imported from your package:

```python
# math_package/__init__.py
"""
Math Package with controlled imports.
"""

# Import specific functions from modules
from .basic_operations import add, subtract, multiply, divide
from .advanced_operations import power, square_root, logarithm
from .statistics import mean, median, mode, standard_deviation
from .geometry import Circle, Rectangle, Triangle

# Import entire modules (accessible as submodules)
from . import constants
from . import utilities

# Package metadata
__version__ = "2.0.0"
__author__ = "Math Team"

# Control what gets imported with "from math_package import *"
__all__ = [
    # Basic operations
    'add', 'subtract', 'multiply', 'divide',
    # Advanced operations  
    'power', 'square_root', 'logarithm',
    # Statistics
    'mean', 'median', 'mode', 'standard_deviation',
    # Geometry
    'Circle', 'Rectangle', 'Triangle',
    # Modules
    'constants', 'utilities'
]

# Package-level convenience functions
def info():
    """Display package information."""
    return {
        'name': __name__,
        'version': __version__,
        'author': __author__,
        'functions': __all__
    }

# Conditional imports based on availability
try:
    import numpy as np
    from .numpy_extensions import enhanced_stats
    __all__.append('enhanced_stats')
    HAS_NUMPY = True
except ImportError:
    HAS_NUMPY = False

# Package-level configuration
DEFAULT_PRECISION = 10
```

### 7.5 Advanced __init__.py Patterns

#### 7.5.1 Lazy Loading
```python
# lazy_package/__init__.py
"""
Package with lazy loading of heavy modules.
"""

import sys
from typing import Any

__version__ = "1.0.0"
__all__ = ['heavy_module', 'light_module']

# Light module - import immediately
from .light_module import quick_function

class LazyLoader:
    """Lazy loader for heavy modules."""
    
    def __init__(self, module_name: str):
        self.module_name = module_name
        self._module = None
    
    def __getattr__(self, name: str) -> Any:
        if self._module is None:
            # Import the module only when first accessed
            self._module = __import__(f"{__package__}.{self.module_name}", fromlist=[name])
        return getattr(self._module, name)

# Create lazy loader for heavy module
heavy_module = LazyLoader('heavy_processing')

# Module will be loaded only when first accessed
# Example: heavy_module.complex_calculation()
```

#### 7.5.2 Plugin System
```python
# plugin_package/__init__.py
"""
Package with plugin discovery system.
"""

import os
import importlib
from typing import Dict, List, Any

__version__ = "1.0.0"
__all__ = ['load_plugins', 'get_plugins', 'register_plugin']

# Plugin registry
_plugins: Dict[str, Any] = {}

def register_plugin(name: str, plugin: Any) -> None:
    """Register a plugin."""
    _plugins[name] = plugin

def get_plugins() -> Dict[str, Any]:
    """Get all registered plugins."""
    return _plugins.copy()

def load_plugins() -> None:
    """Automatically load plugins from plugins directory."""
    plugins_dir = os.path.join(os.path.dirname(__file__), 'plugins')
    if not os.path.exists(plugins_dir):
        return
    
    for filename in os.listdir(plugins_dir):
        if filename.endswith('.py') and not filename.startswith('_'):
            module_name = filename[:-3]  # Remove .py extension
            try:
                module = importlib.import_module(f'{__package__}.plugins.{module_name}')
                # Look for plugin registration function
                if hasattr(module, 'register'):
                    module.register()
            except ImportError as e:
                print(f"Failed to load plugin {module_name}: {e}")

# Load plugins on package import
load_plugins()
```

#### 7.5.3 API Versioning
```python
# versioned_package/__init__.py
"""
Package with API versioning support.
"""

import sys
import warnings
from typing import Any

__version__ = "3.0.0"
__api_version__ = "3.0"

# Current API (v3)
from .v3 import new_function, improved_class

# Deprecated API (v2) - with warnings
def old_function(*args, **kwargs):
    """Deprecated function from v2 API."""
    warnings.warn(
        "old_function is deprecated and will be removed in v4.0. Use new_function instead.",
        DeprecationWarning,
        stacklevel=2
    )
    from .v2 import old_function as _old_function
    return _old_function(*args, **kwargs)

# Version compatibility check
def check_compatibility(required_version: str) -> bool:
    """Check if the package version is compatible with required version."""
    from packaging import version
    return version.parse(__version__) >= version.parse(required_version)

# API selection based on import
class APISelector:
    """Select API version based on import."""
    
    def __init__(self):
        self._apis = {
            'v2': lambda: importlib.import_module(f'{__package__}.v2'),
            'v3': lambda: importlib.import_module(f'{__package__}.v3')
        }
    
    def get_api(self, version: str = None):
        """Get API for specific version."""
        if version is None:
            version = 'v3'  # Default to latest
        
        if version not in self._apis:
            raise ValueError(f"API version {version} not supported")
        
        return self._apis[version]()

# Export API selector
api = APISelector()
```

### 7.6 Package Configuration in __init__.py

Configure package behavior through `__init__.py`:

```python
# configurable_package/__init__.py
"""
Package with configuration support.
"""

import os
import json
from typing import Dict, Any, Optional

__version__ = "1.0.0"

# Default configuration
DEFAULT_CONFIG = {
    'debug': False,
    'log_level': 'INFO',
    'max_workers': 4,
    'timeout': 30,
    'cache_enabled': True
}

class PackageConfig:
    """Package configuration manager."""
    
    def __init__(self):
        self.config = DEFAULT_CONFIG.copy()
        self._load_config()
    
    def _load_config(self):
        """Load configuration from various sources."""
        # Load from environment variables
        for key in self.config:
            env_key = f"PACKAGE_{key.upper()}"
            if env_key in os.environ:
                self.config[key] = self._convert_value(os.environ[env_key])
        
        # Load from config file
        config_file = os.path.join(os.path.dirname(__file__), 'config.json')
        if os.path.exists(config_file):
            try:
                with open(config_file, 'r') as f:
                    file_config = json.load(f)
                    self.config.update(file_config)
            except (json.JSONDecodeError, IOError):
                pass
    
    def _convert_value(self, value: str) -> Any:
        """Convert string value to appropriate type."""
        if value.lower() in ('true', 'false'):
            return value.lower() == 'true'
        try:
            return int(value)
        except ValueError:
            try:
                return float(value)
            except ValueError:
                return value
    
    def get(self, key: str, default: Any = None) -> Any:
        """Get configuration value."""
        return self.config.get(key, default)
    
    def set(self, key: str, value: Any) -> None:
        """Set configuration value."""
        self.config[key] = value
    
    def update(self, config: Dict[str, Any]) -> None:
        """Update configuration with dictionary."""
        self.config.update(config)

# Global configuration instance
config = PackageConfig()

# Configure logging based on package configuration
import logging
logging.basicConfig(
    level=getattr(logging, config.get('log_level', 'INFO')),
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

# Import modules based on configuration
if config.get('cache_enabled', True):
    from .cache import CacheManager
    cache = CacheManager()
else:
    cache = None

# Export configuration
__all__ = ['config', 'cache']
```

### 7.7 Error Handling in __init__.py

Handle errors gracefully during package initialization:

```python
# robust_package/__init__.py
"""
Package with robust error handling.
"""

import sys
import logging
from typing import Optional

__version__ = "1.0.0"
__all__ = []

# Set up logging for initialization
logger = logging.getLogger(__name__)

# Track initialization status
_initialization_errors = []
_successfully_imported = []

def safe_import(module_name: str, attr_name: str = None) -> Optional[object]:
    """Safely import a module or attribute."""
    try:
        if attr_name:
            module = __import__(f"{__package__}.{module_name}", fromlist=[attr_name])
            result = getattr(module, attr_name)
        else:
            result = __import__(f"{__package__}.{module_name}")
        
        _successfully_imported.append(module_name)
        return result
    except ImportError as e:
        error_msg = f"Failed to import {module_name}: {e}"
        _initialization_errors.append(error_msg)
        logger.warning(error_msg)
        return None
    except Exception as e:
        error_msg = f"Unexpected error importing {module_name}: {e}"
        _initialization_errors.append(error_msg)
        logger.error(error_msg)
        return None

# Core functionality (must be available)
try:
    from .core import CoreClass, core_function
    __all__.extend(['CoreClass', 'core_function'])
except ImportError as e:
    logger.critical(f"Failed to import core functionality: {e}")
    raise ImportError(f"Package cannot function without core module: {e}")

# Optional functionality
statistics = safe_import('statistics')
if statistics:
    from .statistics import mean, median
    __all__.extend(['mean', 'median'])

# Advanced features (may not be available)
advanced = safe_import('advanced_features')
if advanced:
    from .advanced_features import AdvancedProcessor
    __all__.append('AdvancedProcessor')

# Third-party integrations
numpy_support = safe_import('numpy_integration', 'NumpyProcessor')
if numpy_support:
    globals()['NumpyProcessor'] = numpy_support
    __all__.append('NumpyProcessor')

def get_initialization_status():
    """Get package initialization status."""
    return {
        'version': __version__,
        'successfully_imported': _successfully_imported,
        'errors': _initialization_errors,
        'available_features': __all__
    }

# Add status function to exports
__all__.append('get_initialization_status')
```

### 7.8 Testing __init__.py

Test your package initialization:

```python
# test_init.py
"""
Tests for package initialization.
"""

import pytest
import sys
import importlib

def test_package_imports():
    """Test that package imports successfully."""
    import my_package
    assert hasattr(my_package, '__version__')

def test_package_all():
    """Test __all__ definition."""
    import my_package
    assert hasattr(my_package, '__all__')
    assert isinstance(my_package.__all__, list)

def test_package_functions_available():
    """Test that package functions are available."""
    import my_package
    for func_name in my_package.__all__:
        assert hasattr(my_package, func_name)

def test_package_reload():
    """Test package reloading."""
    import my_package
    original_version = my_package.__version__
    
    # Reload package
    importlib.reload(my_package)
    
    # Check that version is still the same
    assert my_package.__version__ == original_version

def test_package_initialization_idempotent():
    """Test that multiple imports don't cause issues."""
    import my_package
    first_import_time = my_package.get_init_info()['init_time']
    
    # Import again
    importlib.reload(my_package)
    second_import_time = my_package.get_init_info()['init_time']
    
    # Should be the same (cached)
    assert first_import_time == second_import_time
```
