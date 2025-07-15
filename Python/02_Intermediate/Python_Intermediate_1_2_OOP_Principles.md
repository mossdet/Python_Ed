# Python Intermediate 1.2: Object-Oriented Programming Principles

## Table of Contents
- [1. Introduction to OOP Principles](#1-introduction-to-oop-principles)
- [2. Encapsulation](#2-encapsulation)
- [3. Inheritance](#3-inheritance)
- [4. Polymorphism](#4-polymorphism)
- [5. Abstraction](#5-abstraction)
- [6. Advanced Inheritance Concepts](#6-advanced-inheritance-concepts)
- [7. Multiple Inheritance and Method Resolution Order](#7-multiple-inheritance-and-method-resolution-order)
- [8. Design Patterns and OOP](#8-design-patterns-and-oop)
- [9. SOLID Principles](#9-solid-principles)
- [10. Practical Examples](#10-practical-examples)
- [11. Best Practices](#11-best-practices)
- [12. Common Antipatterns](#12-common-antipatterns)
- [13. Review Questions](#13-review-questions)
- [14. Answers to Review Questions](#14-answers-to-review-questions)

---

## 1. Introduction to OOP Principles

The four fundamental principles of Object-Oriented Programming form the foundation of good software design. These principles help create maintainable, scalable, and robust applications.

### The Four Pillars of OOP:
1. **Encapsulation**: Bundling data and methods, controlling access
2. **Inheritance**: Creating new classes based on existing ones
3. **Polymorphism**: One interface, multiple implementations
4. **Abstraction**: Hiding complex implementation details

### Benefits of Following OOP Principles:
- **Code Reusability**: Write once, use many times
- **Maintainability**: Easier to modify and extend
- **Modularity**: Self-contained, interchangeable components
- **Flexibility**: Adapt to changing requirements
- **Testability**: Easier to unit test individual components

---

## 2. Encapsulation

Encapsulation is the bundling of data and methods that operate on that data within a single unit, while restricting access to some components.

### 2.1 Data Hiding and Access Control

```python
class BankAccount:
    def __init__(self, account_number, initial_balance=0):
        self.account_number = account_number  # Public
        self._balance = initial_balance       # Protected
        self.__transaction_history = []       # Private
    
    def deposit(self, amount):
        if self._validate_amount(amount):
            self._balance += amount
            self.__record_transaction(f"Deposit: ${amount}")
            return True
        return False
    
    def withdraw(self, amount):
        if self._validate_amount(amount) and amount <= self._balance:
            self._balance -= amount
            self.__record_transaction(f"Withdrawal: ${amount}")
            return True
        return False
    
    def get_balance(self):
        return self._balance
    
    def get_transaction_history(self):
        return self.__transaction_history.copy()
    
    def _validate_amount(self, amount):
        """Protected method for internal validation"""
        return isinstance(amount, (int, float)) and amount > 0
    
    def __record_transaction(self, transaction):
        """Private method for internal record keeping"""
        from datetime import datetime
        self.__transaction_history.append({
            'timestamp': datetime.now(),
            'transaction': transaction
        })

# Usage
account = BankAccount("12345", 1000)
print(account.deposit(500))  # True
print(account.get_balance())  # 1500
print(account.withdraw(200))  # True
print(len(account.get_transaction_history()))  # 2

# Direct access to private members fails
# print(account.__transaction_history)  # AttributeError
```

### 2.2 Property-Based Encapsulation

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

# Usage
temp = Temperature(25)
print(f"Celsius: {temp.celsius}°C")
print(f"Fahrenheit: {temp.fahrenheit}°F")
print(f"Kelvin: {temp.kelvin}K")

temp.fahrenheit = 86
print(f"Celsius: {temp.celsius}°C")  # 30.0°C
```

### 2.3 Encapsulation with Context Managers

```python
class DatabaseConnection:
    def __init__(self, connection_string):
        self._connection_string = connection_string
        self._connection = None
        self._is_connected = False
    
    def __enter__(self):
        self._connect()
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self._disconnect()
    
    def _connect(self):
        """Private method to establish connection"""
        print(f"Connecting to {self._connection_string}")
        self._connection = f"Connection to {self._connection_string}"
        self._is_connected = True
    
    def _disconnect(self):
        """Private method to close connection"""
        print("Disconnecting from database")
        self._connection = None
        self._is_connected = False
    
    def execute_query(self, query):
        if not self._is_connected:
            raise RuntimeError("Not connected to database")
        return f"Executing: {query}"

# Usage
with DatabaseConnection("localhost:5432/mydb") as db:
    result = db.execute_query("SELECT * FROM users")
    print(result)
# Connection automatically closed
```

---

## 3. Inheritance

Inheritance allows a class to inherit attributes and methods from another class, promoting code reuse and establishing hierarchical relationships.

### 3.1 Basic Inheritance

```python
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species
        self.energy = 100
    
    def eat(self, food):
        self.energy += 10
        return f"{self.name} ate {food}"
    
    def sleep(self):
        self.energy += 20
        return f"{self.name} is sleeping"
    
    def make_sound(self):
        return f"{self.name} makes a sound"

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, "Canine")
        self.breed = breed
        self.tricks = []
    
    def make_sound(self):
        return f"{self.name} barks: Woof!"
    
    def learn_trick(self, trick):
        self.tricks.append(trick)
        return f"{self.name} learned {trick}"
    
    def perform_trick(self, trick):
        if trick in self.tricks:
            return f"{self.name} performs {trick}"
        return f"{self.name} doesn't know {trick}"

class Cat(Animal):
    def __init__(self, name, color):
        super().__init__(name, "Feline")
        self.color = color
    
    def make_sound(self):
        return f"{self.name} meows: Meow!"
    
    def purr(self):
        return f"{self.name} purrs contentedly"

# Usage
dog = Dog("Buddy", "Golden Retriever")
cat = Cat("Whiskers", "Orange")

print(dog.make_sound())  # Buddy barks: Woof!
print(cat.make_sound())  # Whiskers meows: Meow!
print(dog.eat("kibble"))  # Buddy ate kibble
print(cat.purr())  # Whiskers purrs contentedly
```

### 3.2 Method Overriding and Super()

```python
class Vehicle:
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.is_running = False
    
    def start(self):
        if not self.is_running:
            self.is_running = True
            return f"{self.make} {self.model} started"
        return f"{self.make} {self.model} is already running"
    
    def stop(self):
        if self.is_running:
            self.is_running = False
            return f"{self.make} {self.model} stopped"
        return f"{self.make} {self.model} is not running"
    
    def __str__(self):
        return f"{self.year} {self.make} {self.model}"

class Car(Vehicle):
    def __init__(self, make, model, year, num_doors):
        super().__init__(make, model, year)
        self.num_doors = num_doors
        self.gear = "Park"
    
    def start(self):
        # Override parent method with additional functionality
        result = super().start()
        if self.is_running:
            self.gear = "Drive"
            return f"{result} - Gear set to Drive"
        return result
    
    def shift_gear(self, new_gear):
        if self.is_running:
            self.gear = new_gear
            return f"Shifted to {new_gear}"
        return "Start the car first"

class Motorcycle(Vehicle):
    def __init__(self, make, model, year, engine_size):
        super().__init__(make, model, year)
        self.engine_size = engine_size
        self.kickstand_up = False
    
    def start(self):
        if not self.kickstand_up:
            return "Put the kickstand up first"
        return super().start()
    
    def kick_start(self):
        self.kickstand_up = True
        return self.start()

# Usage
car = Car("Honda", "Civic", 2022, 4)
motorcycle = Motorcycle("Harley", "Sportster", 2021, 883)

print(car.start())  # Honda Civic started - Gear set to Drive
print(motorcycle.start())  # Put the kickstand up first
print(motorcycle.kick_start())  # Harley Sportster started
```

### 3.3 Abstract Base Classes

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    def __init__(self, color):
        self.color = color
    
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimeter(self):
        pass
    
    def display_info(self):
        return f"Shape: {self.__class__.__name__}, Color: {self.color}"

class Rectangle(Shape):
    def __init__(self, color, width, height):
        super().__init__(color)
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, color, radius):
        super().__init__(color)
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def perimeter(self):
        return 2 * 3.14159 * self.radius

# Usage
rectangle = Rectangle("blue", 5, 3)
circle = Circle("red", 4)

print(rectangle.display_info())
print(f"Area: {rectangle.area()}")
print(f"Perimeter: {rectangle.perimeter()}")

print(circle.display_info())
print(f"Area: {circle.area():.2f}")
print(f"Perimeter: {circle.perimeter():.2f}")

# This would raise TypeError: Can't instantiate abstract class
# shape = Shape("green")
```

---

## 4. Polymorphism

Polymorphism allows objects of different types to be treated as instances of the same type through a common interface.

### 4.1 Method Polymorphism

```python
class Animal:
    def make_sound(self):
        pass

class Dog(Animal):
    def make_sound(self):
        return "Woof!"

class Cat(Animal):
    def make_sound(self):
        return "Meow!"

class Cow(Animal):
    def make_sound(self):
        return "Moo!"

# Polymorphic function
def animal_chorus(animals):
    for animal in animals:
        print(f"{animal.__class__.__name__} says: {animal.make_sound()}")

# Usage
animals = [Dog(), Cat(), Cow(), Dog()]
animal_chorus(animals)
```

### 4.2 Duck Typing

```python
class Duck:
    def quack(self):
        return "Quack!"
    
    def fly(self):
        return "Flying like a duck"

class Person:
    def quack(self):
        return "Person imitating a duck: Quack!"
    
    def fly(self):
        return "Person pretending to fly"

class Airplane:
    def fly(self):
        return "Flying like an airplane"

# Duck typing in action
def make_it_quack(duck_like):
    if hasattr(duck_like, 'quack'):
        return duck_like.quack()
    return "Can't quack"

def make_it_fly(flyable):
    if hasattr(flyable, 'fly'):
        return flyable.fly()
    return "Can't fly"

# Usage
duck = Duck()
person = Person()
airplane = Airplane()

print(make_it_quack(duck))    # Quack!
print(make_it_quack(person))  # Person imitating a duck: Quack!
print(make_it_quack(airplane))  # Can't quack

print(make_it_fly(duck))      # Flying like a duck
print(make_it_fly(person))    # Person pretending to fly
print(make_it_fly(airplane))  # Flying like an airplane
```

### 4.3 Operator Overloading

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __add__(self, other):
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        return NotImplemented
    
    def __sub__(self, other):
        if isinstance(other, Vector):
            return Vector(self.x - other.x, self.y - other.y)
        return NotImplemented
    
    def __mul__(self, scalar):
        if isinstance(scalar, (int, float)):
            return Vector(self.x * scalar, self.y * scalar)
        return NotImplemented
    
    def __rmul__(self, scalar):
        return self.__mul__(scalar)
    
    def __eq__(self, other):
        if isinstance(other, Vector):
            return self.x == other.x and self.y == other.y
        return False
    
    def __len__(self):
        return int((self.x ** 2 + self.y ** 2) ** 0.5)
    
    def magnitude(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5

# Usage
v1 = Vector(3, 4)
v2 = Vector(1, 2)

print(v1 + v2)  # Vector(4, 6)
print(v1 - v2)  # Vector(2, 2)
print(v1 * 2)   # Vector(6, 8)
print(3 * v1)   # Vector(9, 12)
print(v1 == v2) # False
print(len(v1))  # 5
```

### 4.4 Protocol-Based Polymorphism

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> str:
        ...

class Movable(Protocol):
    def move(self, x: float, y: float) -> None:
        ...

class Circle:
    def __init__(self, radius):
        self.radius = radius
        self.x = 0
        self.y = 0
    
    def draw(self):
        return f"Drawing circle with radius {self.radius}"
    
    def move(self, x, y):
        self.x += x
        self.y += y

class Square:
    def __init__(self, side):
        self.side = side
        self.x = 0
        self.y = 0
    
    def draw(self):
        return f"Drawing square with side {self.side}"
    
    def move(self, x, y):
        self.x += x
        self.y += y

def draw_shape(shape: Drawable):
    return shape.draw()

def move_shape(shape: Movable, x: float, y: float):
    shape.move(x, y)

# Usage
circle = Circle(5)
square = Square(4)

print(draw_shape(circle))  # Drawing circle with radius 5
print(draw_shape(square))  # Drawing square with side 4

move_shape(circle, 10, 20)
print(f"Circle position: ({circle.x}, {circle.y})")
```

---

## 5. Abstraction

Abstraction hides complex implementation details and shows only the necessary features of an object.

### 5.1 Abstract Classes and Methods

```python
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, amount: float) -> bool:
        pass
    
    @abstractmethod
    def refund_payment(self, transaction_id: str) -> bool:
        pass
    
    def log_transaction(self, transaction_type: str, amount: float):
        print(f"Transaction logged: {transaction_type} of ${amount}")

class CreditCardProcessor(PaymentProcessor):
    def __init__(self, merchant_id):
        self.merchant_id = merchant_id
    
    def process_payment(self, amount: float) -> bool:
        # Complex credit card processing logic
        print(f"Processing credit card payment of ${amount}")
        self.log_transaction("Credit Card Payment", amount)
        return True
    
    def refund_payment(self, transaction_id: str) -> bool:
        # Complex refund logic
        print(f"Refunding transaction {transaction_id}")
        self.log_transaction("Credit Card Refund", 0)
        return True

class PayPalProcessor(PaymentProcessor):
    def __init__(self, api_key):
        self.api_key = api_key
    
    def process_payment(self, amount: float) -> bool:
        # PayPal-specific processing
        print(f"Processing PayPal payment of ${amount}")
        self.log_transaction("PayPal Payment", amount)
        return True
    
    def refund_payment(self, transaction_id: str) -> bool:
        # PayPal refund logic
        print(f"Refunding PayPal transaction {transaction_id}")
        self.log_transaction("PayPal Refund", 0)
        return True

class PaymentService:
    def __init__(self, processor: PaymentProcessor):
        self._processor = processor
    
    def make_payment(self, amount: float):
        return self._processor.process_payment(amount)
    
    def refund(self, transaction_id: str):
        return self._processor.refund_payment(transaction_id)

# Usage
cc_processor = CreditCardProcessor("MERCHANT123")
paypal_processor = PayPalProcessor("API_KEY_456")

payment_service = PaymentService(cc_processor)
payment_service.make_payment(100.00)

payment_service = PaymentService(paypal_processor)
payment_service.make_payment(50.00)
```

### 5.2 Interface Segregation

```python
from abc import ABC, abstractmethod

# Instead of one large interface
class BadWorkerInterface(ABC):
    @abstractmethod
    def work(self):
        pass
    
    @abstractmethod
    def eat(self):
        pass
    
    @abstractmethod
    def sleep(self):
        pass

# Better: Segregated interfaces
class Workable(ABC):
    @abstractmethod
    def work(self):
        pass

class Eater(ABC):
    @abstractmethod
    def eat(self):
        pass

class Sleeper(ABC):
    @abstractmethod
    def sleep(self):
        pass

class Human(Workable, Eater, Sleeper):
    def work(self):
        return "Human working"
    
    def eat(self):
        return "Human eating"
    
    def sleep(self):
        return "Human sleeping"

class Robot(Workable):
    def work(self):
        return "Robot working"
    
    # Robot doesn't need to eat or sleep

# Usage
human = Human()
robot = Robot()

print(human.work())  # Human working
print(robot.work())  # Robot working
```

---

## 6. Advanced Inheritance Concepts

### 6.1 Multiple Inheritance

```python
class Flyable:
    def fly(self):
        return "Flying through the air"
    
    def land(self):
        return "Landing safely"

class Swimmable:
    def swim(self):
        return "Swimming in water"
    
    def dive(self):
        return "Diving underwater"

class Duck(Flyable, Swimmable):
    def __init__(self, name):
        self.name = name
    
    def quack(self):
        return f"{self.name} says quack!"
    
    def fly(self):
        return f"{self.name} is flying with wings"

class Penguin(Swimmable):
    def __init__(self, name):
        self.name = name
    
    def swim(self):
        return f"{self.name} is swimming like a torpedo"

# Usage
duck = Duck("Daffy")
penguin = Penguin("Pingu")

print(duck.fly())   # Daffy is flying with wings
print(duck.swim())  # Swimming in water
print(duck.quack()) # Daffy says quack!

print(penguin.swim())  # Pingu is swimming like a torpedo
print(penguin.dive())  # Diving underwater
```

### 6.2 Method Resolution Order (MRO)

```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):
    pass

class E(C, B):
    pass

# Check MRO
print(D.__mro__)
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

print(E.__mro__)
# (<class '__main__.E'>, <class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)

d = D()
e = E()

print(d.method())  # B (B comes before C in D's MRO)
print(e.method())  # C (C comes before B in E's MRO)
```

### 6.3 Cooperative Inheritance with super()

```python
class Animal:
    def __init__(self, name):
        self.name = name
        print(f"Animal.__init__ called for {name}")
    
    def speak(self):
        return f"{self.name} makes a sound"

class Mammal(Animal):
    def __init__(self, name, warm_blooded=True):
        super().__init__(name)
        self.warm_blooded = warm_blooded
        print(f"Mammal.__init__ called for {name}")
    
    def speak(self):
        return f"{self.name} makes a mammalian sound"

class Dog(Mammal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed
        print(f"Dog.__init__ called for {name}")
    
    def speak(self):
        return f"{self.name} barks"

class WorkingDog(Dog):
    def __init__(self, name, breed, job):
        super().__init__(name, breed)
        self.job = job
        print(f"WorkingDog.__init__ called for {name}")
    
    def work(self):
        return f"{self.name} is working as a {self.job}"

# Usage
working_dog = WorkingDog("Rex", "German Shepherd", "Police Dog")
print(working_dog.speak())
print(working_dog.work())
```

---

## 7. Multiple Inheritance and Method Resolution Order

### 7.1 Diamond Problem and MRO

```python
class Base:
    def method(self):
        print("Base.method")

class Left(Base):
    def method(self):
        print("Left.method")
        super().method()

class Right(Base):
    def method(self):
        print("Right.method")
        super().method()

class Child(Left, Right):
    def method(self):
        print("Child.method")
        super().method()

# MRO resolves the diamond problem
child = Child()
child.method()
# Output:
# Child.method
# Left.method
# Right.method
# Base.method

print(Child.__mro__)
# (<class '__main__.Child'>, <class '__main__.Left'>, <class '__main__.Right'>, <class '__main__.Base'>, <class 'object'>)
```

### 7.2 Mixins

```python
class TimestampMixin:
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        from datetime import datetime
        self.created_at = datetime.now()
        self.updated_at = datetime.now()
    
    def update_timestamp(self):
        from datetime import datetime
        self.updated_at = datetime.now()

class SerializableMixin:
    def to_dict(self):
        return {
            key: value for key, value in self.__dict__.items()
            if not key.startswith('_')
        }
    
    def from_dict(self, data):
        for key, value in data.items():
            if hasattr(self, key):
                setattr(self, key, value)

class ValidatableMixin:
    def validate(self):
        errors = []
        for attr_name, attr_value in self.__dict__.items():
            if not attr_name.startswith('_'):
                validator_name = f"validate_{attr_name}"
                if hasattr(self, validator_name):
                    validator = getattr(self, validator_name)
                    if not validator(attr_value):
                        errors.append(f"Invalid {attr_name}: {attr_value}")
        return errors

class User(TimestampMixin, SerializableMixin, ValidatableMixin):
    def __init__(self, username, email):
        self.username = username
        self.email = email
        super().__init__()
    
    def validate_username(self, username):
        return len(username) >= 3
    
    def validate_email(self, email):
        return '@' in email

# Usage
user = User("john_doe", "john@example.com")
print(user.to_dict())
print(user.validate())  # []

user.email = "invalid_email"
print(user.validate())  # ['Invalid email: invalid_email']
```

---

## 8. Design Patterns and OOP

### 8.1 Singleton Pattern

```python
class Singleton:
    _instance = None
    _initialized = False
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
    
    def __init__(self):
        if not self._initialized:
            self._initialized = True
            self.value = 0
    
    def increment(self):
        self.value += 1
    
    def get_value(self):
        return self.value

# Usage
s1 = Singleton()
s2 = Singleton()

print(s1 is s2)  # True - same instance

s1.increment()
print(s2.get_value())  # 1 - shared state
```

### 8.2 Factory Pattern

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def make_sound(self):
        pass

class Dog(Animal):
    def make_sound(self):
        return "Woof!"

class Cat(Animal):
    def make_sound(self):
        return "Meow!"

class Cow(Animal):
    def make_sound(self):
        return "Moo!"

class AnimalFactory:
    @staticmethod
    def create_animal(animal_type):
        if animal_type.lower() == "dog":
            return Dog()
        elif animal_type.lower() == "cat":
            return Cat()
        elif animal_type.lower() == "cow":
            return Cow()
        else:
            raise ValueError(f"Unknown animal type: {animal_type}")

# Usage
factory = AnimalFactory()
dog = factory.create_animal("dog")
cat = factory.create_animal("cat")

print(dog.make_sound())  # Woof!
print(cat.make_sound())  # Meow!
```

### 8.3 Observer Pattern

```python
class Subject:
    def __init__(self):
        self._observers = []
    
    def attach(self, observer):
        self._observers.append(observer)
    
    def detach(self, observer):
        self._observers.remove(observer)
    
    def notify(self, message):
        for observer in self._observers:
            observer.update(message)

class Observer:
    def __init__(self, name):
        self.name = name
    
    def update(self, message):
        print(f"{self.name} received: {message}")

class NewsAgency(Subject):
    def __init__(self):
        super().__init__()
        self._news = None
    
    def set_news(self, news):
        self._news = news
        self.notify(news)
    
    def get_news(self):
        return self._news

# Usage
agency = NewsAgency()
observer1 = Observer("Channel 1")
observer2 = Observer("Channel 2")

agency.attach(observer1)
agency.attach(observer2)

agency.set_news("Breaking: New Python version released!")
# Output:
# Channel 1 received: Breaking: New Python version released!
# Channel 2 received: Breaking: New Python version released!
```

---

## 9. SOLID Principles

### 9.1 Single Responsibility Principle (SRP)

```python
# Bad: Multiple responsibilities
class BadUser:
    def __init__(self, username, email):
        self.username = username
        self.email = email
    
    def save_to_database(self):
        # Database logic
        pass
    
    def send_email(self):
        # Email logic
        pass
    
    def validate_email(self):
        # Validation logic
        pass

# Good: Single responsibility
class User:
    def __init__(self, username, email):
        self.username = username
        self.email = email

class UserRepository:
    def save(self, user):
        # Database logic
        pass

class EmailService:
    def send_email(self, user, message):
        # Email logic
        pass

class UserValidator:
    def validate_email(self, email):
        # Validation logic
        return '@' in email
```

### 9.2 Open/Closed Principle (OCP)

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2

class AreaCalculator:
    def calculate_total_area(self, shapes):
        return sum(shape.area() for shape in shapes)

# Adding new shapes doesn't require modifying existing code
class Triangle(Shape):
    def __init__(self, base, height):
        self.base = base
        self.height = height
    
    def area(self):
        return 0.5 * self.base * self.height
```

### 9.3 Liskov Substitution Principle (LSP)

```python
class Bird:
    def fly(self):
        return "Flying"

class Sparrow(Bird):
    def fly(self):
        return "Sparrow flying"

class Ostrich(Bird):
    def fly(self):
        # Violates LSP - Ostrich can't fly
        raise NotImplementedError("Ostriches can't fly")

# Better design
class Bird:
    def move(self):
        return "Moving"

class FlyingBird(Bird):
    def fly(self):
        return "Flying"

class Sparrow(FlyingBird):
    def fly(self):
        return "Sparrow flying"

class Ostrich(Bird):
    def run(self):
        return "Ostrich running"
```

### 9.4 Interface Segregation Principle (ISP)

```python
from abc import ABC, abstractmethod

# Bad: Fat interface
class BadWorker(ABC):
    @abstractmethod
    def work(self):
        pass
    
    @abstractmethod
    def eat(self):
        pass

# Good: Segregated interfaces
class Workable(ABC):
    @abstractmethod
    def work(self):
        pass

class Eater(ABC):
    @abstractmethod
    def eat(self):
        pass

class Human(Workable, Eater):
    def work(self):
        return "Human working"
    
    def eat(self):
        return "Human eating"

class Robot(Workable):
    def work(self):
        return "Robot working"
```

### 9.5 Dependency Inversion Principle (DIP)

```python
from abc import ABC, abstractmethod

# High-level module depends on abstraction
class NotificationService(ABC):
    @abstractmethod
    def send(self, message):
        pass

class EmailService(NotificationService):
    def send(self, message):
        return f"Email sent: {message}"

class SMSService(NotificationService):
    def send(self, message):
        return f"SMS sent: {message}"

class OrderService:
    def __init__(self, notification_service: NotificationService):
        self._notification_service = notification_service
    
    def process_order(self, order):
        # Process order logic
        result = self._notification_service.send(f"Order {order} processed")
        return result

# Usage
email_service = EmailService()
sms_service = SMSService()

order_service = OrderService(email_service)
print(order_service.process_order("12345"))

order_service = OrderService(sms_service)
print(order_service.process_order("12345"))
```

---

## 10. Practical Examples

### 10.1 E-commerce System

```python
from abc import ABC, abstractmethod
from datetime import datetime
from typing import List

class Product:
    def __init__(self, product_id, name, price, category):
        self.product_id = product_id
        self.name = name
        self.price = price
        self.category = category
    
    def __str__(self):
        return f"{self.name} - ${self.price:.2f}"

class CartItem:
    def __init__(self, product, quantity):
        self.product = product
        self.quantity = quantity
    
    @property
    def total_price(self):
        return self.product.price * self.quantity

class ShoppingCart:
    def __init__(self):
        self.items = []
    
    def add_item(self, product, quantity=1):
        for item in self.items:
            if item.product.product_id == product.product_id:
                item.quantity += quantity
                return
        self.items.append(CartItem(product, quantity))
    
    def remove_item(self, product_id):
        self.items = [item for item in self.items if item.product.product_id != product_id]
    
    @property
    def total_price(self):
        return sum(item.total_price for item in self.items)

class PaymentMethod(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass

class CreditCard(PaymentMethod):
    def __init__(self, card_number, cvv):
        self.card_number = card_number
        self.cvv = cvv
    
    def process_payment(self, amount):
        return f"Credit card payment of ${amount:.2f} processed"

class PayPal(PaymentMethod):
    def __init__(self, email):
        self.email = email
    
    def process_payment(self, amount):
        return f"PayPal payment of ${amount:.2f} processed for {self.email}"

class Order:
    def __init__(self, order_id, customer, cart, payment_method):
        self.order_id = order_id
        self.customer = customer
        self.cart = cart
        self.payment_method = payment_method
        self.status = "pending"
        self.created_at = datetime.now()
    
    def process(self):
        payment_result = self.payment_method.process_payment(self.cart.total_price)
        self.status = "processed"
        return payment_result

class Customer:
    def __init__(self, customer_id, name, email):
        self.customer_id = customer_id
        self.name = name
        self.email = email
        self.orders = []
    
    def place_order(self, cart, payment_method):
        order_id = f"ORD{len(self.orders) + 1:04d}"
        order = Order(order_id, self, cart, payment_method)
        self.orders.append(order)
        return order

# Usage
customer = Customer("C001", "John Doe", "john@example.com")
cart = ShoppingCart()

product1 = Product("P001", "Laptop", 999.99, "Electronics")
product2 = Product("P002", "Mouse", 29.99, "Electronics")

cart.add_item(product1)
cart.add_item(product2, 2)

payment_method = CreditCard("1234-5678-9012-3456", "123")
order = customer.place_order(cart, payment_method)

print(order.process())
print(f"Order total: ${order.cart.total_price:.2f}")
```

### 10.2 Document Management System

```python
from abc import ABC, abstractmethod
from datetime import datetime
from typing import List

class Document(ABC):
    def __init__(self, title, content, author):
        self.title = title
        self.content = content
        self.author = author
        self.created_at = datetime.now()
        self.modified_at = datetime.now()
    
    @abstractmethod
    def render(self):
        pass
    
    def update_content(self, new_content):
        self.content = new_content
        self.modified_at = datetime.now()

class TextDocument(Document):
    def render(self):
        return f"Text Document: {self.title}\n{self.content}"

class HTMLDocument(Document):
    def render(self):
        return f"<html><head><title>{self.title}</title></head><body>{self.content}</body></html>"

class PDFDocument(Document):
    def render(self):
        return f"PDF Document: {self.title}\n[PDF Content: {self.content}]"

class DocumentProcessor(ABC):
    @abstractmethod
    def process(self, document):
        pass

class TextProcessor(DocumentProcessor):
    def process(self, document):
        return f"Processing text: {document.content[:50]}..."

class HTMLProcessor(DocumentProcessor):
    def process(self, document):
        return f"Processing HTML: {document.title}"

class DocumentManager:
    def __init__(self):
        self.documents = []
        self.processors = {
            TextDocument: TextProcessor(),
            HTMLDocument: HTMLProcessor()
        }
    
    def add_document(self, document):
        self.documents.append(document)
    
    def process_document(self, document):
        processor = self.processors.get(type(document))
        if processor:
            return processor.process(document)
        return "No processor available for this document type"
    
    def search_documents(self, query):
        results = []
        for doc in self.documents:
            if query.lower() in doc.title.lower() or query.lower() in doc.content.lower():
                results.append(doc)
        return results

# Usage
manager = DocumentManager()

text_doc = TextDocument("Report", "This is a quarterly report...", "John")
html_doc = HTMLDocument("Website", "<h1>Welcome</h1><p>Content here</p>", "Jane")

manager.add_document(text_doc)
manager.add_document(html_doc)

print(manager.process_document(text_doc))
print(manager.process_document(html_doc))

search_results = manager.search_documents("report")
for doc in search_results:
    print(f"Found: {doc.title}")
```

---

## 11. Best Practices

### 11.1 Design Guidelines

1. **Favor composition over inheritance**
2. **Use abstract base classes for contracts**
3. **Keep inheritance hierarchies shallow**
4. **Follow the SOLID principles**
5. **Use meaningful names for classes and methods**

### 11.2 Code Organization

```python
# Good class organization
class WellOrganizedClass:
    """Class with clear documentation and organization"""
    
    # Class constants
    DEFAULT_VALUE = 42
    
    def __init__(self, value):
        """Initialize with proper validation"""
        if not isinstance(value, int):
            raise TypeError("Value must be an integer")
        self._value = value
    
    def __str__(self):
        """Human-readable representation"""
        return f"WellOrganizedClass({self._value})"
    
    def __repr__(self):
        """Developer-friendly representation"""
        return f"WellOrganizedClass(value={self._value})"
    
    # Properties
    @property
    def value(self):
        return self._value
    
    @value.setter
    def value(self, new_value):
        if not isinstance(new_value, int):
            raise TypeError("Value must be an integer")
        self._value = new_value
    
    # Public methods
    def calculate(self):
        """Public method with clear documentation"""
        return self._internal_calculation()
    
    # Private methods
    def _internal_calculation(self):
        """Private method for internal use"""
        return self._value * 2
```

### 11.3 Error Handling

```python
class SafeClass:
    def __init__(self, value):
        self.value = self._validate_value(value)
    
    def _validate_value(self, value):
        if not isinstance(value, (int, float)):
            raise TypeError(f"Expected int or float, got {type(value).__name__}")
        if value < 0:
            raise ValueError("Value must be non-negative")
        return value
    
    def divide(self, divisor):
        if divisor == 0:
            raise ValueError("Cannot divide by zero")
        return self.value / divisor

# Usage with proper error handling
try:
    obj = SafeClass(10)
    result = obj.divide(2)
    print(f"Result: {result}")
except (TypeError, ValueError) as e:
    print(f"Error: {e}")
```

---

## 12. Common Antipatterns

### 12.1 God Object

```python
# BAD: God object doing too much
class BadUserManager:
    def __init__(self):
        self.users = []
    
    def add_user(self, user):
        pass
    
    def remove_user(self, user):
        pass
    
    def send_email(self, user, message):
        pass
    
    def validate_email(self, email):
        pass
    
    def encrypt_password(self, password):
        pass
    
    def log_activity(self, activity):
        pass

# GOOD: Separated concerns
class UserRepository:
    def add_user(self, user):
        pass
    
    def remove_user(self, user):
        pass

class EmailService:
    def send_email(self, user, message):
        pass

class UserValidator:
    def validate_email(self, email):
        pass
```

### 12.2 Inappropriate Inheritance

```python
# BAD: Inheritance for code reuse only
class BadSquare(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)
    
    def set_width(self, width):
        super().set_width(width)
        super().set_height(width)  # Violates LSP

# GOOD: Composition or proper hierarchy
class Shape:
    def area(self):
        pass

class Square(Shape):
    def __init__(self, side):
        self.side = side
    
    def area(self):
        return self.side ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
```

---

## 13. Review Questions

1. **What are the four pillars of OOP, and how do they relate to each other?**

2. **Explain the difference between method overriding and method overloading. How does Python handle each?**

3. **What is the Method Resolution Order (MRO), and why is it important in multiple inheritance?**

4. **When should you use composition instead of inheritance? Provide examples.**

5. **Explain the SOLID principles and how they apply to Python programming.**

6. **What is duck typing, and how does it relate to polymorphism in Python?**

7. **How do abstract base classes enforce contracts in Python?**

8. **What are mixins, and how do they differ from regular inheritance?**

9. **Explain the difference between `__str__` and `__repr__` methods in the context of polymorphism.**

10. **How does the `super()` function work in multiple inheritance scenarios?**

11. **What are the advantages and disadvantages of multiple inheritance?**

12. **How do you implement the Singleton pattern in Python, and what are its use cases?**

13. **What is the difference between is-a and has-a relationships in OOP?**

14. **How do you handle the diamond problem in multiple inheritance?**

15. **What are some common antipatterns in OOP, and how can they be avoided?**

---

## 14. Answers to Review Questions

1. **What are the four pillars of OOP, and how do they relate to each other?**
   - **Encapsulation**: Bundling data and methods, controlling access
   - **Inheritance**: Creating new classes based on existing ones
   - **Polymorphism**: One interface, multiple implementations
   - **Abstraction**: Hiding complex implementation details
   
   They work together: Encapsulation protects data, Inheritance promotes code reuse, Polymorphism enables flexible interfaces, and Abstraction simplifies complex systems.

2. **Explain the difference between method overriding and method overloading. How does Python handle each?**
   - **Method overriding**: Subclass provides different implementation of parent method
   - **Method overloading**: Multiple methods with same name but different parameters
   
   Python supports overriding naturally. For overloading, Python uses default parameters or `*args`/`**kwargs` since it doesn't support true method overloading.

3. **What is the Method Resolution Order (MRO), and why is it important in multiple inheritance?**
   - MRO determines the order in which methods are searched in inheritance hierarchy
   - Uses C3 linearization algorithm to resolve conflicts
   - Important for avoiding ambiguity in multiple inheritance scenarios
   - Accessible via `ClassName.__mro__` or `ClassName.mro()`

4. **When should you use composition instead of inheritance? Provide examples.**
   - Use composition when:
     - Relationship is "has-a" rather than "is-a"
     - You need flexible, runtime relationships
     - You want to avoid tight coupling
   
   Example: Car has Engine (composition) vs Car is Vehicle (inheritance)

5. **Explain the SOLID principles and how they apply to Python programming.**
   - **S**ingle Responsibility: One class, one reason to change
   - **O**pen/Closed: Open for extension, closed for modification
   - **L**iskov Substitution: Subclasses must be substitutable for base classes
   - **I**nterface Segregation: Many specific interfaces vs one general interface
   - **D**ependency Inversion: Depend on abstractions, not concretions

6. **What is duck typing, and how does it relate to polymorphism in Python?**
   - Duck typing: "If it walks like a duck and quacks like a duck, it's a duck"
   - Objects are used based on their behavior, not their type
   - Enables polymorphism without explicit inheritance
   - Python checks for method existence at runtime, not compile time

7. **How do abstract base classes enforce contracts in Python?**
   - ABC module provides `@abstractmethod` decorator
   - Prevents instantiation of incomplete classes
   - Ensures subclasses implement required methods
   - Provides documentation of expected interface

8. **What are mixins, and how do they differ from regular inheritance?**
   - Mixins are classes designed to be mixed into other classes
   - Provide reusable functionality without creating "is-a" relationships
   - Usually don't stand alone as complete classes
   - Enable horizontal code reuse across unrelated hierarchies

9. **Explain the difference between `__str__` and `__repr__` methods in the context of polymorphism.**
   - `__str__`: User-friendly representation (readable)
   - `__repr__`: Developer-friendly representation (unambiguous)
   - Both enable polymorphic behavior for string conversion
   - Different objects can have different string representations while sharing the same interface

10. **How does the `super()` function work in multiple inheritance scenarios?**
    - `super()` follows MRO to find next method in chain
    - Enables cooperative inheritance
    - Ensures all parent methods are called exactly once
    - Critical for proper multiple inheritance behavior

11. **What are the advantages and disadvantages of multiple inheritance?**
    - Advantages: Code reuse, modeling complex relationships, mixins
    - Disadvantages: Complexity, diamond problem, ambiguity, maintenance difficulty
    - Best practices: Use mixins, keep hierarchies shallow, prefer composition

12. **How do you implement the Singleton pattern in Python, and what are its use cases?**
    - Override `__new__` method to control instance creation
    - Store single instance in class variable
    - Use cases: Database connections, logging, configuration settings
    - Consider thread safety in concurrent environments

13. **What is the difference between is-a and has-a relationships in OOP?**
    - **Is-a**: Inheritance relationship (Dog is-a Animal)
    - **Has-a**: Composition relationship (Car has-a Engine)
    - Is-a creates type hierarchy, has-a creates object collaboration
    - Choose based on whether substitutability makes sense

14. **How do you handle the diamond problem in multiple inheritance?**
    - Python's MRO using C3 linearization automatically resolves conflicts
    - Use `super()` for cooperative inheritance
    - Design mixins carefully to avoid conflicts
    - Consider composition as alternative to complex inheritance

15. **What are some common antipatterns in OOP, and how can they be avoided?**
    - **God Object**: Break into smaller, focused classes
    - **Inappropriate Inheritance**: Use composition when appropriate
    - **Tight Coupling**: Use dependency injection and interfaces
    - **Circular Dependencies**: Redesign relationships, use weak references
    - **Premature Optimization**: Focus on clear design first

---

*This completes the OOP Principles module. These concepts are fundamental to designing robust, maintainable object-oriented systems in Python and form the basis for more advanced design patterns and architectural concepts.*
