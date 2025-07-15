# Object-Oriented Programming Fundamentals

## Table of Contents
- [Introduction to OOP](#introduction-to-oop)
- [Classes and Objects](#classes-and-objects)
- [Attributes and Methods](#attributes-and-methods)
- [Constructor and Destructor](#constructor-and-destructor)
- [Instance vs Class Variables](#instance-vs-class-variables)
- [Practical Examples](#practical-examples)
- [Exercises](#exercises)

---

## Introduction to OOP

Object-Oriented Programming (OOP) is a programming paradigm that organizes code around objects rather than functions and logic. It's based on the concept of "objects" which contain data (attributes) and code (methods).

### Why Use OOP?

1. **Code Organization**: Groups related data and functions together
2. **Reusability**: Write once, use many times
3. **Maintainability**: Easier to modify and extend
4. **Modularity**: Break complex problems into smaller, manageable pieces
5. **Real-world Modeling**: Maps closely to how we think about real-world entities

### Key Concepts

- **Class**: A blueprint or template for creating objects
- **Object**: An instance of a class
- **Attribute**: Data stored in a class or instance
- **Method**: Functions defined inside a class
- **Instance**: A specific occurrence of a class

---

## Classes and Objects

### Defining a Class

A class is defined using the `class` keyword followed by the class name (conventionally in PascalCase).

```python
class Dog:
    pass  # Empty class for now
```

### Creating Objects (Instances)

```python
# Create instances of the Dog class
my_dog = Dog()
your_dog = Dog()

print(type(my_dog))  # <class '__main__.Dog'>
print(isinstance(my_dog, Dog))  # True
```

### Basic Class with Attributes and Methods

```python
class Dog:
    def __init__(self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age
    
    def bark(self):
        return f"{self.name} says Woof!"
    
    def get_info(self):
        return f"{self.name} is a {self.age}-year-old {self.breed}"

# Creating objects
buddy = Dog("Buddy", "Golden Retriever", 3)
max_dog = Dog("Max", "German Shepherd", 5)

# Using methods
print(buddy.bark())  # Buddy says Woof!
print(max_dog.get_info())  # Max is a 5-year-old German Shepherd
```

### Class Naming Conventions

```python
# Good class names (PascalCase)
class BankAccount:
    pass

class CarEngine:
    pass

class StudentRecord:
    pass

# Avoid these naming patterns
class bank_account:  # snake_case is for functions/variables
    pass

class carengine:  # No separation of words
    pass
```

---

## Attributes and Methods

### Instance Attributes

Instance attributes are unique to each object and are defined in the `__init__` method or assigned later.

```python
class Student:
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
        self.grades = []  # Initialize empty list
        self.gpa = 0.0
    
    def add_grade(self, grade):
        if 0 <= grade <= 100:
            self.grades.append(grade)
            self.calculate_gpa()
        else:
            print("Grade must be between 0 and 100")
    
    def calculate_gpa(self):
        if self.grades:
            self.gpa = sum(self.grades) / len(self.grades)
        else:
            self.gpa = 0.0
    
    def get_grade_summary(self):
        return {
            'name': self.name,
            'grades': self.grades,
            'gpa': round(self.gpa, 2),
            'grade_count': len(self.grades)
        }

# Usage
alice = Student("Alice Johnson", "STU001")
alice.add_grade(85)
alice.add_grade(92)
alice.add_grade(78)

print(alice.get_grade_summary())
# {'name': 'Alice Johnson', 'grades': [85, 92, 78], 'gpa': 85.0, 'grade_count': 3}
```

### Instance Methods

Instance methods are functions defined inside a class that operate on instance data.

```python
class BankAccount:
    def __init__(self, account_number, account_holder, initial_balance=0):
        self.account_number = account_number
        self.account_holder = account_holder
        self.balance = initial_balance
        self.transaction_history = []
    
    def deposit(self, amount):
        """Add money to the account"""
        if amount > 0:
            self.balance += amount
            self.transaction_history.append(f"Deposited: ${amount}")
            return f"Deposited ${amount}. New balance: ${self.balance}"
        else:
            return "Deposit amount must be positive"
    
    def withdraw(self, amount):
        """Remove money from the account"""
        if amount > 0:
            if amount <= self.balance:
                self.balance -= amount
                self.transaction_history.append(f"Withdrew: ${amount}")
                return f"Withdrew ${amount}. New balance: ${self.balance}"
            else:
                return "Insufficient funds"
        else:
            return "Withdrawal amount must be positive"
    
    def get_balance(self):
        """Return current balance"""
        return self.balance
    
    def get_transaction_history(self):
        """Return list of all transactions"""
        return self.transaction_history.copy()
    
    def transfer(self, other_account, amount):
        """Transfer money to another account"""
        if amount > 0 and amount <= self.balance:
            self.balance -= amount
            other_account.balance += amount
            self.transaction_history.append(f"Transferred ${amount} to {other_account.account_number}")
            other_account.transaction_history.append(f"Received ${amount} from {self.account_number}")
            return f"Transferred ${amount} to account {other_account.account_number}"
        else:
            return "Transfer failed: Invalid amount or insufficient funds"

# Usage
account1 = BankAccount("ACC001", "John Doe", 1000)
account2 = BankAccount("ACC002", "Jane Smith", 500)

print(account1.deposit(200))  # Deposited $200. New balance: $1200
print(account1.withdraw(150))  # Withdrew $150. New balance: $1050
print(account1.transfer(account2, 300))  # Transferred $300 to account ACC002

print(f"Account 1 balance: ${account1.get_balance()}")  # Account 1 balance: $750
print(f"Account 2 balance: ${account2.get_balance()}")  # Account 2 balance: $800
```

### Method Parameters and Return Values

```python
class Calculator:
    def __init__(self):
        self.history = []
    
    def add(self, a, b):
        result = a + b
        self.history.append(f"{a} + {b} = {result}")
        return result
    
    def subtract(self, a, b):
        result = a - b
        self.history.append(f"{a} - {b} = {result}")
        return result
    
    def multiply(self, a, b):
        result = a * b
        self.history.append(f"{a} * {b} = {result}")
        return result
    
    def divide(self, a, b):
        if b != 0:
            result = a / b
            self.history.append(f"{a} / {b} = {result}")
            return result
        else:
            self.history.append(f"Error: Division by zero ({a} / {b})")
            return None
    
    def get_history(self):
        return self.history
    
    def clear_history(self):
        self.history = []

# Usage
calc = Calculator()
result1 = calc.add(10, 5)
result2 = calc.multiply(result1, 3)
result3 = calc.divide(result2, 2)

print(f"Final result: {result3}")
print("Calculation history:")
for operation in calc.get_history():
    print(f"  {operation}")
```

---

## Constructor and Destructor

### Constructor (`__init__`)

The constructor is a special method that gets called when an object is created. It initializes the object's attributes.

```python
class Car:
    def __init__(self, make, model, year, mileage=0):
        # Required parameters
        self.make = make
        self.model = model
        self.year = year
        
        # Optional parameter with default value
        self.mileage = mileage
        
        # Derived/computed attributes
        self.is_running = False
        self.fuel_level = 100  # Start with full tank
        
        # Validation
        if year < 1900 or year > 2030:
            raise ValueError("Invalid year")
        if mileage < 0:
            raise ValueError("Mileage cannot be negative")
    
    def start(self):
        if not self.is_running:
            self.is_running = True
            return f"The {self.year} {self.make} {self.model} is now running"
        return "Car is already running"
    
    def stop(self):
        if self.is_running:
            self.is_running = False
            return f"The {self.year} {self.make} {self.model} has stopped"
        return "Car is already stopped"
    
    def drive(self, miles):
        if self.is_running and self.fuel_level > 0:
            self.mileage += miles
            self.fuel_level -= miles * 0.05  # Simplified fuel consumption
            self.fuel_level = max(0, self.fuel_level)  # Don't go below 0
            return f"Drove {miles} miles. Total mileage: {self.mileage}"
        return "Cannot drive: Car not running or no fuel"

# Creating objects with constructor
car1 = Car("Toyota", "Camry", 2020)
car2 = Car("Honda", "Civic", 2019, 15000)

print(car1.start())
print(car1.drive(50))
print(f"Car 1 fuel level: {car1.fuel_level}%")
```

### Constructor with Different Parameter Patterns

```python
class Rectangle:
    def __init__(self, width, height=None):
        """
        Create a rectangle. If height is not provided, create a square.
        """
        self.width = width
        self.height = height if height is not None else width
        
        # Validate parameters
        if self.width <= 0 or self.height <= 0:
            raise ValueError("Width and height must be positive")
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
    
    def is_square(self):
        return self.width == self.height
    
    def __str__(self):
        shape_type = "Square" if self.is_square() else "Rectangle"
        return f"{shape_type}({self.width}x{self.height})"

# Different ways to create rectangles
rect1 = Rectangle(5, 3)    # Rectangle 5x3
rect2 = Rectangle(4)       # Square 4x4
rect3 = Rectangle(6, 6)    # Rectangle 6x6 (happens to be square)

print(rect1)  # Rectangle(5x3)
print(rect2)  # Square(4x4)
print(rect3)  # Square(6x6)
```

### Destructor (`__del__`)

The destructor is called when an object is about to be destroyed (garbage collected).

```python
class FileManager:
    def __init__(self, filename):
        self.filename = filename
        self.file = None
        self.is_open = False
    
    def open_file(self, mode='r'):
        try:
            self.file = open(self.filename, mode)
            self.is_open = True
            return f"File '{self.filename}' opened successfully"
        except IOError as e:
            return f"Error opening file: {e}"
    
    def close_file(self):
        if self.file and self.is_open:
            self.file.close()
            self.is_open = False
            return f"File '{self.filename}' closed"
        return "No file to close"
    
    def __del__(self):
        """Destructor - ensure file is closed when object is destroyed"""
        if self.file and self.is_open:
            print(f"Warning: Auto-closing file '{self.filename}'")
            self.file.close()

# Usage
file_mgr = FileManager("test.txt")
file_mgr.open_file('w')
# file_mgr.close_file()  # If we forget to close...
# When file_mgr goes out of scope, destructor will be called
```

---

## Instance vs Class Variables

### Instance Variables

Instance variables are unique to each instance of a class. They are defined in `__init__` or assigned to `self`.

```python
class Employee:
    def __init__(self, name, employee_id, salary):
        # These are instance variables
        self.name = name
        self.employee_id = employee_id
        self.salary = salary
        self.benefits = []
    
    def add_benefit(self, benefit):
        self.benefits.append(benefit)
    
    def get_info(self):
        return f"Employee: {self.name} (ID: {self.employee_id}), Salary: ${self.salary}"

# Each instance has its own copy of instance variables
emp1 = Employee("Alice", "E001", 50000)
emp2 = Employee("Bob", "E002", 60000)

emp1.add_benefit("Health Insurance")
emp2.add_benefit("Dental Insurance")

print(emp1.benefits)  # ['Health Insurance']
print(emp2.benefits)  # ['Dental Insurance']
```

### Class Variables

Class variables are shared among all instances of a class. They are defined directly in the class body.

```python
class Employee:
    # Class variables - shared by all instances
    company_name = "Tech Corp"
    total_employees = 0
    benefits_package = ["Health Insurance", "Retirement Plan"]
    
    def __init__(self, name, employee_id, salary):
        # Instance variables - unique to each instance
        self.name = name
        self.employee_id = employee_id
        self.salary = salary
        
        # Modify class variable when new employee is created
        Employee.total_employees += 1
    
    def get_info(self):
        return f"{self.name} works at {Employee.company_name}"
    
    @classmethod
    def get_company_info(cls):
        return f"Company: {cls.company_name}, Total Employees: {cls.total_employees}"
    
    @classmethod
    def change_company_name(cls, new_name):
        cls.company_name = new_name

# Usage
emp1 = Employee("Alice", "E001", 50000)
emp2 = Employee("Bob", "E002", 60000)
emp3 = Employee("Charlie", "E003", 55000)

print(Employee.get_company_info())  # Company: Tech Corp, Total Employees: 3
print(emp1.get_info())  # Alice works at Tech Corp
print(emp2.get_info())  # Bob works at Tech Corp

# Change company name for all employees
Employee.change_company_name("Innovation Inc")
print(emp1.get_info())  # Alice works at Innovation Inc
print(emp3.get_info())  # Charlie works at Innovation Inc
```

### Accessing Class Variables

```python
class Counter:
    count = 0  # Class variable
    
    def __init__(self, name):
        self.name = name  # Instance variable
        Counter.count += 1  # Increment class variable
    
    def get_count(self):
        return f"{self.name}: Total counters created = {Counter.count}"
    
    def reset_count(self):
        Counter.count = 0

# Creating instances
c1 = Counter("Counter 1")
c2 = Counter("Counter 2")
c3 = Counter("Counter 3")

print(c1.get_count())  # Counter 1: Total counters created = 3
print(c2.get_count())  # Counter 2: Total counters created = 3

# Accessing class variable directly
print(Counter.count)  # 3

# Reset count
c1.reset_count()
print(Counter.count)  # 0
```

### Important Notes about Class vs Instance Variables

```python
class Example:
    class_var = "I'm a class variable"
    
    def __init__(self, value):
        self.instance_var = value
    
    def modify_variables(self, new_class_val, new_instance_val):
        # Modifying class variable affects all instances
        Example.class_var = new_class_val
        
        # Modifying instance variable affects only this instance
        self.instance_var = new_instance_val

obj1 = Example("Instance 1")
obj2 = Example("Instance 2")

print(f"obj1 class_var: {obj1.class_var}")  # I'm a class variable
print(f"obj2 class_var: {obj2.class_var}")  # I'm a class variable

obj1.modify_variables("Modified class var", "Modified instance 1")

print(f"obj1 class_var: {obj1.class_var}")  # Modified class var
print(f"obj2 class_var: {obj2.class_var}")  # Modified class var (affected!)
print(f"obj1 instance_var: {obj1.instance_var}")  # Modified instance 1
print(f"obj2 instance_var: {obj2.instance_var}")  # Instance 2 (not affected)
```

---

## Practical Examples

### Example 1: Library Book System

```python
class Book:
    # Class variables
    total_books = 0
    library_name = "Central Library"
    
    def __init__(self, title, author, isbn, pages):
        # Instance variables
        self.title = title
        self.author = author
        self.isbn = isbn
        self.pages = pages
        self.is_borrowed = False
        self.borrower = None
        
        # Increment total books
        Book.total_books += 1
    
    def borrow_book(self, borrower_name):
        if not self.is_borrowed:
            self.is_borrowed = True
            self.borrower = borrower_name
            return f"'{self.title}' has been borrowed by {borrower_name}"
        else:
            return f"'{self.title}' is already borrowed by {self.borrower}"
    
    def return_book(self):
        if self.is_borrowed:
            borrower = self.borrower
            self.is_borrowed = False
            self.borrower = None
            return f"'{self.title}' has been returned by {borrower}"
        else:
            return f"'{self.title}' was not borrowed"
    
    def get_info(self):
        status = f"Borrowed by {self.borrower}" if self.is_borrowed else "Available"
        return f"'{self.title}' by {self.author} - {status}"
    
    @classmethod
    def get_library_stats(cls):
        return f"{cls.library_name}: {cls.total_books} books total"

# Usage
book1 = Book("Python Programming", "John Smith", "978-1234567890", 350)
book2 = Book("Data Science Basics", "Jane Doe", "978-0987654321", 420)
book3 = Book("Machine Learning", "Bob Johnson", "978-1122334455", 500)

print(Book.get_library_stats())  # Central Library: 3 books total

print(book1.borrow_book("Alice"))  # 'Python Programming' has been borrowed by Alice
print(book2.borrow_book("Bob"))    # 'Data Science Basics' has been borrowed by Bob
print(book1.borrow_book("Charlie"))  # 'Python Programming' is already borrowed by Alice

print(book1.get_info())  # 'Python Programming' by John Smith - Borrowed by Alice
print(book3.get_info())  # 'Machine Learning' by Bob Johnson - Available

print(book1.return_book())  # 'Python Programming' has been returned by Alice
print(book1.get_info())     # 'Python Programming' by John Smith - Available
```

### Example 2: Simple Game Character System

```python
class Character:
    # Class variables
    game_name = "Adventure Quest"
    total_characters = 0
    
    def __init__(self, name, character_class, level=1):
        # Instance variables
        self.name = name
        self.character_class = character_class
        self.level = level
        self.health = 100
        self.experience = 0
        self.inventory = []
        
        # Class-specific stats
        self.set_class_stats()
        
        # Increment total characters
        Character.total_characters += 1
    
    def set_class_stats(self):
        """Set stats based on character class"""
        if self.character_class.lower() == "warrior":
            self.strength = 15
            self.magic = 5
            self.defense = 12
        elif self.character_class.lower() == "mage":
            self.strength = 7
            self.magic = 18
            self.defense = 6
        elif self.character_class.lower() == "rogue":
            self.strength = 10
            self.magic = 8
            self.defense = 9
        else:
            self.strength = 10
            self.magic = 10
            self.defense = 10
    
    def gain_experience(self, exp_points):
        self.experience += exp_points
        # Level up every 100 experience points
        new_level = (self.experience // 100) + 1
        if new_level > self.level:
            old_level = self.level
            self.level = new_level
            self.health += 20  # Increase health on level up
            return f"{self.name} leveled up from {old_level} to {self.level}!"
        return f"{self.name} gained {exp_points} experience"
    
    def add_to_inventory(self, item):
        self.inventory.append(item)
        return f"{item} added to {self.name}'s inventory"
    
    def get_stats(self):
        return {
            'name': self.name,
            'class': self.character_class,
            'level': self.level,
            'health': self.health,
            'experience': self.experience,
            'strength': self.strength,
            'magic': self.magic,
            'defense': self.defense,
            'inventory': self.inventory
        }
    
    def __str__(self):
        return f"{self.name} (Level {self.level} {self.character_class})"
    
    @classmethod
    def get_game_stats(cls):
        return f"Game: {cls.game_name}, Total Characters: {cls.total_characters}"

# Usage
hero1 = Character("Aragorn", "Warrior")
hero2 = Character("Gandalf", "Mage")
hero3 = Character("Legolas", "Rogue")

print(Character.get_game_stats())  # Game: Adventure Quest, Total Characters: 3

print(hero1.gain_experience(150))  # Aragorn leveled up from 1 to 2!
print(hero2.add_to_inventory("Magic Staff"))  # Magic Staff added to Gandalf's inventory
print(hero3.add_to_inventory("Bow"))  # Bow added to Legolas's inventory

print(hero1)  # Aragorn (Level 2 Warrior)
print(hero2)  # Gandalf (Level 1 Mage)

# Display detailed stats
import json
print(json.dumps(hero1.get_stats(), indent=2))
```

---

## Exercises

### Exercise 1: Student Management System

Create a `Student` class with the following requirements:
- Class variables: `school_name`, `total_students`
- Instance variables: `name`, `student_id`, `grade`, `subjects`
- Methods: `enroll_subject()`, `drop_subject()`, `get_gpa()`, `get_info()`

### Exercise 2: Vehicle Fleet Management

Create a `Vehicle` class with:
- Class variables: `company_name`, `total_vehicles`
- Instance variables: `make`, `model`, `year`, `mileage`, `fuel_level`
- Methods: `drive()`, `refuel()`, `get_info()`, `service_needed()`

### Exercise 3: Bank Account System

Create a `BankAccount` class with:
- Class variables: `bank_name`, `total_accounts`, `interest_rate`
- Instance variables: `account_number`, `holder_name`, `balance`, `account_type`
- Methods: `deposit()`, `withdraw()`, `transfer()`, `calculate_interest()`, `get_statement()`

### Exercise 4: Online Shopping Cart

Create a `ShoppingCart` class with:
- Class variables: `store_name`, `tax_rate`
- Instance variables: `customer_name`, `items`, `total_amount`
- Methods: `add_item()`, `remove_item()`, `calculate_total()`, `apply_discount()`, `checkout()`

### Exercise 5: Game Inventory System

Create an `Inventory` class with:
- Class variables: `max_capacity`, `game_name`
- Instance variables: `owner`, `items`, `current_weight`
- Methods: `add_item()`, `remove_item()`, `get_weight()`, `is_full()`, `list_items()`

### Solutions

Here's a solution for Exercise 1:

```python
class Student:
    school_name = "Python Academy"
    total_students = 0
    
    def __init__(self, name, student_id, grade):
        self.name = name
        self.student_id = student_id
        self.grade = grade
        self.subjects = {}  # subject_name: grade
        
        Student.total_students += 1
    
    def enroll_subject(self, subject_name):
        if subject_name not in self.subjects:
            self.subjects[subject_name] = None
            return f"{self.name} enrolled in {subject_name}"
        return f"{self.name} is already enrolled in {subject_name}"
    
    def drop_subject(self, subject_name):
        if subject_name in self.subjects:
            del self.subjects[subject_name]
            return f"{self.name} dropped {subject_name}"
        return f"{self.name} is not enrolled in {subject_name}"
    
    def add_grade(self, subject_name, grade):
        if subject_name in self.subjects:
            self.subjects[subject_name] = grade
            return f"Grade {grade} added for {subject_name}"
        return f"{self.name} is not enrolled in {subject_name}"
    
    def get_gpa(self):
        grades = [g for g in self.subjects.values() if g is not None]
        if grades:
            return sum(grades) / len(grades)
        return 0.0
    
    def get_info(self):
        return {
            'name': self.name,
            'student_id': self.student_id,
            'grade': self.grade,
            'subjects': self.subjects,
            'gpa': round(self.get_gpa(), 2)
        }
    
    @classmethod
    def get_school_stats(cls):
        return f"School: {cls.school_name}, Total Students: {cls.total_students}"

# Test the solution
student1 = Student("Alice Johnson", "S001", 10)
student1.enroll_subject("Mathematics")
student1.enroll_subject("Science")
student1.add_grade("Mathematics", 95)
student1.add_grade("Science", 88)

print(student1.get_info())
print(Student.get_school_stats())
```

## Summary

In this section, you learned:

1. **Classes and Objects**: How to define classes and create instances
2. **Attributes and Methods**: How to store data and define behavior
3. **Constructor**: How to initialize objects with `__init__`
4. **Destructor**: How to clean up resources with `__del__`
5. **Instance vs Class Variables**: The difference between instance-specific and class-wide data

These fundamentals form the foundation for more advanced OOP concepts like inheritance, polymorphism, and encapsulation that we'll cover in the next sections.

## Next Steps

1. Practice creating classes for real-world entities
2. Experiment with different constructor patterns
3. Understand when to use instance vs class variables
4. Complete the exercises to reinforce your understanding
5. Move on to OOP principles (encapsulation, inheritance, polymorphism, abstraction)
