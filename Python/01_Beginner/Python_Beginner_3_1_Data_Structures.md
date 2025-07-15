# Python Beginner 3.1: Built-in Data Structures

## Table of Contents
1. [Learning Objectives](#learning-objectives)
2. [Lists](#1-lists)
3. [Tuples](#2-tuples)
4. [Dictionaries](#3-dictionaries)
5. [Sets](#4-sets)
6. [Choosing the Right Data Structure](#5-choosing-the-right-data-structure)
7. [Data Structure Performance](#6-data-structure-performance)
8. [Advanced Operations](#7-advanced-operations)
9. [Common Patterns and Use Cases](#8-common-patterns-and-use-cases)
10. [Memory Management](#9-memory-management)
11. [Review Questions](#review-questions)
12. [Practical Exercises](#practical-exercises)
13. [Next Steps](#next-steps)
14. [Additional Resources](#additional-resources)

## Learning Objectives
By the end of this module, you will be able to:
- Master Python's four main built-in data structures: lists, tuples, dictionaries, and sets
- Understand when to use each data structure based on performance and use case requirements
- Perform complex operations including creation, modification, and manipulation
- Apply advanced techniques like list comprehensions and dictionary comprehensions
- Implement efficient algorithms using appropriate data structures
- Handle nested data structures and complex data relationships
- Choose optimal data structures for specific programming scenarios

## 1. Lists

### 1.1 List Basics

Lists are ordered, mutable collections that can contain duplicate elements of any type.

```python
# Creating lists
empty_list = []
numbers = [1, 2, 3, 4, 5]
mixed_list = [1, "hello", 3.14, True, None]
nested_list = [[1, 2], [3, 4], [5, 6]]

# List creation methods
from_range = list(range(1, 6))        # [1, 2, 3, 4, 5]
from_string = list("hello")           # ['h', 'e', 'l', 'l', 'o']
repeated = [0] * 5                    # [0, 0, 0, 0, 0]
```

### 1.2 List Indexing and Slicing

```python
fruits = ["apple", "banana", "cherry", "date", "elderberry"]

# Indexing
first_fruit = fruits[0]      # "apple"
last_fruit = fruits[-1]      # "elderberry"
second_last = fruits[-2]     # "date"

# Slicing
first_three = fruits[:3]     # ["apple", "banana", "cherry"]
last_two = fruits[-2:]       # ["date", "elderberry"]
middle = fruits[1:4]         # ["banana", "cherry", "date"]
every_second = fruits[::2]   # ["apple", "cherry", "elderberry"]
reversed_list = fruits[::-1] # ["elderberry", "date", "cherry", "banana", "apple"]

# Slice assignment
fruits[1:3] = ["kiwi", "mango"]
print(fruits)  # ["apple", "kiwi", "mango", "date", "elderberry"]
```

### 1.3 List Methods

```python
# Adding elements
fruits = ["apple", "banana"]
fruits.append("cherry")              # Add to end
fruits.insert(1, "kiwi")             # Insert at index 1
fruits.extend(["mango", "orange"])   # Add multiple elements
combined = fruits + ["grape"]        # Create new list

print(fruits)  # ["apple", "kiwi", "banana", "cherry", "mango", "orange"]

# Removing elements
fruits.remove("kiwi")          # Remove first occurrence
last_item = fruits.pop()       # Remove and return last item
second_item = fruits.pop(1)    # Remove and return item at index 1
del fruits[0]                  # Delete item at index 0
fruits.clear()                 # Remove all elements

# Searching and counting
numbers = [1, 2, 3, 2, 4, 2, 5]
index = numbers.index(3)       # Find index of first occurrence
count = numbers.count(2)       # Count occurrences
exists = 3 in numbers          # Check if element exists

# Sorting and reversing
numbers = [3, 1, 4, 1, 5, 9, 2]
numbers.sort()                 # Sort in place
numbers.sort(reverse=True)     # Sort in descending order
numbers.reverse()              # Reverse in place

# Non-destructive operations
sorted_numbers = sorted(numbers)           # Return new sorted list
reversed_numbers = list(reversed(numbers)) # Return new reversed list
```

### 1.4 List Comprehensions

```python
# Basic list comprehension
numbers = [1, 2, 3, 4, 5]
squares = [x**2 for x in numbers]
print(squares)  # [1, 4, 9, 16, 25]

# List comprehension with condition
even_squares = [x**2 for x in numbers if x % 2 == 0]
print(even_squares)  # [4, 16]

# Complex transformations
words = ["hello", "world", "python", "programming"]
capitalized = [word.upper() for word in words if len(word) > 4]
print(capitalized)  # ['HELLO', 'WORLD', 'PYTHON', 'PROGRAMMING']

# Nested list comprehension
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [item for row in matrix for item in row]
print(flattened)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# List comprehension with multiple conditions
numbers = range(1, 21)
filtered = [x for x in numbers if x % 2 == 0 and x % 3 == 0]
print(filtered)  # [6, 12, 18]
```

### 1.5 Advanced List Operations

```python
# List multiplication and copying
original = [1, 2, 3]
repeated = original * 3           # [1, 2, 3, 1, 2, 3, 1, 2, 3]
shallow_copy = original.copy()    # or original[:]
deep_copy = original[:]           # For nested lists, use copy.deepcopy()

# List unpacking
a, b, c = [1, 2, 3]
first, *middle, last = [1, 2, 3, 4, 5]  # first=1, middle=[2, 3, 4], last=5

# Enumerate and zip with lists
fruits = ["apple", "banana", "cherry"]
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")

colors = ["red", "yellow", "red"]
for fruit, color in zip(fruits, colors):
    print(f"{fruit} is {color}")

# List as stack (LIFO)
stack = []
stack.append("first")
stack.append("second")
stack.append("third")
top = stack.pop()  # "third"

# List as queue (FIFO) - not recommended, use collections.deque
from collections import deque
queue = deque()
queue.append("first")
queue.append("second")
first = queue.popleft()  # "first"
```

## 2. Tuples

### 2.1 Tuple Basics

Tuples are ordered, immutable collections that can contain duplicate elements.

```python
# Creating tuples
empty_tuple = ()
single_element = (42,)          # Note the comma!
coordinates = (10, 20)
person = ("Alice", 25, "Engineer")
nested_tuple = ((1, 2), (3, 4))

# Tuple creation methods
from_list = tuple([1, 2, 3, 4])
from_string = tuple("hello")
from_range = tuple(range(1, 6))
```

### 2.2 Tuple Operations

```python
# Accessing elements
point = (10, 20, 30)
x = point[0]        # 10
y = point[1]        # 20
z = point[-1]       # 30

# Slicing
numbers = (1, 2, 3, 4, 5)
first_three = numbers[:3]    # (1, 2, 3)
last_two = numbers[-2:]      # (4, 5)

# Tuple methods
numbers = (1, 2, 3, 2, 4, 2, 5)
count = numbers.count(2)     # 3
index = numbers.index(3)     # 2

# Membership testing
exists = 3 in numbers        # True
not_exists = 10 not in numbers  # True

# Tuple unpacking
person = ("Alice", 25, "Engineer")
name, age, job = person

# Multiple assignment
a, b = 1, 2  # Actually creates tuple (1, 2)
a, b = b, a  # Swap values using tuple unpacking
```

### 2.3 Named Tuples

```python
from collections import namedtuple

# Define a named tuple
Point = namedtuple('Point', ['x', 'y'])
Person = namedtuple('Person', ['name', 'age', 'job'])

# Create instances
origin = Point(0, 0)
point1 = Point(10, 20)
person1 = Person("Alice", 25, "Engineer")

# Access by name or index
print(point1.x)        # 10
print(point1[0])       # 10
print(person1.name)    # "Alice"
print(person1[0])      # "Alice"

# Named tuple methods
print(point1._fields)  # ('x', 'y')
point_dict = point1._asdict()  # OrderedDict([('x', 10), ('y', 20)])
new_point = point1._replace(x=15)  # Point(x=15, y=20)

# Using named tuples in functions
def calculate_distance(p1, p2):
    return ((p1.x - p2.x)**2 + (p1.y - p2.y)**2)**0.5

distance = calculate_distance(origin, point1)
print(f"Distance: {distance}")
```

### 2.4 Tuple Use Cases

```python
# Returning multiple values from functions
def get_name_parts(full_name):
    parts = full_name.split()
    return parts[0], parts[-1]  # Returns tuple

first, last = get_name_parts("John Doe")

# Dictionary keys (tuples are hashable)
coordinates = {
    (0, 0): "origin",
    (1, 0): "right",
    (0, 1): "up",
    (1, 1): "diagonal"
}

# Immutable configuration
CONFIG = (
    ("database_host", "localhost"),
    ("database_port", 5432),
    ("debug_mode", True)
)

# Tuple as record
student_records = [
    ("Alice", 85, "Math"),
    ("Bob", 92, "Science"),
    ("Charlie", 78, "History")
]

# Processing records
for name, grade, subject in student_records:
    print(f"{name} scored {grade} in {subject}")
```

## 3. Dictionaries

### 3.1 Dictionary Basics

Dictionaries are unordered (ordered in Python 3.7+), mutable collections of key-value pairs.

```python
# Creating dictionaries
empty_dict = {}
person = {"name": "Alice", "age": 25, "city": "New York"}
mixed_dict = {1: "one", "2": 2, (3, 4): "tuple key"}

# Dictionary creation methods
from_pairs = dict([("a", 1), ("b", 2), ("c", 3)])
from_kwargs = dict(name="Alice", age=25, city="New York")
from_keys = dict.fromkeys(["a", "b", "c"], 0)  # {"a": 0, "b": 0, "c": 0}

# Dictionary comprehension
squares = {x: x**2 for x in range(1, 6)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

### 3.2 Dictionary Operations

```python
# Accessing values
person = {"name": "Alice", "age": 25, "city": "New York"}
name = person["name"]           # "Alice"
age = person.get("age")         # 25
height = person.get("height", "Unknown")  # "Unknown" (default)

# Modifying dictionaries
person["age"] = 26              # Update existing key
person["job"] = "Engineer"      # Add new key
person.update({"salary": 75000, "age": 27})  # Update multiple

# Removing items
age = person.pop("age")         # Remove and return value
item = person.popitem()         # Remove and return arbitrary (key, value)
del person["city"]              # Delete key
person.clear()                  # Remove all items

# Dictionary methods
person = {"name": "Alice", "age": 25, "city": "New York"}
keys = person.keys()            # dict_keys(['name', 'age', 'city'])
values = person.values()        # dict_values(['Alice', 25, 'New York'])
items = person.items()          # dict_items([('name', 'Alice'), ('age', 25), ('city', 'New York')])
```

### 3.3 Dictionary Comprehensions

```python
# Basic dictionary comprehension
numbers = [1, 2, 3, 4, 5]
squares = {x: x**2 for x in numbers}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# Dictionary comprehension with condition
even_squares = {x: x**2 for x in numbers if x % 2 == 0}
print(even_squares)  # {2: 4, 4: 16}

# Transforming existing dictionary
original = {"a": 1, "b": 2, "c": 3}
doubled = {k: v * 2 for k, v in original.items()}
print(doubled)  # {"a": 2, "b": 4, "c": 6}

# Complex transformations
words = ["hello", "world", "python"]
word_info = {word: {"length": len(word), "uppercase": word.upper()} 
             for word in words}
print(word_info)
# {'hello': {'length': 5, 'uppercase': 'HELLO'}, 
#  'world': {'length': 5, 'uppercase': 'WORLD'}, 
#  'python': {'length': 6, 'uppercase': 'PYTHON'}}
```

### 3.4 Advanced Dictionary Operations

```python
# Nested dictionaries
students = {
    "Alice": {
        "grades": [85, 92, 78],
        "subjects": ["Math", "Science", "History"]
    },
    "Bob": {
        "grades": [90, 87, 95],
        "subjects": ["Math", "English", "Science"]
    }
}

# Accessing nested values
alice_grades = students["Alice"]["grades"]
bob_first_grade = students["Bob"]["grades"][0]

# Dictionary merging (Python 3.9+)
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
merged = dict1 | dict2  # {"a": 1, "b": 2, "c": 3, "d": 4}

# Dictionary unpacking
def greet(name, age, city):
    return f"Hello {name}, age {age} from {city}"

person = {"name": "Alice", "age": 25, "city": "New York"}
greeting = greet(**person)

# Default dictionaries
from collections import defaultdict

# Regular dictionary would raise KeyError
dd = defaultdict(list)
dd["fruits"].append("apple")
dd["fruits"].append("banana")
print(dd)  # defaultdict(<class 'list'>, {'fruits': ['apple', 'banana']})

# Counter for counting
from collections import Counter
text = "hello world"
char_count = Counter(text)
print(char_count)  # Counter({'l': 3, 'o': 2, 'h': 1, 'e': 1, ' ': 1, 'w': 1, 'r': 1, 'd': 1})
```

## 4. Sets

### 4.1 Set Basics

Sets are unordered collections of unique elements.

```python
# Creating sets
empty_set = set()  # Note: {} creates empty dict, not set
numbers = {1, 2, 3, 4, 5}
mixed_set = {1, "hello", 3.14, True}

# Set creation methods
from_list = set([1, 2, 3, 2, 4, 2, 5])  # {1, 2, 3, 4, 5}
from_string = set("hello")               # {'h', 'e', 'l', 'o'}
from_range = set(range(1, 6))            # {1, 2, 3, 4, 5}
```

### 4.2 Set Operations

```python
# Adding and removing elements
fruits = {"apple", "banana", "cherry"}
fruits.add("orange")                    # Add single element
fruits.update(["grape", "kiwi"])        # Add multiple elements
fruits.remove("banana")                 # Remove element (raises KeyError if not found)
fruits.discard("mango")                 # Remove element (no error if not found)
random_fruit = fruits.pop()             # Remove and return arbitrary element
fruits.clear()                          # Remove all elements

# Set operations
set1 = {1, 2, 3, 4, 5}
set2 = {4, 5, 6, 7, 8}

# Union (elements in either set)
union = set1 | set2                     # {1, 2, 3, 4, 5, 6, 7, 8}
union = set1.union(set2)                # Same as above

# Intersection (elements in both sets)
intersection = set1 & set2              # {4, 5}
intersection = set1.intersection(set2)  # Same as above

# Difference (elements in first set but not second)
difference = set1 - set2                # {1, 2, 3}
difference = set1.difference(set2)      # Same as above

# Symmetric difference (elements in either set but not both)
sym_diff = set1 ^ set2                  # {1, 2, 3, 6, 7, 8}
sym_diff = set1.symmetric_difference(set2)  # Same as above
```

### 4.3 Set Relationships

```python
# Subset and superset relationships
set1 = {1, 2, 3}
set2 = {1, 2, 3, 4, 5}

is_subset = set1 <= set2                # True
is_subset = set1.issubset(set2)         # True

is_superset = set2 >= set1              # True
is_superset = set2.issuperset(set1)     # True

# Proper subset/superset
is_proper_subset = set1 < set2          # True
is_proper_superset = set2 > set1        # True

# Disjoint sets (no common elements)
set3 = {6, 7, 8}
are_disjoint = set1.isdisjoint(set3)    # True
```

### 4.4 Set Comprehensions

```python
# Basic set comprehension
numbers = [1, 2, 3, 4, 5, 2, 3, 4]
squares = {x**2 for x in numbers}
print(squares)  # {1, 4, 9, 16, 25}

# Set comprehension with condition
even_squares = {x**2 for x in numbers if x % 2 == 0}
print(even_squares)  # {4, 16}

# Complex set operations
words = ["hello", "world", "python", "hello"]
unique_lengths = {len(word) for word in words}
print(unique_lengths)  # {5, 6}

# Set of first letters
first_letters = {word[0].upper() for word in words}
print(first_letters)  # {'H', 'W', 'P'}
```

### 4.5 Frozen Sets

```python
# Frozen sets are immutable sets
regular_set = {1, 2, 3}
frozen_set = frozenset([1, 2, 3])

# Frozen sets can be dictionary keys
nested_sets = {
    frozenset([1, 2]): "first",
    frozenset([3, 4]): "second"
}

# Frozen sets can be elements of other sets
set_of_sets = {frozenset([1, 2]), frozenset([3, 4])}

# Operations work the same
fs1 = frozenset([1, 2, 3])
fs2 = frozenset([3, 4, 5])
union = fs1 | fs2  # frozenset({1, 2, 3, 4, 5})
```

## 5. Choosing the Right Data Structure

### 5.1 Decision Matrix

```python
# Use cases for each data structure

# LIST - Use when:
# - Order matters
# - Need to modify elements
# - Allow duplicates
# - Need indexing
shopping_list = ["milk", "bread", "eggs", "milk"]  # Duplicates allowed
shopping_list.append("cheese")  # Can modify
first_item = shopping_list[0]   # Indexing available

# TUPLE - Use when:
# - Order matters
# - Data shouldn't change
# - Need hashable collection
# - Returning multiple values
coordinates = (10, 20)  # Immutable
point_dict = {coordinates: "location"}  # Hashable

# DICTIONARY - Use when:
# - Need key-value mapping
# - Fast lookups by key
# - Associative data
user_data = {"name": "Alice", "age": 25}  # Key-value mapping
age = user_data["age"]  # Fast lookup

# SET - Use when:
# - Need unique elements
# - Set operations (union, intersection)
# - Membership testing
unique_ids = {1, 2, 3, 4, 5}  # No duplicates
has_id = 3 in unique_ids  # Fast membership testing
```

### 5.2 Performance Comparison

```python
import time

# Performance comparison function
def time_operation(operation, description):
    start = time.time()
    operation()
    end = time.time()
    print(f"{description}: {end - start:.6f} seconds")

# Test data
data = list(range(100000))

# List operations
def list_operations():
    test_list = data.copy()
    test_list.append(100001)
    test_list.insert(0, -1)
    test_list.remove(-1)

# Set operations
def set_operations():
    test_set = set(data)
    test_set.add(100001)
    test_set.discard(100001)

# Dictionary operations
def dict_operations():
    test_dict = {i: i**2 for i in range(1000)}
    test_dict[1001] = 1001**2
    del test_dict[1001]

# Compare performance
time_operation(list_operations, "List operations")
time_operation(set_operations, "Set operations")
time_operation(dict_operations, "Dictionary operations")
```

## 6. Data Structure Performance

### 6.1 Time Complexity

```python
# Time complexity of common operations

# LIST
# Access by index: O(1)
# Search by value: O(n)
# Insert at beginning: O(n)
# Insert at end: O(1) amortized
# Delete by index: O(n)

# TUPLE
# Access by index: O(1)
# Search by value: O(n)
# Creation: O(n)

# DICTIONARY
# Access by key: O(1) average
# Insert/Update: O(1) average
# Delete by key: O(1) average
# Search by key: O(1) average

# SET
# Add element: O(1) average
# Remove element: O(1) average
# Membership test: O(1) average
# Union/Intersection: O(len(s1) + len(s2))

# Demonstration
import time

def test_list_search(data, target):
    start = time.time()
    found = target in data
    return time.time() - start

def test_set_search(data, target):
    start = time.time()
    found = target in data
    return time.time() - start

# Create test data
large_list = list(range(100000))
large_set = set(large_list)
target = 99999

list_time = test_list_search(large_list, target)
set_time = test_set_search(large_set, target)

print(f"List search time: {list_time:.6f} seconds")
print(f"Set search time: {set_time:.6f} seconds")
print(f"Set is {list_time/set_time:.0f}x faster")
```

### 6.2 Space Complexity

```python
import sys

# Memory usage comparison
def compare_memory_usage():
    # Create data structures with same elements
    data = list(range(1000))
    
    test_list = data.copy()
    test_tuple = tuple(data)
    test_set = set(data)
    test_dict = {i: i for i in data}
    
    # Compare memory usage
    print(f"List memory: {sys.getsizeof(test_list)} bytes")
    print(f"Tuple memory: {sys.getsizeof(test_tuple)} bytes")
    print(f"Set memory: {sys.getsizeof(test_set)} bytes")
    print(f"Dict memory: {sys.getsizeof(test_dict)} bytes")

compare_memory_usage()
```

## 7. Advanced Operations

### 7.1 Nested Data Structures

```python
# Complex nested structure
company_data = {
    "departments": {
        "engineering": {
            "employees": [
                {"name": "Alice", "skills": ["Python", "JavaScript"]},
                {"name": "Bob", "skills": ["Java", "C++"]}
            ],
            "budget": 500000
        },
        "marketing": {
            "employees": [
                {"name": "Charlie", "skills": ["SEO", "Content"]},
                {"name": "Diana", "skills": ["Design", "Analytics"]}
            ],
            "budget": 200000
        }
    }
}

# Accessing nested data
alice_skills = company_data["departments"]["engineering"]["employees"][0]["skills"]
print(alice_skills)  # ["Python", "JavaScript"]

# Modifying nested data
company_data["departments"]["engineering"]["employees"][0]["skills"].append("React")

# Iterating through nested structures
for dept_name, dept_data in company_data["departments"].items():
    print(f"\nDepartment: {dept_name}")
    print(f"Budget: ${dept_data['budget']:,}")
    print("Employees:")
    for employee in dept_data["employees"]:
        print(f"  - {employee['name']}: {', '.join(employee['skills'])}")
```

### 7.2 Data Structure Conversions

```python
# Converting between data structures

# List to other structures
numbers = [1, 2, 3, 2, 4, 2, 5]
unique_numbers = list(set(numbers))      # Remove duplicates
tuple_numbers = tuple(numbers)           # Convert to tuple
dict_numbers = {i: i**2 for i in set(numbers)}  # Create dict

# String to data structures
text = "hello world"
char_list = list(text)                   # ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']
unique_chars = set(text)                 # {'h', 'e', 'l', 'o', ' ', 'w', 'r', 'd'}
char_positions = {char: i for i, char in enumerate(text)}

# CSV-like data processing
csv_data = [
    ["Name", "Age", "City"],
    ["Alice", "25", "New York"],
    ["Bob", "30", "London"],
    ["Charlie", "35", "Paris"]
]

# Convert to list of dictionaries
headers = csv_data[0]
records = [dict(zip(headers, row)) for row in csv_data[1:]]
print(records)
# [{'Name': 'Alice', 'Age': '25', 'City': 'New York'}, 
#  {'Name': 'Bob', 'Age': '30', 'City': 'London'}, 
#  {'Name': 'Charlie', 'Age': '35', 'City': 'Paris'}]
```

### 7.3 Advanced List Techniques

```python
# Matrix operations
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Transpose matrix
transposed = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
print(transposed)  # [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

# Flatten nested lists
nested = [[1, 2, [3, 4]], [5, 6], [7, [8, 9]]]
def flatten(lst):
    result = []
    for item in lst:
        if isinstance(item, list):
            result.extend(flatten(item))
        else:
            result.append(item)
    return result

flattened = flatten(nested)
print(flattened)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Sliding window
def sliding_window(lst, window_size):
    return [lst[i:i+window_size] for i in range(len(lst) - window_size + 1)]

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
windows = sliding_window(numbers, 3)
print(windows)  # [[1, 2, 3], [2, 3, 4], [3, 4, 5], ...]
```

## 8. Common Patterns and Use Cases

### 8.1 Data Processing Patterns

```python
# Group by pattern
from collections import defaultdict

# Group students by grade
students = [
    {"name": "Alice", "grade": "A", "subject": "Math"},
    {"name": "Bob", "grade": "B", "subject": "Math"},
    {"name": "Charlie", "grade": "A", "subject": "Science"},
    {"name": "David", "grade": "B", "subject": "Science"}
]

# Group by grade
by_grade = defaultdict(list)
for student in students:
    by_grade[student["grade"]].append(student)

print(dict(by_grade))

# Frequency counting
def count_words(text):
    words = text.lower().split()
    word_count = {}
    for word in words:
        word_count[word] = word_count.get(word, 0) + 1
    return word_count

text = "hello world hello python world"
frequencies = count_words(text)
print(frequencies)  # {'hello': 2, 'world': 2, 'python': 1}
```

### 8.2 Caching and Memoization

```python
# Simple cache using dictionary
cache = {}

def expensive_function(n):
    if n in cache:
        return cache[n]
    
    # Simulate expensive computation
    result = sum(i**2 for i in range(n))
    cache[n] = result
    return result

# LRU Cache pattern
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = OrderedDict()
    
    def get(self, key):
        if key in self.cache:
            # Move to end (most recently used)
            self.cache.move_to_end(key)
            return self.cache[key]
        return None
    
    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        
        if len(self.cache) > self.capacity:
            # Remove least recently used
            self.cache.popitem(last=False)

# Usage
lru = LRUCache(3)
lru.put("a", 1)
lru.put("b", 2)
lru.put("c", 3)
print(lru.get("a"))  # 1
lru.put("d", 4)      # Removes "b"
```

### 8.3 Data Validation and Cleaning

```python
# Data validation using sets
def validate_data(records, required_fields, valid_values=None):
    required_set = set(required_fields)
    errors = []
    
    for i, record in enumerate(records):
        record_fields = set(record.keys())
        
        # Check for missing fields
        missing = required_set - record_fields
        if missing:
            errors.append(f"Record {i}: Missing fields {missing}")
        
        # Check for invalid values
        if valid_values:
            for field, valid_set in valid_values.items():
                if field in record and record[field] not in valid_set:
                    errors.append(f"Record {i}: Invalid {field} value")
    
    return errors

# Example usage
records = [
    {"name": "Alice", "age": 25, "status": "active"},
    {"name": "Bob", "age": 30},  # Missing status
    {"name": "Charlie", "age": 35, "status": "inactive"}
]

required_fields = ["name", "age", "status"]
valid_values = {"status": {"active", "inactive", "pending"}}

errors = validate_data(records, required_fields, valid_values)
for error in errors:
    print(error)
```

## 9. Memory Management

### 9.1 Memory Optimization

```python
# Memory-efficient data structures
import sys
from array import array

# Regular list vs array for numeric data
regular_list = [i for i in range(1000)]
int_array = array('i', range(1000))

print(f"List memory: {sys.getsizeof(regular_list)} bytes")
print(f"Array memory: {sys.getsizeof(int_array)} bytes")

# Using __slots__ for memory-efficient classes
class RegularPerson:
    def __init__(self, name, age):
        self.name = name
        self.age = age

class OptimizedPerson:
    __slots__ = ['name', 'age']
    
    def __init__(self, name, age):
        self.name = name
        self.age = age

# Memory comparison
regular = RegularPerson("Alice", 25)
optimized = OptimizedPerson("Alice", 25)

print(f"Regular object: {sys.getsizeof(regular)} bytes")
print(f"Optimized object: {sys.getsizeof(optimized)} bytes")
```

### 9.2 Garbage Collection

```python
import gc
import weakref

# Weak references to avoid circular references
class Node:
    def __init__(self, value):
        self.value = value
        self.children = []
        self.parent = None  # This could create circular references
    
    def add_child(self, child):
        self.children.append(child)
        child.parent = weakref.ref(self)  # Weak reference

# Monitor garbage collection
def monitor_gc():
    print(f"Garbage collection counts: {gc.get_count()}")
    collected = gc.collect()
    print(f"Collected {collected} objects")

# Force garbage collection
monitor_gc()
```

## Review Questions

### Theoretical Questions

1. **Explain the key differences between lists and tuples in Python.**

**Answer:**
- **Mutability**: Lists are mutable (can be modified), tuples are immutable (cannot be modified)
- **Performance**: Tuples are faster for access and iteration
- **Memory**: Tuples use less memory than lists
- **Use cases**: Lists for data that changes, tuples for fixed data
- **Hashability**: Tuples are hashable (can be dict keys), lists are not

```python
# List - mutable
my_list = [1, 2, 3]
my_list.append(4)  # Works

# Tuple - immutable
my_tuple = (1, 2, 3)
# my_tuple.append(4)  # Error: AttributeError

# Tuple as dictionary key
locations = {(0, 0): "origin", (1, 1): "diagonal"}
```

2. **When should you use a set versus a list for storing data?**

**Answer:**
Use **sets** when:
- You need unique elements only
- Fast membership testing is important
- You need set operations (union, intersection)
- Order doesn't matter

Use **lists** when:
- You need to maintain order
- Duplicates are allowed/needed
- You need indexing
- You need to modify elements by position

```python
# Set for unique elements and fast lookup
unique_ids = {1, 2, 3, 4, 5}
if user_id in unique_ids:  # O(1) operation
    print("Valid user")

# List for ordered, indexed data
task_queue = ["task1", "task2", "task3"]
next_task = task_queue[0]  # Need indexing
```

3. **What are the time complexities of common operations on Python dictionaries?**

**Answer:**
- **Access by key**: O(1) average, O(n) worst case
- **Insert/Update**: O(1) average, O(n) worst case
- **Delete by key**: O(1) average, O(n) worst case
- **Search by key**: O(1) average, O(n) worst case
- **Iteration**: O(n)

The worst-case scenarios occur when there are many hash collisions, but Python's hash table implementation makes this rare in practice.

### Practical Questions

4. **Write a function that merges two dictionaries, handling conflicts by keeping the maximum value.**

**Answer:**
```python
def merge_dicts_max(dict1, dict2):
    """Merge two dictionaries, keeping maximum values for conflicts."""
    result = dict1.copy()
    
    for key, value in dict2.items():
        if key in result:
            result[key] = max(result[key], value)
        else:
            result[key] = value
    
    return result

# Usage
scores1 = {"Alice": 85, "Bob": 90, "Charlie": 78}
scores2 = {"Alice": 92, "Bob": 88, "David": 95}

merged = merge_dicts_max(scores1, scores2)
print(merged)  # {"Alice": 92, "Bob": 90, "Charlie": 78, "David": 95}
```

5. **Create a function that finds the intersection of multiple lists.**

**Answer:**
```python
def find_intersection(*lists):
    """Find intersection of multiple lists."""
    if not lists:
        return []
    
    # Convert first list to set for efficient operations
    intersection = set(lists[0])
    
    # Find intersection with all other lists
    for lst in lists[1:]:
        intersection &= set(lst)
    
    return list(intersection)

def find_intersection_preserve_order(*lists):
    """Find intersection while preserving order from first list."""
    if not lists:
        return []
    
    # Create sets for efficient lookup
    sets = [set(lst) for lst in lists[1:]]
    
    # Keep elements from first list that exist in all others
    result = []
    for item in lists[0]:
        if item not in result and all(item in s for s in sets):
            result.append(item)
    
    return result

# Usage
list1 = [1, 2, 3, 4, 5]
list2 = [3, 4, 5, 6, 7]
list3 = [4, 5, 6, 7, 8]

intersection = find_intersection(list1, list2, list3)
print(intersection)  # [4, 5]
```

### Advanced Questions

6. **Implement a data structure that maintains the top K elements efficiently.**

**Answer:**
```python
import heapq

class TopKTracker:
    """Efficiently track top K elements using a min-heap."""
    
    def __init__(self, k):
        self.k = k
        self.heap = []
    
    def add(self, value):
        """Add a value to the tracker."""
        if len(self.heap) < self.k:
            heapq.heappush(self.heap, value)
        elif value > self.heap[0]:
            heapq.heapreplace(self.heap, value)
    
    def get_top_k(self):
        """Get the top K elements in descending order."""
        return sorted(self.heap, reverse=True)
    
    def get_kth_largest(self):
        """Get the Kth largest element."""
        return self.heap[0] if self.heap else None

# Usage
tracker = TopKTracker(3)
for value in [1, 5, 3, 8, 2, 9, 4, 7, 6]:
    tracker.add(value)

print(tracker.get_top_k())  # [9, 8, 7]
print(tracker.get_kth_largest())  # 7
```

7. **Design a caching system with TTL (Time To Live) functionality.**

**Answer:**
```python
import time
from collections import OrderedDict

class TTLCache:
    """Cache with Time To Live functionality."""
    
    def __init__(self, max_size=100, default_ttl=300):
        self.max_size = max_size
        self.default_ttl = default_ttl
        self.cache = OrderedDict()
        self.timestamps = {}
    
    def _is_expired(self, key):
        """Check if a key has expired."""
        if key not in self.timestamps:
            return True
        return time.time() - self.timestamps[key] > self.default_ttl
    
    def _cleanup_expired(self):
        """Remove expired entries."""
        current_time = time.time()
        expired_keys = [
            key for key, timestamp in self.timestamps.items()
            if current_time - timestamp > self.default_ttl
        ]
        
        for key in expired_keys:
            self.cache.pop(key, None)
            self.timestamps.pop(key, None)
    
    def get(self, key):
        """Get value from cache."""
        self._cleanup_expired()
        
        if key in self.cache and not self._is_expired(key):
            # Move to end (LRU)
            self.cache.move_to_end(key)
            return self.cache[key]
        
        return None
    
    def set(self, key, value, ttl=None):
        """Set value in cache with optional TTL."""
        self._cleanup_expired()
        
        if key in self.cache:
            self.cache.move_to_end(key)
        
        self.cache[key] = value
        self.timestamps[key] = time.time()
        
        # Remove oldest if over capacity
        while len(self.cache) > self.max_size:
            oldest_key = next(iter(self.cache))
            self.cache.pop(oldest_key)
            self.timestamps.pop(oldest_key)

# Usage
cache = TTLCache(max_size=3, default_ttl=2)
cache.set("key1", "value1")
cache.set("key2", "value2")

print(cache.get("key1"))  # "value1"
time.sleep(3)
print(cache.get("key1"))  # None (expired)
```

8. **Implement a graph data structure using dictionaries and sets.**

**Answer:**
```python
class Graph:
    """Graph implementation using adjacency lists."""
    
    def __init__(self, directed=False):
        self.directed = directed
        self.adjacency_list = {}
    
    def add_vertex(self, vertex):
        """Add a vertex to the graph."""
        if vertex not in self.adjacency_list:
            self.adjacency_list[vertex] = set()
    
    def add_edge(self, vertex1, vertex2):
        """Add an edge between two vertices."""
        self.add_vertex(vertex1)
        self.add_vertex(vertex2)
        
        self.adjacency_list[vertex1].add(vertex2)
        if not self.directed:
            self.adjacency_list[vertex2].add(vertex1)
    
    def remove_edge(self, vertex1, vertex2):
        """Remove an edge between two vertices."""
        if vertex1 in self.adjacency_list:
            self.adjacency_list[vertex1].discard(vertex2)
        if not self.directed and vertex2 in self.adjacency_list:
            self.adjacency_list[vertex2].discard(vertex1)
    
    def get_neighbors(self, vertex):
        """Get neighbors of a vertex."""
        return self.adjacency_list.get(vertex, set())
    
    def bfs(self, start):
        """Breadth-first search."""
        if start not in self.adjacency_list:
            return []
        
        visited = set()
        queue = [start]
        result = []
        
        while queue:
            vertex = queue.pop(0)
            if vertex not in visited:
                visited.add(vertex)
                result.append(vertex)
                
                for neighbor in self.adjacency_list[vertex]:
                    if neighbor not in visited:
                        queue.append(neighbor)
        
        return result
    
    def dfs(self, start):
        """Depth-first search."""
        if start not in self.adjacency_list:
            return []
        
        visited = set()
        stack = [start]
        result = []
        
        while stack:
            vertex = stack.pop()
            if vertex not in visited:
                visited.add(vertex)
                result.append(vertex)
                
                for neighbor in self.adjacency_list[vertex]:
                    if neighbor not in visited:
                        stack.append(neighbor)
        
        return result

# Usage
graph = Graph()
graph.add_edge("A", "B")
graph.add_edge("A", "C")
graph.add_edge("B", "D")
graph.add_edge("C", "D")

print(graph.bfs("A"))  # ['A', 'B', 'C', 'D']
print(graph.dfs("A"))  # ['A', 'C', 'D', 'B']
```

## Practical Exercises

### Exercise 1: Data Structure Performance Analysis
Create a comprehensive performance analysis comparing operations on different data structures for various use cases.

### Exercise 2: Text Analysis System
Build a text analysis system that uses all four data structures to count words, find unique words, create word indices, and generate statistics.

### Exercise 3: Social Network Graph
Implement a social network system using appropriate data structures to store users, friendships, and posts.

### Exercise 4: Inventory Management System
Create an inventory management system that efficiently handles products, categories, suppliers, and stock levels.

---

## Next Steps

In the next module, we'll explore string manipulation and regular expressions, building on your understanding of data structures to process and analyze text data effectively.

## Additional Resources

- [Python Data Structures Documentation](https://docs.python.org/3/tutorial/datastructures.html)
- [Python Collections Module](https://docs.python.org/3/library/collections.html)
- [Time Complexity of Python Operations](https://wiki.python.org/moin/TimeComplexity)
- [Python Memory Management](https://docs.python.org/3/c-api/memory.html)
- [Effective Python: Lists and Tuples](https://effectivepython.com/)

---

*Data structures are the foundation of efficient programming. Choose the right structure for your specific use case to write performant and readable code.*
