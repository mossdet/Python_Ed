# OOP Principles: Encapsulation, Inheritance, Polymorphism, and Abstraction

## Table of Contents
- [Encapsulation and Data Hiding](#encapsulation-and-data-hiding)
- [Inheritance and Method Overriding](#inheritance-and-method-overriding)
- [Polymorphism](#polymorphism)
- [Abstraction and Abstract Classes](#abstraction-and-abstract-classes)
- [Advanced OOP Concepts](#advanced-oop-concepts)
- [Real-World Applications](#real-world-applications)
- [Exercises](#exercises)

---

## Encapsulation and Data Hiding

### Theoretical Foundations

Encapsulation represents a fundamental principle of object-oriented programming that stems from the concept of **Abstract Data Types (ADTs)** and **information hiding** as formalized by David Parnas (1972). From a theoretical perspective, encapsulation serves multiple purposes:

1. **Modularity**: It enables the decomposition of complex systems into manageable, self-contained units that can be developed, tested, and maintained independently.

2. **Information Hiding**: It enforces the principle that implementation details should be hidden from clients, allowing for **representation independence** - the ability to change internal representations without affecting external interfaces.

3. **Invariant Preservation**: Encapsulation provides mechanisms to maintain **class invariants** - conditions that must always be true for instances of a class, ensuring system consistency and correctness.

### Formal Semantics of Encapsulation

From a formal methods perspective, encapsulation can be understood through the lens of **algebraic specifications** and **operational semantics**:

- **State Abstraction**: The internal state of an object is abstracted through a **state space** S, where each state s ∈ S represents a valid configuration of the object's attributes.

- **Operation Semantics**: Each method m defines a partial function f_m: S → S that transforms the object's state while preserving invariants.

- **Access Control**: The visibility of attributes and methods is formalized through **access predicates** that determine whether a given operation can be performed by a specific client.

### Encapsulation in Python: Design Philosophy

Python's approach to encapsulation follows the **"We are all consenting adults"** philosophy, emphasizing convention over enforcement. This design choice reflects several important considerations:

1. **Pragmatic Flexibility**: Unlike languages with strict access control (C++, Java), Python allows developers to access internal state when necessary, supporting rapid prototyping and debugging.

2. **Cultural Conventions**: The naming conventions serve as **social contracts** that communicate intent rather than enforcing technical barriers.

3. **Metaprogramming Support**: Python's dynamic nature requires flexible access patterns to support advanced metaprogramming techniques.

### Python Access Modifiers and Name Mangling

Python uses naming conventions to indicate access levels, implementing a **lexical scoping** mechanism through name mangling:

- **Public attributes** (no underscore): Form the **public interface** of the class - the contract that defines how clients should interact with objects
- **Protected attributes** (single underscore): Indicate **implementation details** subject to change, following the **open-closed principle**
- **Private attributes** (double underscore): Trigger Python's **name mangling** mechanism, implementing a form of **namespace isolation**

#### Name Mangling: Implementation Details

Python's name mangling transforms `__attribute` to `_ClassName__attribute`, providing:

1. **Namespace Collision Avoidance**: Prevents accidental conflicts in complex inheritance hierarchies
2. **Intentional Access Friction**: Creates deliberate barriers to accessing implementation details
3. **Lexical Scoping**: Ensures attribute names are resolved within their defining class context

The mangling algorithm can be formally described as:
```
mangle(attr_name, class_name) = {
    "_" + class_name + attr_name  if attr_name starts with "__"
    attr_name                     otherwise
}
```

```python
class BankAccount:
    def __init__(self, account_number, balance=0):
        self.account_number = account_number    # Public
        self._balance = balance                 # Protected
        self.__pin = "1234"                    # Private
    
    def deposit(self, amount):
        if amount > 0:
            self._balance += amount
            return f"Deposited ${amount}. New balance: ${self._balance}"
        return "Invalid deposit amount"
    
    def withdraw(self, amount):
        if 0 < amount <= self._balance:
            self._balance -= amount
            return f"Withdrew ${amount}. New balance: ${self._balance}"
        return "Insufficient funds"
    
    def get_balance(self):
        return self._balance
    
    def verify_pin(self, pin):
        return pin == self.__pin
    
    def change_pin(self, old_pin, new_pin):
        if self.verify_pin(old_pin):
            self.__pin = new_pin
            return "PIN changed successfully"
        return "Invalid PIN"

# Usage
account = BankAccount("123456789", 1000)

# Public access
print(account.account_number)  # 123456789

# Protected access (convention - should be used carefully)
print(account._balance)  # 1000

# Private access - will raise AttributeError
try:
    print(account.__pin)  # AttributeError
except AttributeError:
    print("Cannot access private attribute directly")

# Correct way to access private data
print(account.verify_pin("1234"))  # True
print(account.change_pin("1234", "5678"))  # PIN changed successfully
```

### Property Decorators

#### Theoretical Foundation

Property decorators represent a sophisticated implementation of the **Descriptor Protocol** in Python, providing a declarative approach to computed attributes and controlled access patterns. From a theoretical perspective, properties implement a form of **aspect-oriented programming** where cross-cutting concerns (validation, logging, caching) are separated from core business logic.

**Formal Definition**: A property is a special kind of attribute that uses the descriptor protocol to intercept attribute access. Mathematically, a property P can be defined as:

```
P = (getter: () → T, setter: (T) → Unit, deleter: () → Unit)
```

Where:
- `getter`: A function that computes or retrieves the attribute value
- `setter`: A function that validates and stores the attribute value
- `deleter`: A function that handles attribute deletion

#### Descriptor Protocol and Computational Semantics

The descriptor protocol provides a low-level mechanism for customizing attribute access. When Python encounters attribute access `obj.attr`, it follows this resolution order:

1. **Type Dictionary Check**: Search in `type(obj).__dict__` for a descriptor
2. **Instance Dictionary Check**: Search in `obj.__dict__` for the attribute
3. **Class Hierarchy Search**: Use MRO to find the attribute
4. **Attribute Error**: Raise AttributeError if not found

```python
class Descriptor:
    def __get__(self, obj, owner):
        """Called when attribute is accessed"""
        pass
    
    def __set__(self, obj, value):
        """Called when attribute is set"""
        pass
    
    def __delete__(self, obj):
        """Called when attribute is deleted"""
        pass
```

#### Computed Properties and Invariant Preservation

Properties excel at implementing **computed attributes** that maintain class invariants while providing a natural attribute-like interface. This aligns with the principle of **uniform access** where clients cannot distinguish between stored and computed attributes.

Properties provide a way to customize access to instance attributes, allowing you to implement getters, setters, and deleters while maintaining encapsulation boundaries.

```python
class Temperature:
    """
    Temperature class demonstrating property decorators with invariant preservation.
    
    Invariants:
    - All temperature values must be above absolute zero in their respective scales
    - Internal representation maintains consistency across scale conversions
    - Validation ensures thermodynamic constraints are respected
    """
    
    def __init__(self, celsius=0):
        # Use property setter to ensure validation on initialization
        self.celsius = celsius
    
    @property
    def celsius(self):
        """
        Getter for celsius temperature.
        
        Returns:
            float: Temperature in Celsius
            
        Complexity: O(1)
        """
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """
        Setter for celsius with thermodynamic validation.
        
        Implements constraint: T_celsius >= -273.15
        
        Args:
            value (float): Temperature in Celsius
            
        Raises:
            ValueError: If temperature is below absolute zero
            
        Complexity: O(1)
        """
        if not isinstance(value, (int, float)):
            raise TypeError("Temperature must be a number")
        if value < -273.15:
            raise ValueError("Temperature cannot be below absolute zero (-273.15°C)")
        self._celsius = float(value)
    
    @property
    def fahrenheit(self):
        """
        Computed property for Fahrenheit temperature.
        
        Mathematical transformation: F = (C × 9/5) + 32
        
        Returns:
            float: Temperature in Fahrenheit
            
        Complexity: O(1)
        """
        return (self._celsius * 9/5) + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        """
        Setter for Fahrenheit with inverse transformation.
        
        Mathematical transformation: C = (F - 32) × 5/9
        Implements constraint: T_fahrenheit >= -459.67
        
        Args:
            value (float): Temperature in Fahrenheit
            
        Raises:
            ValueError: If temperature is below absolute zero
            
        Complexity: O(1)
        """
        if not isinstance(value, (int, float)):
            raise TypeError("Temperature must be a number")
        if value < -459.67:
            raise ValueError("Temperature cannot be below absolute zero (-459.67°F)")
        self._celsius = (value - 32) * 5/9
    
    @property
    def kelvin(self):
        """
        Computed property for Kelvin temperature.
        
        Mathematical transformation: K = C + 273.15
        
        Returns:
            float: Temperature in Kelvin
            
        Complexity: O(1)
        """
        return self._celsius + 273.15
    
    @kelvin.setter
    def kelvin(self, value):
        """
        Setter for Kelvin with inverse transformation.
        
        Mathematical transformation: C = K - 273.15
        Implements constraint: T_kelvin >= 0
        
        Args:
            value (float): Temperature in Kelvin
            
        Raises:
            ValueError: If temperature is below absolute zero
            
        Complexity: O(1)
        """
        if not isinstance(value, (int, float)):
            raise TypeError("Temperature must be a number")
        if value < 0:
            raise ValueError("Temperature cannot be below absolute zero (0K)")
        self._celsius = value - 273.15
    
    def __str__(self):
        """String representation showing all three scales."""
        return f"{self._celsius:.1f}°C ({self.fahrenheit:.1f}°F, {self.kelvin:.1f}K)"
    
    def __repr__(self):
        """Developer representation."""
        return f"Temperature({self._celsius})"
    
    def __eq__(self, other):
        """Equality comparison based on Celsius value."""
        if not isinstance(other, Temperature):
            return NotImplemented
        return abs(self._celsius - other._celsius) < 1e-10  # Floating point tolerance

# Usage Examples with Error Handling
temp = Temperature(25)
print(temp)  # 25.0°C (77.0°F, 298.1K)

# Property setters with validation
temp.fahrenheit = 100
print(f"After setting Fahrenheit to 100: {temp.celsius:.2f}°C")

temp.kelvin = 300
print(f"After setting Kelvin to 300: {temp.celsius:.2f}°C")

# Validation in action
try:
    temp.celsius = -300  # Below absolute zero
except ValueError as e:
    print(f"Validation Error: {e}")

# Type checking
try:
    temp.celsius = "hot"  # Invalid type
except TypeError as e:
    print(f"Type Error: {e}")
```

#### Advanced Property Patterns

```python
class CachedProperty:
    """
    Custom descriptor implementing lazy evaluation with caching.
    
    Demonstrates advanced descriptor protocol usage for performance optimization.
    """
    
    def __init__(self, func):
        self.func = func
        self.name = func.__name__
        self.__doc__ = func.__doc__
    
    def __get__(self, obj, owner):
        if obj is None:
            return self
        
        # Check if value is already cached
        cache_attr = f"_cached_{self.name}"
        if hasattr(obj, cache_attr):
            return getattr(obj, cache_attr)
        
        # Compute and cache the value
        value = self.func(obj)
        setattr(obj, cache_attr, value)
        return value
    
    def __set__(self, obj, value):
        # Invalidate cache when value is set
        cache_attr = f"_cached_{self.name}"
        if hasattr(obj, cache_attr):
            delattr(obj, cache_attr)
        setattr(obj, f"_{self.name}", value)
    
    def __delete__(self, obj):
        # Clear cache when property is deleted
        cache_attr = f"_cached_{self.name}"
        if hasattr(obj, cache_attr):
            delattr(obj, cache_attr)
        private_attr = f"_{self.name}"
        if hasattr(obj, private_attr):
            delattr(obj, private_attr)

class ExpensiveComputation:
    """Example class demonstrating cached properties."""
    
    def __init__(self, n):
        self.n = n
    
    @CachedProperty
    def factorial(self):
        """Expensive computation that should be cached."""
        print(f"Computing factorial of {self.n}...")
        result = 1
        for i in range(1, self.n + 1):
            result *= i
        return result
    
    @CachedProperty
    def fibonacci(self):
        """Another expensive computation."""
        print(f"Computing Fibonacci of {self.n}...")
        if self.n <= 1:
            return self.n
        a, b = 0, 1
        for _ in range(2, self.n + 1):
            a, b = b, a + b
        return b
```

### Advanced Encapsulation Example

```python
class SecureDatabase:
    def __init__(self, db_name):
        self._db_name = db_name
        self.__data = {}
        self.__access_log = []
        self.__max_login_attempts = 3
        self.__login_attempts = 0
        self.__is_locked = False
        self.__admin_password = "admin123"
    
    @property
    def db_name(self):
        return self._db_name
    
    @property
    def is_locked(self):
        return self.__is_locked
    
    def authenticate(self, password):
        if self.__is_locked:
            return False, "Database is locked due to too many failed attempts"
        
        if password == self.__admin_password:
            self.__login_attempts = 0
            self.__access_log.append(f"Successful login at {self._get_timestamp()}")
            return True, "Authentication successful"
        else:
            self.__login_attempts += 1
            if self.__login_attempts >= self.__max_login_attempts:
                self.__is_locked = True
                self.__access_log.append(f"Database locked at {self._get_timestamp()}")
                return False, "Database locked due to too many failed attempts"
            return False, f"Invalid password. {self.__max_login_attempts - self.__login_attempts} attempts remaining"
    
    def insert_data(self, key, value, password):
        success, message = self.authenticate(password)
        if success:
            self.__data[key] = value
            self.__access_log.append(f"Data inserted: {key} at {self._get_timestamp()}")
            return f"Data inserted successfully"
        return message
    
    def get_data(self, key, password):
        success, message = self.authenticate(password)
        if success:
            self.__access_log.append(f"Data accessed: {key} at {self._get_timestamp()}")
            return self.__data.get(key, "Key not found")
        return message
    
    def get_access_log(self, password):
        success, message = self.authenticate(password)
        if success:
            return self.__access_log.copy()
        return message
    
    def unlock_database(self, master_key):
        if master_key == "master_unlock_2023":
            self.__is_locked = False
            self.__login_attempts = 0
            self.__access_log.append(f"Database unlocked at {self._get_timestamp()}")
            return "Database unlocked successfully"
        return "Invalid master key"
    
    def _get_timestamp(self):
        from datetime import datetime
        return datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    def __str__(self):
        status = "LOCKED" if self.__is_locked else "ACTIVE"
        return f"Database '{self._db_name}' - Status: {status}"

# Usage
db = SecureDatabase("UserData")

# Successful operations
result = db.insert_data("user1", {"name": "John", "email": "john@example.com"}, "admin123")
print(result)  # Data inserted successfully

data = db.get_data("user1", "admin123")
print(data)  # {'name': 'John', 'email': 'john@example.com'}

# Failed authentication attempts
print(db.insert_data("user2", {"name": "Jane"}, "wrong_password"))
print(db.insert_data("user2", {"name": "Jane"}, "wrong_password"))
print(db.insert_data("user2", {"name": "Jane"}, "wrong_password"))  # Database locked

# Try to access locked database
print(db.get_data("user1", "admin123"))  # Database is locked

# Unlock database
print(db.unlock_database("master_unlock_2023"))  # Database unlocked successfully

# Access log
log = db.get_access_log("admin123")
for entry in log:
    print(entry)
```

---

## Inheritance and Method Overriding

#### Theoretical Foundation

Inheritance represents a fundamental relationship in object-oriented programming that implements the **"is-a"** relationship and enables **behavioral subtyping**. From a theoretical perspective, inheritance establishes a **subtype relation** where derived classes can be substituted for their base classes without altering program correctness.

**Formal Definition**: Given classes B (base) and D (derived), inheritance creates a subtype relationship D ≺ B, where:
- D inherits all non-private members of B
- D can override virtual methods of B
- D can extend B with additional members
- D can be used wherever B is expected (Liskov Substitution Principle)

#### Method Resolution Order (MRO) and Linearization

Python uses **C3 linearization** to determine method resolution order in multiple inheritance scenarios. The MRO algorithm ensures:

1. **Consistency**: Child classes are checked before parent classes
2. **Monotonicity**: The ordering is consistent across the inheritance hierarchy
3. **Preservation**: The local precedence order is maintained

For a class C with parents P₁, P₂, ..., Pₙ, the MRO is computed as:
```
MRO(C) = [C] + merge(MRO(P₁), MRO(P₂), ..., MRO(Pₙ), [P₁, P₂, ..., Pₙ])
```

#### Type Theory and Behavioral Substitution

The **Liskov Substitution Principle (LSP)** provides the theoretical foundation for safe inheritance:

**Definition**: If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering program correctness.

**Formal Constraints**:
- **Preconditions**: Cannot be strengthened in subtypes
- **Postconditions**: Cannot be weakened in subtypes
- **Invariants**: Must be preserved in subtypes
- **Exception Safety**: Subtypes cannot throw new exceptions

Inheritance allows a class to inherit attributes and methods from another class, enabling code reuse and the creation of hierarchical relationships while maintaining type safety and behavioral consistency.

### Basic Inheritance with Type Safety

```python
from abc import ABC, abstractmethod
from typing import Optional, List, TypeVar, Generic

T = TypeVar('T')

class Animal(ABC):
    """
    Abstract base class demonstrating inheritance with behavioral contracts.
    
    Establishes the behavioral interface that all concrete animal types must implement.
    This ensures type safety and behavioral consistency across the inheritance hierarchy.
    """
    
    def __init__(self, name: str, species: str, age: int):
        if age < 0:
            raise ValueError("Age cannot be negative")
        if not name.strip():
            raise ValueError("Name cannot be empty")
        
        self.name = name
        self.species = species
        self.age = age
        self._health = 100
        self._energy = 100
        self._hunger = 0
    
    @property
    def health(self) -> int:
        """Health property with invariant: 0 <= health <= 100"""
        return self._health
    
    @health.setter
    def health(self, value: int):
        """Setter ensuring health invariant is maintained"""
        if not 0 <= value <= 100:
            raise ValueError("Health must be between 0 and 100")
        self._health = value
    
    @property
    def energy(self) -> int:
        """Energy property with invariant: 0 <= energy <= 100"""
        return self._energy
    
    @energy.setter
    def energy(self, value: int):
        """Setter ensuring energy invariant is maintained"""
        if not 0 <= value <= 100:
            raise ValueError("Energy must be between 0 and 100")
        self._energy = value
    
    def eat(self, food: str) -> str:
        """
        Basic eating behavior that maintains health and hunger invariants.
        
        Postcondition: health increases (up to maximum), hunger decreases
        """
        if self._hunger > 0:
            self._hunger = max(0, self._hunger - 20)
            self._health = min(100, self._health + 10)
            return f"{self.name} is eating {food} and feels better"
        return f"{self.name} is not hungry"
    
    def sleep(self) -> str:
        """
        Basic sleeping behavior that restores energy.
        
        Postcondition: energy increases (up to maximum)
        """
        self._energy = min(100, self._energy + 30)
        return f"{self.name} is sleeping and restoring energy"
    
    def exercise(self) -> str:
        """
        Basic exercise behavior that affects energy and hunger.
        
        Postcondition: energy decreases, hunger increases
        """
        self._energy = max(0, self._energy - 20)
        self._hunger = min(100, self._hunger + 15)
        return f"{self.name} is exercising"
    
    @abstractmethod
    def make_sound(self) -> str:
        """Abstract method that must be implemented by concrete subclasses"""
        pass
    
    @abstractmethod
    def get_species_info(self) -> str:
        """Abstract method for species-specific information"""
        pass
    
    def get_info(self) -> str:
        """Template method using other methods"""
        return (f"{self.name} is a {self.age}-year-old {self.species} "
                f"with {self.health} health and {self.energy} energy")
    
    def __str__(self) -> str:
        return f"{self.name} the {self.species}"
    
    def __repr__(self) -> str:
        return f"Animal(name='{self.name}', species='{self.species}', age={self.age})"

class Dog(Animal):
    """
    Concrete Dog class that extends Animal with dog-specific behaviors.
    
    Maintains LSP by preserving all parent class contracts while adding
    dog-specific functionality.
    """
    
    def __init__(self, name: str, breed: str, age: int):
        super().__init__(name, "Dog", age)
        self.breed = breed
        self._loyalty = 100
        self._training_level = 0
    
    @property
    def loyalty(self) -> int:
        """Dog-specific property for loyalty"""
        return self._loyalty
    
    @loyalty.setter
    def loyalty(self, value: int):
        if not 0 <= value <= 100:
            raise ValueError("Loyalty must be between 0 and 100")
        self._loyalty = value
    
    def make_sound(self) -> str:
        """Implementation of abstract method with dog-specific behavior"""
        return f"{self.name} barks: Woof! Woof!"
    
    def get_species_info(self) -> str:
        """Implementation of abstract method with dog-specific information"""
        return f"{self.name} is a {self.breed} dog with {self.loyalty} loyalty"
    
    def fetch(self, item: str) -> str:
        """Dog-specific behavior that affects energy and loyalty"""
        if self._energy < 20:
            return f"{self.name} is too tired to fetch"
        
        self._energy -= 15
        self._loyalty = min(100, self._loyalty + 5)
        return f"{self.name} fetches the {item} and brings it back"
    
    def wag_tail(self) -> str:
        """Dog-specific behavior indicating happiness"""
        return f"{self.name} is wagging its tail happily"
    
    def train(self, command: str) -> str:
        """Dog-specific training behavior"""
        if self._energy < 10:
            return f"{self.name} is too tired to train"
        
        self._energy -= 10
        self._training_level = min(100, self._training_level + 10)
        return f"{self.name} is learning the '{command}' command"
    
    def exercise(self) -> str:
        """Override parent method with dog-specific exercise behavior"""
        result = super().exercise()  # Call parent implementation
        self._loyalty = min(100, self._loyalty + 2)  # Dogs love exercise
        return result + " and enjoying the activity"

class Cat(Animal):
    """
    Concrete Cat class that extends Animal with cat-specific behaviors.
    
    Demonstrates different behavioral implementations while maintaining
    the same interface contracts.
    """
    
    def __init__(self, name: str, color: str, age: int):
        super().__init__(name, "Cat", age)
        self.color = color
        self._independence = 80
        self._curiosity = 90
    
    @property
    def independence(self) -> int:
        """Cat-specific property for independence"""
        return self._independence
    
    @independence.setter
    def independence(self, value: int):
        if not 0 <= value <= 100:
            raise ValueError("Independence must be between 0 and 100")
        self._independence = value
    
    def make_sound(self) -> str:
        """Implementation of abstract method with cat-specific behavior"""
        return f"{self.name} meows: Meow!"
    
    def get_species_info(self) -> str:
        """Implementation of abstract method with cat-specific information"""
        return f"{self.name} is a {self.color} cat with {self.independence} independence"
    
    def climb(self, object_name: str) -> str:
        """Cat-specific behavior for climbing"""
        if self._energy < 15:
            return f"{self.name} is too tired to climb"
        
        self._energy -= 10
        self._curiosity = min(100, self._curiosity + 5)
        return f"{self.name} gracefully climbs up the {object_name}"
    
    def purr(self) -> str:
        """Cat-specific behavior indicating contentment"""
        return f"{self.name} is purring contentedly"
    
    def hunt(self, prey: str) -> str:
        """Cat-specific hunting behavior"""
        if self._energy < 25:
            return f"{self.name} is too tired to hunt"
        
        self._energy -= 20
        self._hunger = max(0, self._hunger - 10)
        return f"{self.name} is stalking the {prey}"
    
    def exercise(self) -> str:
        """Override parent method with cat-specific exercise behavior"""
        result = super().exercise()
        self._curiosity = min(100, self._curiosity + 3)  # Cats are curious
        return result + " with feline grace"

# Usage demonstrating polymorphism and type safety
def animal_care_routine(animal: Animal) -> List[str]:
    """
    Function demonstrating polymorphism - works with any Animal subtype.
    
    This function can accept any concrete Animal subclass and will work
    correctly due to the behavioral contracts established by the base class.
    """
    results = []
    results.append(animal.get_info())
    results.append(animal.make_sound())
    results.append(animal.eat("food"))
    results.append(animal.exercise())
    results.append(animal.sleep())
    results.append(animal.get_species_info())
    return results

# Create instances
dog = Dog("Buddy", "Golden Retriever", 3)
cat = Cat("Whiskers", "Orange", 2)

# Demonstrate polymorphism
print("=== Dog Care Routine ===")
for result in animal_care_routine(dog):
    print(result)

print("\n=== Cat Care Routine ===")
for result in animal_care_routine(cat):
    print(result)

# Demonstrate subclass-specific behaviors
print("\n=== Species-Specific Behaviors ===")
print(dog.fetch("ball"))
print(dog.wag_tail())
print(dog.train("sit"))

print(cat.climb("tree"))
print(cat.purr())
print(cat.hunt("mouse"))
```

print(dog.make_sound())  # Buddy barks: Woof! Woof!
print(cat.make_sound())  # Whiskers meows: Meow!

print(dog.fetch("ball"))  # Buddy fetches the ball
print(cat.climb("tree"))  # Whiskers climbs up the tree
```

### Method Overriding and super()

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
            return f"{self.year} {self.make} {self.model} started"
        return "Vehicle is already running"
    
    def stop(self):
        if self.is_running:
            self.is_running = False
            return f"{self.year} {self.make} {self.model} stopped"
        return "Vehicle is already stopped"
    
    def get_info(self):
        status = "Running" if self.is_running else "Stopped"
        return f"{self.year} {self.make} {self.model} - Status: {status}"

class Car(Vehicle):
    def __init__(self, make, model, year, doors):
        super().__init__(make, model, year)
        self.doors = doors
        self.gear = "P"  # Park
    
    def start(self):
        if self.gear != "P":
            return "Cannot start: Car must be in Park"
        result = super().start()
        if self.is_running:
            self.gear = "D"  # Drive
        return result
    
    def stop(self):
        result = super().stop()
        if not self.is_running:
            self.gear = "P"
        return result
    
    def shift_gear(self, gear):
        if self.is_running and gear in ["D", "R", "N", "P"]:
            self.gear = gear
            return f"Shifted to {gear}"
        return "Cannot shift gear"
    
    def get_info(self):
        base_info = super().get_info()
        return f"{base_info}, Doors: {self.doors}, Gear: {self.gear}"

class Motorcycle(Vehicle):
    def __init__(self, make, model, year, engine_size):
        super().__init__(make, model, year)
        self.engine_size = engine_size
        self.has_helmet = False
    
    def start(self):
        if not self.has_helmet:
            return "Cannot start: Must wear helmet first"
        return super().start()
    
    def wear_helmet(self):
        self.has_helmet = True
        return "Helmet is now on"
    
    def remove_helmet(self):
        if not self.is_running:
            self.has_helmet = False
            return "Helmet removed"
        return "Cannot remove helmet while riding"
    
    def get_info(self):
        base_info = super().get_info()
        helmet_status = "On" if self.has_helmet else "Off"
        return f"{base_info}, Engine: {self.engine_size}cc, Helmet: {helmet_status}"

# Usage
car = Car("Toyota", "Camry", 2022, 4)
motorcycle = Motorcycle("Honda", "CBR600", 2023, 600)

print(car.start())  # 2022 Toyota Camry started
print(car.shift_gear("R"))  # Shifted to R
print(car.get_info())  # 2022 Toyota Camry - Status: Running, Doors: 4, Gear: R

print(motorcycle.start())  # Cannot start: Must wear helmet first
print(motorcycle.wear_helmet())  # Helmet is now on
print(motorcycle.start())  # 2023 Honda CBR600 started
print(motorcycle.get_info())  # 2023 Honda CBR600 - Status: Running, Engine: 600cc, Helmet: On
```

### Multiple Inheritance

```python
class Flyable:
    def __init__(self):
        self.altitude = 0
        self.max_altitude = 10000
    
    def take_off(self):
        if self.altitude == 0:
            self.altitude = 100
            return "Taking off!"
        return "Already in the air"
    
    def land(self):
        if self.altitude > 0:
            self.altitude = 0
            return "Landing!"
        return "Already on the ground"
    
    def fly_higher(self, feet):
        if self.altitude + feet <= self.max_altitude:
            self.altitude += feet
            return f"Flying at {self.altitude} feet"
        return f"Cannot exceed maximum altitude of {self.max_altitude} feet"

class Swimmable:
    def __init__(self):
        self.depth = 0
        self.max_depth = 50
    
    def dive(self, depth):
        if self.depth + depth <= self.max_depth:
            self.depth += depth
            return f"Diving to {self.depth} feet deep"
        return f"Cannot exceed maximum depth of {self.max_depth} feet"
    
    def surface(self):
        if self.depth > 0:
            self.depth = 0
            return "Surfacing!"
        return "Already at surface"

class Duck(Animal, Flyable, Swimmable):
    def __init__(self, name, age):
        Animal.__init__(self, name, "Duck", age)
        Flyable.__init__(self)
        Swimmable.__init__(self)
        self.max_altitude = 5000  # Lower than default
        self.max_depth = 10       # Lower than default
    
    def make_sound(self):
        return f"{self.name} quacks: Quack! Quack!"
    
    def paddle(self):
        return f"{self.name} is paddling in the water"
    
    def get_status(self):
        location = "Flying" if self.altitude > 0 else "Swimming" if self.depth > 0 else "On ground"
        return f"{self.name} is {location}"

# Usage
duck = Duck("Donald", 2)

print(duck.make_sound())  # Donald quacks: Quack! Quack!
print(duck.get_status())  # Donald is On ground

print(duck.take_off())  # Taking off!
print(duck.fly_higher(500))  # Flying at 600 feet
print(duck.get_status())  # Donald is Flying

print(duck.land())  # Landing!
print(duck.dive(5))  # Diving to 5 feet deep
print(duck.get_status())  # Donald is Swimming

print(duck.surface())  # Surfacing!
print(duck.paddle())  # Donald is paddling in the water
```

### Method Resolution Order (MRO)

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

# Check method resolution order
print(D.__mro__)
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

d = D()
print(d.method())  # "B" (B comes before C in MRO)

# Using super() in multiple inheritance
class E(B, C):
    def method(self):
        return f"E -> {super().method()}"

e = E()
print(e.method())  # "E -> B"
```

---

## Polymorphism

#### Theoretical Foundation

Polymorphism, derived from Greek meaning "many forms," is a fundamental concept in type theory that allows a single interface to represent different underlying forms (data types). It enables **behavioral substitution** where objects of different types can be used interchangeably if they implement the same interface.

**Types of Polymorphism**:

1. **Ad-hoc Polymorphism** (Overloading): Same name, different implementations based on argument types
2. **Parametric Polymorphism** (Generics): Code that works with type parameters
3. **Subtype Polymorphism** (Inheritance): Objects of derived classes can be treated as base class objects
4. **Duck Typing**: "If it walks like a duck and quacks like a duck, it's a duck"

#### Formal Mathematical Foundation

In type theory, polymorphism can be expressed using **universal quantification**:

```
∀ T. (T → T) → List[T] → List[T]
```

This notation means "for all types T, given a function from T to T and a list of T, return a list of T."

#### Liskov Substitution Principle (LSP)

The theoretical foundation for safe polymorphism is the **Liskov Substitution Principle**:

**Definition**: Objects of a superclass should be replaceable with objects of a subclass without breaking the application.

**Formal Constraints**:
- **Contravariance** of method parameters: Subclass methods can accept more general parameters
- **Covariance** of return types: Subclass methods can return more specific types
- **Invariant preservation**: Class invariants must be maintained in all subclasses

#### Method Dispatch and Virtual Function Tables

Python implements **dynamic method dispatch** using a mechanism similar to virtual function tables (vtables). When a method is called on an object, Python:

1. Looks up the method in the object's type dictionary
2. Follows the MRO (Method Resolution Order) if not found
3. Calls the first matching method found

This enables **late binding** where the actual method to call is determined at runtime.

Polymorphism allows objects of different types to be treated as objects of a common base type, enabling flexible and extensible code through behavioral substitution and interface consistency.

### Runtime Polymorphism with Type Safety

```python
from abc import ABC, abstractmethod
from typing import List, Protocol, TypeVar, Generic
import math

# Protocol-based polymorphism (structural subtyping)
class Drawable(Protocol):
    """Protocol defining the interface for drawable objects"""
    def draw(self) -> str:
        ...
    
    def get_bounding_box(self) -> tuple[float, float, float, float]:
        """Return (x, y, width, height) of bounding box"""
        ...

class Measurable(Protocol):
    """Protocol defining the interface for measurable objects"""
    def area(self) -> float:
        ...
    
    def perimeter(self) -> float:
        ...

class Shape(ABC):
    """
    Abstract base class demonstrating polymorphism with behavioral contracts.
    
    Establishes the mathematical interface that all geometric shapes must implement.
    Uses abstract methods to enforce the contract in concrete subclasses.
    """
    
    def __init__(self, name: str):
        if not name.strip():
            raise ValueError("Shape name cannot be empty")
        self.name = name
    
    @abstractmethod
    def area(self) -> float:
        """
        Calculate the area of the shape.
        
        Returns:
            float: Area in square units
            
        Postcondition: result >= 0
        """
        pass
    
    @abstractmethod
    def perimeter(self) -> float:
        """
        Calculate the perimeter of the shape.
        
        Returns:
            float: Perimeter in linear units
            
        Postcondition: result >= 0
        """
        pass
    
    @abstractmethod
    def get_bounding_box(self) -> tuple[float, float, float, float]:
        """
        Get the bounding box of the shape.
        
        Returns:
            tuple: (x, y, width, height) where (x, y) is the top-left corner
        """
        pass
    
    def display_info(self) -> str:
        """
        Template method using abstract methods.
        
        This demonstrates the Template Method pattern where the algorithm
        structure is defined in the base class but specific steps are
        implemented by subclasses.
        """
        return f"{self.name}: Area = {self.area():.2f}, Perimeter = {self.perimeter():.2f}"
    
    def draw(self) -> str:
        """Default drawing implementation"""
        return f"Drawing {self.name}"
    
    def scale(self, factor: float) -> 'Shape':
        """Abstract method for scaling - must be implemented by subclasses"""
        raise NotImplementedError("Subclasses must implement scale method")
    
    def __str__(self) -> str:
        return f"{self.name}(area={self.area():.2f})"
    
    def __repr__(self) -> str:
        return f"{self.__class__.__name__}(name='{self.name}')"

class Rectangle(Shape):
    """
    Concrete Rectangle class implementing the Shape interface.
    
    Maintains the mathematical invariants:
    - width > 0 and height > 0
    - area = width * height
    - perimeter = 2 * (width + height)
    """
    
    def __init__(self, width: float, height: float):
        super().__init__("Rectangle")
        if width <= 0 or height <= 0:
            raise ValueError("Width and height must be positive")
        self.width = width
        self.height = height
    
    def area(self) -> float:
        """Rectangle area calculation: A = w * h"""
        return self.width * self.height
    
    def perimeter(self) -> float:
        """Rectangle perimeter calculation: P = 2(w + h)"""
        return 2 * (self.width + self.height)
    
    def get_bounding_box(self) -> tuple[float, float, float, float]:
        """Rectangle bounding box is the rectangle itself"""
        return (0, 0, self.width, self.height)
    
    def scale(self, factor: float) -> 'Rectangle':
        """Scale rectangle by factor"""
        if factor <= 0:
            raise ValueError("Scale factor must be positive")
        return Rectangle(self.width * factor, self.height * factor)
    
    def draw(self) -> str:
        """Rectangle-specific drawing"""
        return f"Drawing Rectangle: {self.width}x{self.height}"
    
    def __repr__(self) -> str:
        return f"Rectangle(width={self.width}, height={self.height})"

class Circle(Shape):
    """
    Concrete Circle class implementing the Shape interface.
    
    Maintains the mathematical invariants:
    - radius > 0
    - area = π * r²
    - perimeter = 2 * π * r
    """
    
    def __init__(self, radius: float):
        super().__init__("Circle")
        if radius <= 0:
            raise ValueError("Radius must be positive")
        self.radius = radius
    
    def area(self) -> float:
        """Circle area calculation: A = π * r²"""
        return math.pi * self.radius ** 2
    
    def perimeter(self) -> float:
        """Circle perimeter calculation: P = 2π * r"""
        return 2 * math.pi * self.radius
    
    def get_bounding_box(self) -> tuple[float, float, float, float]:
        """Circle bounding box is a square containing the circle"""
        diameter = 2 * self.radius
        return (-self.radius, -self.radius, diameter, diameter)
    
    def scale(self, factor: float) -> 'Circle':
        """Scale circle by factor"""
        if factor <= 0:
            raise ValueError("Scale factor must be positive")
        return Circle(self.radius * factor)
    
    def draw(self) -> str:
        """Circle-specific drawing"""
        return f"Drawing Circle: radius={self.radius}"
    
    def __repr__(self) -> str:
        return f"Circle(radius={self.radius})"

class Triangle(Shape):
    """
    Concrete Triangle class implementing the Shape interface.
    
    Maintains the mathematical invariants:
    - Triangle inequality: a + b > c, b + c > a, a + c > b
    - area = √(s(s-a)(s-b)(s-c)) where s = (a+b+c)/2 (Heron's formula)
    - perimeter = a + b + c
    """
    
    def __init__(self, side_a: float, side_b: float, side_c: float):
        super().__init__("Triangle")
        if side_a <= 0 or side_b <= 0 or side_c <= 0:
            raise ValueError("All sides must be positive")
        
        # Validate triangle inequality
        if not (side_a + side_b > side_c and 
                side_b + side_c > side_a and 
                side_a + side_c > side_b):
            raise ValueError("Triangle inequality not satisfied")
        
        self.side_a = side_a
        self.side_b = side_b
        self.side_c = side_c
    
    def area(self) -> float:
        """Triangle area using Heron's formula"""
        s = (self.side_a + self.side_b + self.side_c) / 2
        return math.sqrt(s * (s - self.side_a) * (s - self.side_b) * (s - self.side_c))
    
    def perimeter(self) -> float:
        """Triangle perimeter: sum of all sides"""
        return self.side_a + self.side_b + self.side_c
    
    def get_bounding_box(self) -> tuple[float, float, float, float]:
        """Simplified bounding box calculation"""
        # For a more accurate implementation, we would need vertex coordinates
        height = (2 * self.area()) / self.side_a
        return (0, 0, self.side_a, height)
    
    def scale(self, factor: float) -> 'Triangle':
        """Scale triangle by factor"""
        if factor <= 0:
            raise ValueError("Scale factor must be positive")
        return Triangle(
            self.side_a * factor,
            self.side_b * factor,
            self.side_c * factor
        )
    
    def draw(self) -> str:
        """Triangle-specific drawing"""
        return f"Drawing Triangle: sides=({self.side_a}, {self.side_b}, {self.side_c})"
    
    def __repr__(self) -> str:
        return f"Triangle(sides=({self.side_a}, {self.side_b}, {self.side_c}))"
    def __init__(self, radius):
        super().__init__("Circle")
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def perimeter(self):
        return 2 * 3.14159 * self.radius

class Triangle(Shape):
    def __init__(self, side1, side2, side3):
        super().__init__("Triangle")
        self.side1 = side1
        self.side2 = side2
        self.side3 = side3
    
    def area(self):
        # Using Heron's formula
        s = (self.side1 + self.side2 + self.side3) / 2
        return (s * (s - self.side1) * (s - self.side2) * (s - self.side3)) ** 0.5
    
    def perimeter(self):
        return self.side1 + self.side2 + self.side3

# Polymorphism in action
shapes = [
    Rectangle(4, 5),
    Circle(3),
    Triangle(3, 4, 5),
    Rectangle(2, 8),
    Circle(2.5)
]

print("=== Polymorphism Demonstration ===")

# Polymorphic method calls
print("\nShape Information:")
for shape in shapes:
    print(f"  {shape.display_info()}")

# Generic algorithm using polymorphism
total_area = calculate_total_area(shapes)
print(f"\nTotal area of all shapes: {total_area:.2f}")

# Sorting using polymorphic comparison
sorted_shapes = sort_shapes_by_area(shapes)
print(f"\nShapes sorted by area:")
for shape in sorted_shapes:
    print(f"  {shape}")

# Polymorphic drawing
print(f"\nDrawing all shapes:")
drawings = draw_all_shapes(shapes)
for drawing in drawings:
    print(f"  {drawing}")

# Polymorphic scaling
print(f"\nScaling all shapes by factor 2:")
scaled_shapes = scale_all_shapes(shapes, 2.0)
for original, scaled in zip(shapes, scaled_shapes):
    print(f"  {original} -> {scaled}")

# Protocol-based polymorphism
print(f"\nComparing shapes using protocols:")
rect = Rectangle(4, 3)
circle = Circle(2)
print(f"  {compare_shapes(rect, circle)}")

# Demonstrating Liskov Substitution Principle
print(f"\nLSP Demonstration - Shape references to different types:")
shape1: Shape = Rectangle(6, 4)  # Shape reference to Rectangle
shape2: Shape = Circle(3)        # Shape reference to Circle
shape3: Shape = Triangle(5, 5, 5) # Shape reference to Triangle

# All can be used through the same interface
test_shapes = [shape1, shape2, shape3]
for i, shape in enumerate(test_shapes, 1):
    print(f"  Shape {i}: {shape.display_info()}")
    print(f"    Bounding box: {shape.get_bounding_box()}")
    print(f"    {shape.draw()}")
    
# Type checking at runtime
print(f"\nRuntime type information:")
for shape in shapes:
    print(f"  {shape.name} is instance of {type(shape).__name__}")
    print(f"    MRO: {[cls.__name__ for cls in type(shape).__mro__]}")
```

#### Advanced Polymorphism Patterns

```python
from typing import TypeVar, Generic, Callable, Any

T = TypeVar('T', bound=Shape)

class ShapeProcessor(Generic[T]):
    """
    Generic shape processor demonstrating parametric polymorphism.
    
    This class can work with any Shape subtype while maintaining type safety.
    """
    
    def __init__(self, shape_type: type[T]):
        self.shape_type = shape_type
        self.processed_shapes: List[T] = []
    
    def process_shape(self, shape: T, operation: Callable[[T], T]) -> T:
        """
        Process a shape using a given operation.
        
        Args:
            shape: The shape to process
            operation: Function that transforms the shape
            
        Returns:
            The processed shape
        """
        if not isinstance(shape, self.shape_type):
            raise TypeError(f"Expected {self.shape_type.__name__}, got {type(shape).__name__}")
        
        processed = operation(shape)
        self.processed_shapes.append(processed)
        return processed
    
    def get_statistics(self) -> dict[str, float]:
        """Get statistics about processed shapes"""
        if not self.processed_shapes:
            return {"count": 0, "total_area": 0.0, "average_area": 0.0}
        
        areas = [shape.area() for shape in self.processed_shapes]
        return {
            "count": len(self.processed_shapes),
            "total_area": sum(areas),
            "average_area": sum(areas) / len(areas),
            "min_area": min(areas),
            "max_area": max(areas)
        }

# Usage of generic shape processor
rectangle_processor = ShapeProcessor(Rectangle)
circle_processor = ShapeProcessor(Circle)

# Process rectangles
rect1 = Rectangle(3, 4)
rect2 = Rectangle(5, 2)

scaled_rect1 = rectangle_processor.process_shape(rect1, lambda r: r.scale(2))
scaled_rect2 = rectangle_processor.process_shape(rect2, lambda r: r.scale(1.5))

print("\nRectangle processor statistics:")
print(rectangle_processor.get_statistics())

# Process circles
circle1 = Circle(2)
circle2 = Circle(3)

scaled_circle1 = circle_processor.process_shape(circle1, lambda c: c.scale(1.5))
scaled_circle2 = circle_processor.process_shape(circle2, lambda c: c.scale(0.8))

print("\nCircle processor statistics:")
print(circle_processor.get_statistics())
```

---

## Abstraction and Abstract Classes

#### Theoretical Foundation

**Abstraction** is a fundamental principle in computer science that involves hiding implementation complexity while exposing only the essential characteristics and operations of an object or system. In object-oriented programming, abstraction serves as a cognitive tool for managing complexity and establishing clear **behavioral contracts**.

**Formal Definition**: An abstraction A over a concrete implementation I is a mapping A: I → E where E represents the essential interface and behaviors that clients need to understand.

**Key Theoretical Concepts**:

1. **Interface Segregation**: Clients should not be forced to depend on interfaces they don't use
2. **Dependency Inversion**: High-level modules should not depend on low-level modules; both should depend on abstractions
3. **Behavioral Specifications**: Abstract classes define **preconditions**, **postconditions**, and **invariants** that all implementations must satisfy

#### Abstract Data Types (ADTs)

From a theoretical perspective, abstract classes implement **Abstract Data Types**, which are mathematical specifications of data structures defined by their behavior rather than their implementation:

**ADT Definition**: An ADT is a triple (D, O, A) where:
- D = set of data values
- O = set of operations
- A = set of axioms defining the relationships between operations

#### Formal Verification and Contracts

Abstract classes enable **design by contract** where each method specifies:
- **Preconditions**: What must be true when the method is called
- **Postconditions**: What must be true when the method returns
- **Invariants**: What must always be true about the object's state

This approach supports **formal verification** where implementations can be mathematically proven to satisfy their specifications.

#### Interface Theory and Behavioral Substitutability

Abstract classes establish **behavioral interfaces** that enable safe substitution through the **Liskov Substitution Principle**. This creates a formal contract where any implementation of the abstract class must preserve the behavioral properties defined by the abstraction.

Abstraction involves hiding complex implementation details and showing only the essential features of an object. Abstract classes provide a way to enforce that certain methods must be implemented by subclasses while establishing formal behavioral contracts.

### Abstract Base Classes with Formal Contracts

```python
from abc import ABC, abstractmethod, abstractproperty
from typing import Optional, List, Dict, Any
from decimal import Decimal
from datetime import datetime
import uuid

class Product(ABC):
    """
    Abstract base class for all products in an e-commerce system.
    
    This class establishes the behavioral contract that all product types must follow.
    It demonstrates abstraction by hiding implementation details while providing
    a clear interface for product operations.
    
    Invariants:
    - price >= 0
    - stock_quantity >= 0
    - name is not empty
    - product_id is unique
    
    Class Invariant: ∀ product ∈ Product: 
        product.price ≥ 0 ∧ product.stock_quantity ≥ 0 ∧ len(product.name) > 0
    """
    
    def __init__(self, name: str, price: Decimal, stock_quantity: int):
        """
        Initialize a product with validation.
        
        Preconditions:
        - name must not be empty
        - price must be non-negative
        - stock_quantity must be non-negative
        
        Postconditions:
        - Object is in a valid state
        - Invariants are established
        """
        if not name.strip():
            raise ValueError("Product name cannot be empty")
        if price < 0:
            raise ValueError("Price cannot be negative")
        if stock_quantity < 0:
            raise ValueError("Stock quantity cannot be negative")
        
        self._product_id = str(uuid.uuid4())
        self._name = name
        self._price = price
        self._stock_quantity = stock_quantity
        self._created_at = datetime.now()
        self._last_modified = datetime.now()
    
    @property
    def product_id(self) -> str:
        """Immutable product identifier"""
        return self._product_id
    
    @property
    def name(self) -> str:
        """Product name"""
        return self._name
    
    @name.setter
    def name(self, value: str):
        """Set product name with validation"""
        if not value.strip():
            raise ValueError("Product name cannot be empty")
        self._name = value
        self._last_modified = datetime.now()
    
    @property
    def price(self) -> Decimal:
        """Product price"""
        return self._price
    
    @price.setter
    def price(self, value: Decimal):
        """Set price with validation"""
        if value < 0:
            raise ValueError("Price cannot be negative")
        self._price = value
        self._last_modified = datetime.now()
    
    @property
    def stock_quantity(self) -> int:
        """Available stock quantity"""
        return self._stock_quantity
    
    @abstractmethod
    def get_product_type(self) -> str:
        """
        Return the type of product.
        
        Abstract method that must be implemented by concrete subclasses.
        
        Returns:
            str: Product type identifier
            
        Postcondition: result is a non-empty string
        """
        pass
    
    @abstractmethod
    def calculate_shipping_cost(self, destination: str, quantity: int = 1) -> Decimal:
        """
        Calculate shipping cost for the product.
        
        Abstract method for computing shipping costs based on product characteristics.
        
        Args:
            destination: Shipping destination
            quantity: Number of items to ship
            
        Returns:
            Decimal: Shipping cost
            
        Preconditions:
        - destination is not empty
        - quantity > 0
        
        Postconditions:
        - result >= 0
        """
        pass
    
    @abstractmethod
    def get_storage_requirements(self) -> Dict[str, Any]:
        """
        Return storage requirements for the product.
        
        Returns:
            Dict containing storage specifications
            
        Postcondition: result is a non-empty dictionary
        """
        pass
    
    def is_available(self, quantity: int = 1) -> bool:
        """
        Check if product is available in requested quantity.
        
        Args:
            quantity: Requested quantity
            
        Returns:
            bool: True if available, False otherwise
            
        Precondition: quantity > 0
        Postcondition: result is consistent with stock_quantity
        """
        if quantity <= 0:
            raise ValueError("Quantity must be positive")
        return self._stock_quantity >= quantity
    
    def reduce_stock(self, quantity: int) -> bool:
        """
        Reduce stock quantity if available.
        
        Args:
            quantity: Amount to reduce
            
        Returns:
            bool: True if reduction successful, False otherwise
            
        Precondition: quantity > 0
        Postcondition: if result is True, stock_quantity is reduced by quantity
        """
        if quantity <= 0:
            raise ValueError("Quantity must be positive")
        
        if self.is_available(quantity):
            self._stock_quantity -= quantity
            self._last_modified = datetime.now()
            return True
        return False
    
    def add_stock(self, quantity: int) -> None:
        """
        Add stock quantity.
        
        Args:
            quantity: Amount to add
            
        Precondition: quantity > 0
        Postcondition: stock_quantity is increased by quantity
        """
        if quantity <= 0:
            raise ValueError("Quantity must be positive")
        
        self._stock_quantity += quantity
        self._last_modified = datetime.now()
    
    def get_info(self) -> str:
        """
        Get formatted product information.
        
        Template method that uses other methods to provide information.
        
        Returns:
            str: Formatted product information
        """
        return (f"{self.name} ({self.get_product_type()}) - "
                f"${self.price} (Stock: {self.stock_quantity})")
    
    def __str__(self) -> str:
        return f"{self.name} - ${self.price}"
    
    def __repr__(self) -> str:
        return f"Product(name='{self.name}', price={self.price}, stock={self.stock_quantity})"
    
    def __eq__(self, other) -> bool:
        """Equality based on product_id"""
        if not isinstance(other, Product):
            return NotImplemented
        return self.product_id == other.product_id
    
    def __hash__(self) -> int:
        return hash(self.product_id)

class PhysicalProduct(Product):
    """
    Concrete implementation for physical products.
    
    Adds physical characteristics like weight and dimensions
    while maintaining the behavioral contract established by Product.
    """
    
    def __init__(self, name: str, price: Decimal, stock_quantity: int, 
                 weight: float, dimensions: tuple[float, float, float]):
        super().__init__(name, price, stock_quantity)
        
        if weight <= 0:
            raise ValueError("Weight must be positive")
        if any(d <= 0 for d in dimensions):
            raise ValueError("All dimensions must be positive")
        
        self.weight = weight  # in kg
        self.dimensions = dimensions  # (length, width, height) in cm
    
    def get_product_type(self) -> str:
        """Implementation of abstract method"""
        return "Physical"
    
    def calculate_shipping_cost(self, destination: str, quantity: int = 1) -> Decimal:
        """
        Calculate shipping cost based on weight and dimensions.
        
        Uses dimensional weight pricing common in shipping.
        """
        if not destination.strip():
            raise ValueError("Destination cannot be empty")
        if quantity <= 0:
            raise ValueError("Quantity must be positive")
        
        # Calculate dimensional weight
        length, width, height = self.dimensions
        dimensional_weight = (length * width * height) / 5000  # Standard divisor
        
        # Use the greater of actual weight or dimensional weight
        billable_weight = max(self.weight, dimensional_weight)
        
        # Base cost calculation (simplified)
        base_cost_per_kg = Decimal('5.00')
        distance_multiplier = Decimal('1.2')  # Simplified distance factor
        
        total_cost = (Decimal(str(billable_weight)) * base_cost_per_kg * 
                     distance_multiplier * quantity)
        
        return round(total_cost, 2)
    
    def get_storage_requirements(self) -> Dict[str, Any]:
        """Return storage requirements for physical products"""
        length, width, height = self.dimensions
        volume = length * width * height
        
        return {
            'type': 'physical_storage',
            'volume_cm3': volume,
            'weight_kg': self.weight,
            'fragile': False,  # Could be extended
            'temperature_controlled': False,
            'hazardous': False
        }
    
    def get_volume(self) -> float:
        """Calculate product volume"""
        return self.dimensions[0] * self.dimensions[1] * self.dimensions[2]
    
    def __repr__(self) -> str:
        return (f"PhysicalProduct(name='{self.name}', price={self.price}, "
                f"weight={self.weight}, dimensions={self.dimensions})")

class DigitalProduct(Product):
    """
    Concrete implementation for digital products.
    
    Demonstrates how abstraction allows for completely different implementations
    while maintaining the same interface.
    """
    
    def __init__(self, name: str, price: Decimal, stock_quantity: int, 
                 file_size: int, download_format: str):
        super().__init__(name, price, stock_quantity)
        
        if file_size <= 0:
            raise ValueError("File size must be positive")
        if not download_format.strip():
            raise ValueError("Download format cannot be empty")
        
        self.file_size = file_size  # in bytes
        self.download_format = download_format
        self.max_downloads = 3  # Default download limit
    
    def get_product_type(self) -> str:
        """Implementation of abstract method"""
        return "Digital"
    
    def calculate_shipping_cost(self, destination: str, quantity: int = 1) -> Decimal:
        """
        Digital products have no shipping cost.
        
        Demonstrates how different product types can have
        completely different implementations of the same interface.
        """
        if not destination.strip():
            raise ValueError("Destination cannot be empty")
        if quantity <= 0:
            raise ValueError("Quantity must be positive")
        
        return Decimal('0.00')  # Digital products have no shipping cost
    
    def get_storage_requirements(self) -> Dict[str, Any]:
        """Return storage requirements for digital products"""
        return {
            'type': 'digital_storage',
            'file_size_bytes': self.file_size,
            'format': self.download_format,
            'backup_required': True,
            'cdn_distribution': True,
            'drm_protection': False
        }
    
    def get_file_size_mb(self) -> float:
        """Get file size in megabytes"""
        return self.file_size / (1024 * 1024)
    
    def __repr__(self) -> str:
        return (f"DigitalProduct(name='{self.name}', price={self.price}, "
                f"file_size={self.file_size}, format='{self.download_format}')")
    
    def calculate_shipping_cost(self):
        base_cost = 5.0
        weight_cost = self.weight * 0.5
        return base_cost + weight_cost

class DigitalProduct(Product):
    def __init__(self, name, price, download_link, file_size):
        super().__init__(name, price, float('inf'))  # Infinite stock
        self.download_link = download_link
        self.file_size = file_size
    
    def get_product_type(self):
        return "Digital"
    
    def calculate_shipping_cost(self):
        return 0.0  # No shipping cost for digital products

class Customer:
    def __init__(self, name, email, address):
        self.name = name
        self.email = email
        self.address = address
        self.order_history = []
    
    def place_order(self, cart):
        order = Order(self, cart.items.copy())
        self.order_history.append(order)
        return order

class ShoppingCart:
    def __init__(self):
        self.items = []
    
    def add_item(self, product, quantity=1):
        if product.is_available(quantity):
            self.items.append({'product': product, 'quantity': quantity})
            return f"Added {quantity} {product.name}(s) to cart"
        return f"Not enough stock for {product.name}"
    
    def remove_item(self, product_name):
        for i, item in enumerate(self.items):
            if item['product'].name == product_name:
                del self.items[i]
                return f"Removed {product_name} from cart"
        return f"{product_name} not found in cart"
    
    def calculate_total(self):
        total = 0
        for item in self.items:
            total += item['product'].price * item['quantity']
        return total
    
    def calculate_shipping(self):
        shipping = 0
        for item in self.items:
            shipping += item['product'].calculate_shipping_cost()
        return shipping
    
    def get_cart_summary(self):
        summary = []
        for item in self.items:
            summary.append(f"{item['product'].name} x{item['quantity']} - ${item['product'].price * item['quantity']}")
        return summary

class Order:
    order_counter = 0
    
    def __init__(self, customer, items):
        Order.order_counter += 1
        self.order_id = Order.order_counter
        self.customer = customer
        self.items = items
        self.order_date = datetime.now()
        self.status = "Pending"
        self.total_amount = self._calculate_total()
    
    def _calculate_total(self):
        total = 0
        for item in self.items:
            total += item['product'].price * item['quantity']
            total += item['product'].calculate_shipping_cost()
        return total
    
    def process_order(self):
        for item in self.items:
            product = item['product']
            quantity = item['quantity']
            if not product.reduce_stock(quantity):
                self.status = "Failed - Insufficient Stock"
                return False
        
        self.status = "Processed"
        return True
    
    def get_order_summary(self):
        return {
            'order_id': self.order_id,
            'customer': self.customer.name,
            'items': len(self.items),
            'total_amount': self.total_amount,
            'status': self.status,
            'order_date': self.order_date.strftime("%Y-%m-%d %H:%M:%S")
        }

# Usage
book = PhysicalProduct("Python Programming", 29.99, 50, 1.2, "8x10x2")
ebook = DigitalProduct("Python Programming (Digital)", 19.99, "https://download.link", 15.5)
laptop = PhysicalProduct("Gaming Laptop", 999.99, 10, 5.5, "15x10x2")

customer = Customer("John Doe", "john@example.com", "123 Main St")
cart = ShoppingCart()

print(cart.add_item(book, 2))
print(cart.add_item(ebook, 1))
print(cart.add_item(laptop, 1))

print(f"\nCart Summary:")
for item in cart.get_cart_summary():
    print(f"  {item}")

print(f"Subtotal: ${cart.calculate_total()}")
print(f"Shipping: ${cart.calculate_shipping()}")
print(f"Total: ${cart.calculate_total() + cart.calculate_shipping()}")

order = customer.place_order(cart)
print(f"\nOrder placed: {order.get_order_summary()}")
print(f"Order processed: {order.process_order()}")
```

---

## Exercises

### Exercise 1: Library Management System

Create a library management system with the following requirements:

1. **Abstract base class `LibraryItem`** with:
   - Abstract methods: `get_loan_period()`, `calculate_fine()`
   - Concrete methods: `borrow()`, `return_item()`

2. **Concrete classes** inheriting from `LibraryItem`:
   - `Book`, `DVD`, `Magazine`

3. **`Member` class** with:
   - Encapsulated member data
   - Methods to borrow and return items

4. **`Library` class** with:
   - Static method to validate member ID
   - Class method to get library statistics

### Exercise 2: Vehicle Rental System

Create a vehicle rental system demonstrating all OOP principles:

1. **Abstract `Vehicle` class**
2. **Concrete vehicle types**: `Car`, `Motorcycle`, `Truck`
3. **`RentalAgency` class** with polymorphic vehicle handling
4. **`Customer` class** with proper encapsulation
5. **`Rental` class** to manage rental transactions

### Exercise 3: Game Character System

Design a game character system with:

1. **Abstract `Character` class**
2. **Multiple inheritance** for abilities (Flyable, Swimmable, etc.)
3. **Polymorphic combat system**
4. **Equipment system** with proper encapsulation
5. **Character progression** with validation

### Exercise 4: Banking System

Create a comprehensive banking system with:

1. **Abstract `Account` class**
2. **Different account types**: Checking, Savings, Credit
3. **`Transaction` class** hierarchy
4. **`Bank` class** with account management
5. **`Customer` class** with multiple accounts

### Exercise 5: E-learning Platform

Design an e-learning platform with:

1. **Abstract `Course` class**
2. **Different course types**: Video, Interactive, Assessment
3. **`Student` and `Instructor` classes**
4. **`Platform` class** for course management
5. **Progress tracking** with proper encapsulation

## Summary

In this comprehensive guide, you've learned the four pillars of Object-Oriented Programming:

1. **Encapsulation**: Bundling data and methods, controlling access with properties and access modifiers
2. **Inheritance**: Creating new classes based on existing ones, promoting code reuse
3. **Polymorphism**: Treating objects of different types uniformly, enabling flexible code
4. **Abstraction**: Hiding complex implementation details, defining interfaces with abstract classes

These concepts form the foundation for writing maintainable, scalable, and organized Python code. Practice implementing these principles in your projects to master OOP in Python.

## Next Steps

1. **Practice**: Implement the exercises to reinforce your understanding
2. **Design Patterns**: Learn common OOP design patterns
3. **Real Projects**: Apply OOP principles to real-world projects
4. **Code Review**: Review existing OOP code to see these principles in action
5. **Advanced Topics**: Explore metaclasses, descriptors, and other advanced Python features
