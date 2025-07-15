# Python Intermediate 1.1: Object-Oriented Programming Fundamentals

## Table of Contents
- [1. Introduction to Object-Oriented Programming](#1-introduction-to-object-oriented-programming)
- [2. Classes and Objects](#2-classes-and-objects)
- [3. Attributes and Methods](#3-attributes-and-methods)
- [4. Constructor and Destructor](#4-constructor-and-destructor)
- [5. Instance vs Class Variables](#5-instance-vs-class-variables)
- [6. Method Types](#6-method-types)
- [7. Access Modifiers and Encapsulation](#7-access-modifiers-and-encapsulation)
- [8. Property Decorators](#8-property-decorators)
- [9. Class Relationships](#9-class-relationships)
- [10. Practical Examples](#10-practical-examples)
- [11. Best Practices](#11-best-practices)
- [12. Common Pitfalls](#12-common-pitfalls)
- [13. Review Questions](#13-review-questions)
- [14. Answers to Review Questions](#14-answers-to-review-questions)

---

## 1. Introduction to Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm that organizes code around objects and classes rather than functions and logic. It's based on the concept of "objects" which contain data (attributes) and code (methods).

### Key Concepts of OOP:
- **Encapsulation**: Bundling data and methods that operate on that data
- **Inheritance**: Creating new classes based on existing ones
- **Polymorphism**: Using a single interface to represent different underlying forms
- **Abstraction**: Hiding complex implementation details

### Why Use OOP?
- **Modularity**: Code is organized into self-contained units
- **Reusability**: Classes can be reused across different programs
- **Maintainability**: Easier to modify and extend existing code
- **Scalability**: Better structure for large applications

---

## 2. Classes and Objects

### 2.1 Defining a Class

A class is a blueprint for creating objects. It defines the structure and behavior that objects of that class will have.

```python
class Car:
    """A simple car class"""
    pass  # Empty class for now
```

### 2.2 Creating Objects (Instantiation)

Objects are instances of a class. You create objects by calling the class like a function.

```python
# Creating objects
my_car = Car()
your_car = Car()

# Check the type
print(type(my_car))  # <class '__main__.Car'>
print(isinstance(my_car, Car))  # True
```

### 2.3 Basic Class with Attributes

```python
class Student:
    """A student class with basic attributes"""
    
    def __init__(self, name, age, student_id):
        self.name = name
        self.age = age
        self.student_id = student_id
    
    def display_info(self):
        return f"Student: {self.name}, Age: {self.age}, ID: {self.student_id}"

# Creating and using objects
student1 = Student("Alice", 20, "S001")
student2 = Student("Bob", 22, "S002")

print(student1.display_info())
print(student2.display_info())
```

---

## 3. Attributes and Methods

### 3.1 Instance Attributes

Instance attributes are variables that belong to a specific instance of a class.

```python
class Person:
    def __init__(self, name, age):
        self.name = name  # Instance attribute
        self.age = age    # Instance attribute
        self.friends = []  # Instance attribute (mutable)
    
    def add_friend(self, friend):
        self.friends.append(friend)
    
    def get_friends(self):
        return self.friends.copy()  # Return a copy to prevent external modification

person1 = Person("Alice", 25)
person2 = Person("Bob", 30)

person1.add_friend("Charlie")
person2.add_friend("David")

print(person1.get_friends())  # ['Charlie']
print(person2.get_friends())  # ['David']
```

### 3.2 Instance Methods

Instance methods are functions defined inside a class that operate on instances.

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
    
    def get_history(self):
        return self.history.copy()
    
    def clear_history(self):
        self.history.clear()

calc = Calculator()
print(calc.add(5, 3))  # 8
print(calc.subtract(10, 4))  # 6
print(calc.get_history())  # ['5 + 3 = 8', '10 - 4 = 6']
```

### 3.3 Method Chaining

You can enable method chaining by returning `self` from methods.

```python
class StringBuilder:
    def __init__(self):
        self.text = ""
    
    def append(self, text):
        self.text += text
        return self  # Enable method chaining
    
    def prepend(self, text):
        self.text = text + self.text
        return self
    
    def upper(self):
        self.text = self.text.upper()
        return self
    
    def build(self):
        return self.text

# Method chaining example
result = (StringBuilder()
          .append("Hello")
          .append(" ")
          .append("World")
          .upper()
          .build())
print(result)  # "HELLO WORLD"
```

---

## 4. Constructor and Destructor

### 4.1 Constructor (`__init__`)

The constructor is called when an object is created. It initializes the object's attributes.

```python
class BankAccount:
    def __init__(self, account_number, initial_balance=0):
        self.account_number = account_number
        self.balance = initial_balance
        self.transactions = []
        print(f"Account {account_number} created with balance ${initial_balance}")
    
    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            self.transactions.append(f"Deposited ${amount}")
            return True
        return False
    
    def withdraw(self, amount):
        if 0 < amount <= self.balance:
            self.balance -= amount
            self.transactions.append(f"Withdrew ${amount}")
            return True
        return False
    
    def get_balance(self):
        return self.balance

# Using the constructor
account = BankAccount("12345", 1000)
account.deposit(500)
print(f"Balance: ${account.get_balance()}")
```

### 4.2 Destructor (`__del__`)

The destructor is called when an object is about to be destroyed (garbage collected).

```python
class FileHandler:
    def __init__(self, filename):
        self.filename = filename
        self.file = None
        print(f"FileHandler created for {filename}")
    
    def open_file(self, mode='r'):
        try:
            self.file = open(self.filename, mode)
            print(f"File {self.filename} opened")
        except FileNotFoundError:
            print(f"File {self.filename} not found")
    
    def close_file(self):
        if self.file:
            self.file.close()
            print(f"File {self.filename} closed")
    
    def __del__(self):
        print(f"FileHandler for {self.filename} is being destroyed")
        if self.file:
            self.file.close()

# Note: In practice, use context managers instead of relying on __del__
```

---

## 5. Instance vs Class Variables

### 5.1 Instance Variables

Instance variables are unique to each instance of a class.

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name      # Instance variable
        self.breed = breed    # Instance variable
        self.tricks = []      # Instance variable
    
    def add_trick(self, trick):
        self.tricks.append(trick)

dog1 = Dog("Buddy", "Golden Retriever")
dog2 = Dog("Max", "German Shepherd")

dog1.add_trick("sit")
dog2.add_trick("roll over")

print(f"{dog1.name} knows: {dog1.tricks}")  # Buddy knows: ['sit']
print(f"{dog2.name} knows: {dog2.tricks}")  # Max knows: ['roll over']
```

### 5.2 Class Variables

Class variables are shared by all instances of a class.

```python
class Employee:
    # Class variables
    company_name = "TechCorp"
    employee_count = 0
    
    def __init__(self, name, salary):
        self.name = name        # Instance variable
        self.salary = salary    # Instance variable
        Employee.employee_count += 1  # Modify class variable
    
    @classmethod
    def get_employee_count(cls):
        return cls.employee_count
    
    @classmethod
    def set_company_name(cls, new_name):
        cls.company_name = new_name

# Creating employees
emp1 = Employee("Alice", 50000)
emp2 = Employee("Bob", 60000)

print(f"Company: {Employee.company_name}")  # TechCorp
print(f"Total employees: {Employee.get_employee_count()}")  # 2

# All instances share the same class variable
print(f"Company via emp1: {emp1.company_name}")  # TechCorp
print(f"Company via emp2: {emp2.company_name}")  # TechCorp
```

### 5.3 Mutable Class Variables (Common Pitfall)

```python
# WRONG - Dangerous mutable class variable
class BadClass:
    shared_list = []  # This is shared among ALL instances
    
    def add_item(self, item):
        self.shared_list.append(item)

# CORRECT - Instance variable
class GoodClass:
    def __init__(self):
        self.instance_list = []  # Each instance has its own list
    
    def add_item(self, item):
        self.instance_list.append(item)

# Demonstrating the problem
bad1 = BadClass()
bad2 = BadClass()

bad1.add_item("item1")
bad2.add_item("item2")

print(bad1.shared_list)  # ['item1', 'item2'] - UNEXPECTED!
print(bad2.shared_list)  # ['item1', 'item2'] - UNEXPECTED!

# Correct approach
good1 = GoodClass()
good2 = GoodClass()

good1.add_item("item1")
good2.add_item("item2")

print(good1.instance_list)  # ['item1'] - EXPECTED
print(good2.instance_list)  # ['item2'] - EXPECTED
```

---

## 6. Method Types

### 6.1 Instance Methods

Instance methods operate on instances of the class and have access to instance attributes.

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):  # Instance method
        return self.width * self.height
    
    def perimeter(self):  # Instance method
        return 2 * (self.width + self.height)
    
    def scale(self, factor):  # Instance method that modifies the instance
        self.width *= factor
        self.height *= factor

rect = Rectangle(5, 3)
print(rect.area())  # 15
rect.scale(2)
print(rect.area())  # 60
```

### 6.2 Class Methods

Class methods operate on the class itself and are decorated with `@classmethod`.

```python
class Person:
    species = "Homo sapiens"
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    @classmethod
    def from_string(cls, person_str):
        """Alternative constructor from string"""
        name, age = person_str.split('-')
        return cls(name, int(age))
    
    @classmethod
    def get_species(cls):
        return cls.species
    
    @classmethod
    def set_species(cls, new_species):
        cls.species = new_species

# Using class methods
person1 = Person("Alice", 25)
person2 = Person.from_string("Bob-30")

print(Person.get_species())  # Homo sapiens
print(person1.name, person1.age)  # Alice 25
print(person2.name, person2.age)  # Bob 30
```

### 6.3 Static Methods

Static methods don't operate on instances or classes directly and are decorated with `@staticmethod`.

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b
    
    @staticmethod
    def is_prime(n):
        if n < 2:
            return False
        for i in range(2, int(n ** 0.5) + 1):
            if n % i == 0:
                return False
        return True
    
    @staticmethod
    def fibonacci(n):
        if n <= 1:
            return n
        a, b = 0, 1
        for _ in range(2, n + 1):
            a, b = b, a + b
        return b

# Static methods can be called on the class or instances
print(MathUtils.add(5, 3))  # 8
print(MathUtils.is_prime(17))  # True
print(MathUtils.fibonacci(10))  # 55

# Can also be called on instances (though not recommended)
utils = MathUtils()
print(utils.add(2, 3))  # 5
```

---

## 7. Access Modifiers and Encapsulation

Python uses naming conventions to indicate the intended visibility of attributes and methods.

### 7.1 Public Members

Public members are accessible from anywhere.

```python
class PublicExample:
    def __init__(self):
        self.public_var = "I'm public"
    
    def public_method(self):
        return "This is a public method"

obj = PublicExample()
print(obj.public_var)  # Accessible
print(obj.public_method())  # Accessible
```

### 7.2 Protected Members (Single Underscore)

Protected members are intended for internal use within the class and its subclasses.

```python
class ProtectedExample:
    def __init__(self):
        self._protected_var = "I'm protected"
    
    def _protected_method(self):
        return "This is a protected method"
    
    def public_method(self):
        return self._protected_method()

obj = ProtectedExample()
print(obj._protected_var)  # Accessible but not recommended
print(obj.public_method())  # Recommended way to access protected functionality
```

### 7.3 Private Members (Double Underscore)

Private members are name-mangled to prevent accidental access.

```python
class PrivateExample:
    def __init__(self):
        self.__private_var = "I'm private"
    
    def __private_method(self):
        return "This is a private method"
    
    def public_method(self):
        return self.__private_method()
    
    def get_private_var(self):
        return self.__private_var

obj = PrivateExample()
print(obj.get_private_var())  # Correct way to access private data
print(obj.public_method())  # Correct way to access private method

# These will raise AttributeError
# print(obj.__private_var)  # AttributeError
# print(obj.__private_method())  # AttributeError

# But you can still access via name mangling (not recommended)
print(obj._PrivateExample__private_var)  # "I'm private"
```

### 7.4 Practical Encapsulation Example

```python
class BankAccount:
    def __init__(self, account_number, initial_balance=0):
        self.account_number = account_number  # Public
        self._balance = initial_balance       # Protected
        self.__pin = None                     # Private
    
    def set_pin(self, pin):
        if isinstance(pin, int) and 1000 <= pin <= 9999:
            self.__pin = pin
            return True
        return False
    
    def verify_pin(self, pin):
        return self.__pin == pin
    
    def deposit(self, amount):
        if amount > 0:
            self._balance += amount
            return True
        return False
    
    def withdraw(self, amount, pin):
        if self.verify_pin(pin) and 0 < amount <= self._balance:
            self._balance -= amount
            return True
        return False
    
    def get_balance(self, pin):
        if self.verify_pin(pin):
            return self._balance
        return None

account = BankAccount("12345", 1000)
account.set_pin(1234)
account.deposit(500)
print(account.get_balance(1234))  # 1500
```

---

## 8. Property Decorators

Properties allow you to define methods that can be accessed like attributes.

### 8.1 Basic Property

```python
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
    
    @property
    def circumference(self):
        return 2 * 3.14159 * self._radius

circle = Circle(5)
print(circle.radius)  # 5
print(circle.area)  # 78.53975
print(circle.circumference)  # 31.4159

circle.radius = 10
print(circle.area)  # 314.159
```

### 8.2 Read-Only Properties

```python
class Person:
    def __init__(self, first_name, last_name, birth_year):
        self.first_name = first_name
        self.last_name = last_name
        self.birth_year = birth_year
    
    @property
    def full_name(self):
        return f"{self.first_name} {self.last_name}"
    
    @property
    def age(self):
        from datetime import datetime
        current_year = datetime.now().year
        return current_year - self.birth_year

person = Person("Alice", "Smith", 1990)
print(person.full_name)  # Alice Smith
print(person.age)  # 35 (assuming current year is 2025)

# These would raise AttributeError since there's no setter
# person.full_name = "Bob Jones"  # AttributeError
# person.age = 25  # AttributeError
```

### 8.3 Property with Validation

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature below absolute zero is not possible")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        return (self._celsius * 9/5) + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        self.celsius = (value - 32) * 5/9
    
    @property
    def kelvin(self):
        return self._celsius + 273.15
    
    @kelvin.setter
    def kelvin(self, value):
        self.celsius = value - 273.15

temp = Temperature(25)
print(f"Celsius: {temp.celsius}")  # 25
print(f"Fahrenheit: {temp.fahrenheit}")  # 77.0
print(f"Kelvin: {temp.kelvin}")  # 298.15

temp.fahrenheit = 86
print(f"Celsius: {temp.celsius}")  # 30.0
```

---

## 9. Class Relationships

### 9.1 Composition

Composition represents a "has-a" relationship where one class contains objects of another class.

```python
class Engine:
    def __init__(self, horsepower, fuel_type):
        self.horsepower = horsepower
        self.fuel_type = fuel_type
        self.is_running = False
    
    def start(self):
        self.is_running = True
        return "Engine started"
    
    def stop(self):
        self.is_running = False
        return "Engine stopped"

class Car:
    def __init__(self, make, model, engine):
        self.make = make
        self.model = model
        self.engine = engine  # Composition
        self.is_moving = False
    
    def start_car(self):
        return self.engine.start()
    
    def stop_car(self):
        self.engine.stop()
        self.is_moving = False
        return "Car stopped"
    
    def accelerate(self):
        if self.engine.is_running:
            self.is_moving = True
            return "Car is accelerating"
        return "Start the engine first"

# Usage
engine = Engine(300, "Gasoline")
car = Car("Toyota", "Camry", engine)

print(car.start_car())  # Engine started
print(car.accelerate())  # Car is accelerating
```

### 9.2 Aggregation

Aggregation is a weaker form of composition where objects can exist independently.

```python
class Student:
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
    
    def __str__(self):
        return f"Student: {self.name} (ID: {self.student_id})"

class Course:
    def __init__(self, course_name, course_code):
        self.course_name = course_name
        self.course_code = course_code
        self.students = []  # Aggregation
    
    def add_student(self, student):
        if student not in self.students:
            self.students.append(student)
            return f"{student.name} added to {self.course_name}"
        return f"{student.name} already enrolled"
    
    def remove_student(self, student):
        if student in self.students:
            self.students.remove(student)
            return f"{student.name} removed from {self.course_name}"
        return f"{student.name} not found in {self.course_name}"
    
    def get_students(self):
        return [str(student) for student in self.students]

# Usage
student1 = Student("Alice", "S001")
student2 = Student("Bob", "S002")

course = Course("Python Programming", "CS101")
print(course.add_student(student1))
print(course.add_student(student2))
print(course.get_students())
```

---

## 10. Practical Examples

### 10.1 Library Management System

```python
from datetime import datetime, timedelta

class Book:
    def __init__(self, isbn, title, author, publication_year):
        self.isbn = isbn
        self.title = title
        self.author = author
        self.publication_year = publication_year
        self.is_available = True
    
    def __str__(self):
        return f"{self.title} by {self.author} ({self.publication_year})"

class Member:
    def __init__(self, member_id, name, email):
        self.member_id = member_id
        self.name = name
        self.email = email
        self.borrowed_books = []
    
    def borrow_book(self, book):
        if book.is_available:
            book.is_available = False
            self.borrowed_books.append(book)
            return f"{self.name} borrowed {book.title}"
        return f"{book.title} is not available"
    
    def return_book(self, book):
        if book in self.borrowed_books:
            book.is_available = True
            self.borrowed_books.remove(book)
            return f"{self.name} returned {book.title}"
        return f"{self.name} hasn't borrowed {book.title}"

class Library:
    def __init__(self, name):
        self.name = name
        self.books = []
        self.members = []
    
    def add_book(self, book):
        self.books.append(book)
        return f"Added {book.title} to {self.name}"
    
    def register_member(self, member):
        self.members.append(member)
        return f"Registered {member.name} as member"
    
    def find_book(self, title):
        for book in self.books:
            if book.title.lower() == title.lower():
                return book
        return None
    
    def find_member(self, member_id):
        for member in self.members:
            if member.member_id == member_id:
                return member
        return None
    
    def get_available_books(self):
        return [book for book in self.books if book.is_available]

# Usage
library = Library("City Library")

# Add books
book1 = Book("978-0134685991", "Effective Python", "Brett Slatkin", 2019)
book2 = Book("978-1491946008", "Fluent Python", "Luciano Ramalho", 2015)

library.add_book(book1)
library.add_book(book2)

# Register members
member1 = Member("M001", "Alice Johnson", "alice@email.com")
member2 = Member("M002", "Bob Smith", "bob@email.com")

library.register_member(member1)
library.register_member(member2)

# Borrow and return books
print(member1.borrow_book(book1))
print(member2.borrow_book(book1))  # Should fail
print(member1.return_book(book1))
print(member2.borrow_book(book1))  # Should succeed now
```

### 10.2 Shopping Cart System

```python
class Product:
    def __init__(self, product_id, name, price, category):
        self.product_id = product_id
        self.name = name
        self.price = price
        self.category = category
    
    def __str__(self):
        return f"{self.name} - ${self.price:.2f}"
    
    def __eq__(self, other):
        return isinstance(other, Product) and self.product_id == other.product_id

class CartItem:
    def __init__(self, product, quantity):
        self.product = product
        self.quantity = quantity
    
    @property
    def total_price(self):
        return self.product.price * self.quantity
    
    def __str__(self):
        return f"{self.product.name} x{self.quantity} = ${self.total_price:.2f}"

class ShoppingCart:
    def __init__(self):
        self.items = []
    
    def add_item(self, product, quantity=1):
        for item in self.items:
            if item.product == product:
                item.quantity += quantity
                return f"Updated {product.name} quantity to {item.quantity}"
        
        self.items.append(CartItem(product, quantity))
        return f"Added {product.name} to cart"
    
    def remove_item(self, product):
        for item in self.items:
            if item.product == product:
                self.items.remove(item)
                return f"Removed {product.name} from cart"
        return f"{product.name} not found in cart"
    
    def update_quantity(self, product, new_quantity):
        for item in self.items:
            if item.product == product:
                if new_quantity <= 0:
                    return self.remove_item(product)
                item.quantity = new_quantity
                return f"Updated {product.name} quantity to {new_quantity}"
        return f"{product.name} not found in cart"
    
    @property
    def total_price(self):
        return sum(item.total_price for item in self.items)
    
    @property
    def item_count(self):
        return sum(item.quantity for item in self.items)
    
    def get_items(self):
        return [str(item) for item in self.items]
    
    def clear(self):
        self.items.clear()
        return "Cart cleared"

# Usage
cart = ShoppingCart()

# Create products
laptop = Product("P001", "Laptop", 999.99, "Electronics")
mouse = Product("P002", "Wireless Mouse", 29.99, "Electronics")
book = Product("P003", "Python Book", 39.99, "Books")

# Add items to cart
print(cart.add_item(laptop))
print(cart.add_item(mouse, 2))
print(cart.add_item(book))

# Display cart
print(f"\nCart Items:")
for item in cart.get_items():
    print(f"  {item}")

print(f"\nTotal Items: {cart.item_count}")
print(f"Total Price: ${cart.total_price:.2f}")

# Update quantity
print(f"\n{cart.update_quantity(mouse, 1)}")
print(f"New Total: ${cart.total_price:.2f}")
```

---

## 11. Best Practices

### 11.1 Design Principles

1. **Single Responsibility Principle**: Each class should have only one reason to change
2. **Don't Repeat Yourself (DRY)**: Avoid code duplication
3. **Composition over Inheritance**: Prefer composition when possible
4. **Clear Naming**: Use descriptive names for classes, methods, and attributes

### 11.2 Code Organization

```python
# Good class structure
class GoodClass:
    """Clear docstring explaining the class purpose"""
    
    # Class variables first
    CLASS_CONSTANT = "value"
    
    def __init__(self, param1, param2):
        """Initialize the instance"""
        self.param1 = param1
        self.param2 = param2
    
    def __str__(self):
        """String representation for users"""
        return f"GoodClass({self.param1}, {self.param2})"
    
    def __repr__(self):
        """String representation for developers"""
        return f"GoodClass(param1={self.param1!r}, param2={self.param2!r})"
    
    # Public methods
    def public_method(self):
        """Public method docstring"""
        return self._private_method()
    
    # Private methods at the end
    def _private_method(self):
        """Private method docstring"""
        return "private result"
```

### 11.3 Error Handling in Classes

```python
class BankAccount:
    def __init__(self, account_number, initial_balance=0):
        if not isinstance(account_number, str) or not account_number:
            raise ValueError("Account number must be a non-empty string")
        if not isinstance(initial_balance, (int, float)) or initial_balance < 0:
            raise ValueError("Initial balance must be a non-negative number")
        
        self.account_number = account_number
        self._balance = initial_balance
    
    def deposit(self, amount):
        if not isinstance(amount, (int, float)):
            raise TypeError("Amount must be a number")
        if amount <= 0:
            raise ValueError("Amount must be positive")
        
        self._balance += amount
        return self._balance
    
    def withdraw(self, amount):
        if not isinstance(amount, (int, float)):
            raise TypeError("Amount must be a number")
        if amount <= 0:
            raise ValueError("Amount must be positive")
        if amount > self._balance:
            raise ValueError("Insufficient funds")
        
        self._balance -= amount
        return self._balance
    
    @property
    def balance(self):
        return self._balance
```

---

## 12. Common Pitfalls

### 12.1 Mutable Default Arguments

```python
# WRONG
class BadClass:
    def __init__(self, items=[]):  # Dangerous!
        self.items = items

# CORRECT
class GoodClass:
    def __init__(self, items=None):
        self.items = items if items is not None else []
```

### 12.2 Modifying Class Variables

```python
# WRONG
class BadCounter:
    count = 0
    
    def increment(self):
        self.count += 1  # Creates instance variable, doesn't modify class variable

# CORRECT
class GoodCounter:
    count = 0
    
    def increment(self):
        GoodCounter.count += 1  # or self.__class__.count += 1
```

### 12.3 Circular References

```python
# WRONG - Can cause memory leaks
class Parent:
    def __init__(self):
        self.child = Child(self)

class Child:
    def __init__(self, parent):
        self.parent = parent

# CORRECT - Use weak references when needed
import weakref

class Parent:
    def __init__(self):
        self.child = Child(self)

class Child:
    def __init__(self, parent):
        self._parent = weakref.ref(parent)
    
    @property
    def parent(self):
        return self._parent()
```

---

## 13. Review Questions

1. **What is the difference between a class and an object?**

2. **Explain the purpose of the `__init__` method. Can a class have multiple `__init__` methods?**

3. **What is the difference between instance variables and class variables? Provide an example.**

4. **What are the three types of methods in Python classes? Explain each with an example.**

5. **What is encapsulation, and how is it implemented in Python?**

6. **Explain the difference between public, protected, and private members in Python.**

7. **What are property decorators, and why are they useful?**

8. **What is method chaining, and how do you implement it?**

9. **Explain the difference between composition and aggregation with examples.**

10. **What are some common pitfalls when working with classes in Python?**

11. **How do you create a read-only property in Python?**

12. **What is the purpose of the `@classmethod` decorator?**

13. **When would you use a static method instead of a regular method?**

14. **How do you properly handle mutable default arguments in class constructors?**

15. **What is the difference between `__str__` and `__repr__` methods?**

---

## 14. Answers to Review Questions

1. **What is the difference between a class and an object?**
   - A **class** is a blueprint or template that defines the structure and behavior of objects. It specifies what attributes and methods objects of that class will have.
   - An **object** is an instance of a class. It's a concrete realization of the class with specific values for its attributes.
   - Example: `Car` is a class, while `my_car = Car("Toyota", "Camry")` creates an object.

2. **Explain the purpose of the `__init__` method. Can a class have multiple `__init__` methods?**
   - The `__init__` method is the constructor that initializes a new instance of a class. It's called automatically when an object is created.
   - Python doesn't support method overloading, so a class can only have one `__init__` method. However, you can use default parameters or class methods to create alternative constructors.
   ```python
   class Person:
       def __init__(self, name, age=0):
           self.name = name
           self.age = age
       
       @classmethod
       def from_string(cls, person_str):
           name, age = person_str.split('-')
           return cls(name, int(age))
   ```

3. **What is the difference between instance variables and class variables?**
   - **Instance variables** are unique to each instance of a class. They're defined inside methods (usually `__init__`) and accessed via `self`.
   - **Class variables** are shared by all instances of a class. They're defined at the class level and belong to the class itself.
   ```python
   class Student:
       school_name = "ABC School"  # Class variable
       
       def __init__(self, name):
           self.name = name  # Instance variable
   ```

4. **What are the three types of methods in Python classes?**
   - **Instance methods**: Operate on instances, take `self` as first parameter
   - **Class methods**: Operate on the class, decorated with `@classmethod`, take `cls` as first parameter
   - **Static methods**: Don't operate on instances or classes, decorated with `@staticmethod`, no special first parameter
   ```python
   class Example:
       def instance_method(self):
           pass
       
       @classmethod
       def class_method(cls):
           pass
       
       @staticmethod
       def static_method():
           pass
   ```

5. **What is encapsulation, and how is it implemented in Python?**
   - **Encapsulation** is the bundling of data and methods that operate on that data within a single unit (class), while restricting access to some components.
   - In Python, encapsulation is implemented through naming conventions:
     - Public: normal names (`attribute`)
     - Protected: single underscore (`_attribute`)
     - Private: double underscore (`__attribute`)

6. **Explain the difference between public, protected, and private members in Python.**
   - **Public**: Accessible from anywhere (e.g., `self.public_var`)
   - **Protected**: Intended for internal use within the class and subclasses (e.g., `self._protected_var`)
   - **Private**: Name-mangled to prevent accidental access (e.g., `self.__private_var`)
   
   Note: Python's privacy is based on convention rather than strict enforcement.

7. **What are property decorators, and why are they useful?**
   - Property decorators allow you to define methods that can be accessed like attributes, providing a way to add validation, computation, or access control to attribute access.
   - They're useful for:
     - Data validation
     - Computed properties
     - Maintaining backward compatibility
     - Controlling access to attributes

8. **What is method chaining, and how do you implement it?**
   - Method chaining allows you to call multiple methods in sequence on the same object.
   - Implementation: Return `self` from methods that modify the object.
   ```python
   class Builder:
       def method1(self):
           # do something
           return self
       
       def method2(self):
           # do something else
           return self
   
   # Usage: obj.method1().method2()
   ```

9. **Explain the difference between composition and aggregation with examples.**
   - **Composition**: "Has-a" relationship where the contained object cannot exist without the container (strong relationship)
   - **Aggregation**: "Has-a" relationship where the contained object can exist independently (weak relationship)
   
   Example:
   - Composition: Car has Engine (engine is part of car)
   - Aggregation: University has Students (students can exist without the university)

10. **What are some common pitfalls when working with classes in Python?**
    - Mutable default arguments
    - Modifying class variables incorrectly
    - Circular references
    - Not using `super()` properly in inheritance
    - Forgetting to call parent `__init__` in subclasses
    - Using class variables for instance-specific data

11. **How do you create a read-only property in Python?**
    - Define a property with only a getter method, no setter:
    ```python
    class Circle:
        def __init__(self, radius):
            self._radius = radius
        
        @property
        def area(self):
            return 3.14159 * self._radius ** 2
    ```

12. **What is the purpose of the `@classmethod` decorator?**
    - Class methods operate on the class itself rather than instances
    - They receive the class as the first argument (`cls`)
    - Common uses:
      - Alternative constructors
      - Accessing/modifying class variables
      - Factory methods

13. **When would you use a static method instead of a regular method?**
    - Use static methods when:
      - The method doesn't need access to `self` or `cls`
      - The method is logically related to the class but doesn't depend on class/instance state
      - You want to group utility functions with a class
    - Example: Validation functions, utility methods

14. **How do you properly handle mutable default arguments in class constructors?**
    - Use `None` as default and create the mutable object inside the method:
    ```python
    def __init__(self, items=None):
        self.items = items if items is not None else []
    ```

15. **What is the difference between `__str__` and `__repr__` methods?**
    - `__str__`: Returns a human-readable string representation (for end users)
    - `__repr__`: Returns an unambiguous string representation (for developers/debugging)
    - `__str__` is called by `str()` and `print()`, `__repr__` is called by `repr()` and in interactive sessions
    - If only `__repr__` is defined, it's used for both purposes

---

*This completes the OOP Fundamentals module. The concepts covered here form the foundation for all object-oriented programming in Python and are essential for building complex, maintainable applications.*
