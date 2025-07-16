# 4.3 Software Design Patterns

## Table of Contents
- [Introduction to Design Patterns](#introduction-to-design-patterns)
- [Pattern Classification](#pattern-classification)
- [Creational Patterns](#creational-patterns)
- [Structural Patterns](#structural-patterns)
- [Behavioral Patterns](#behavioral-patterns)
- [Modern Pattern Applications](#modern-pattern-applications)
- [Anti-Patterns and Code Smells](#anti-patterns-and-code-smells)
- [Pattern Selection and Implementation](#pattern-selection-and-implementation)
- [Practical Pattern Examples](#practical-pattern-examples)
- [Review Questions](#review-questions)
- [Answers and Explanations](#answers-and-explanations)
- [Further Reading and Multimedia Resources](#further-reading-and-multimedia-resources)

---

## Introduction to Design Patterns

**Definition:** Design patterns are reusable solutions to common problems in software design. They represent best practices and proven approaches that have evolved over time.

**Purpose:**
- Improve code reusability and maintainability
- Facilitate communication among developers
- Provide a shared vocabulary for design

**Gang of Four (GoF):** The foundational book "Design Patterns: Elements of Reusable Object-Oriented Software" by Gamma, Helm, Johnson, and Vlissides introduced 23 classic patterns.

**Pattern Structure:**
- Name
- Intent
- Motivation
- Structure (UML)
- Participants
- Consequences
- Implementation

**Advanced Perspective:**
- Patterns are not just code templates; they encode architectural wisdom and design trade-offs.
- Modern design patterns are being adapted for functional, concurrent, and distributed programming paradigms.
- Patterns are foundational for building extensible frameworks and libraries (e.g., Django ORM, Flask extensions).

**Critical Thinking Prompt:**
How do design patterns help in reducing technical debt in large-scale systems?

---

## Pattern Classification

- **Creational Patterns:** Object creation mechanisms (Singleton, Factory, Builder, Prototype, Object Pool)
- **Structural Patterns:** Object composition and relationships (Adapter, Decorator, Facade, Composite, Proxy, Bridge, Flyweight)
- **Behavioral Patterns:** Communication and responsibility (Observer, Strategy, Command, State, Template Method, Chain of Responsibility, Mediator, Memento, Visitor, Iterator)
- **Architectural Patterns vs. Design Patterns:** Architectural patterns address system-level structure; design patterns address class/object-level design.

**Research Trend:**
- Pattern mining from open-source repositories is an active research area (see "Automated Detection of Design Patterns in Source Code," IEEE Transactions on Software Engineering, 2023).

---

## Creational Patterns

**Advanced Theory:**
- Creational patterns abstract the instantiation process, decoupling client code from concrete classes and supporting dependency injection.
- In distributed systems, patterns like Object Pool and Prototype are used for resource management and object cloning across network boundaries.

### Singleton Pattern
**Intent:** Ensure a class has only one instance and provide a global point of access to it.
**What it achieves:**
- Controls access to shared resources (e.g., configuration, logging) by guaranteeing a single instance.
- Prevents inconsistent state by centralizing management of critical objects.
- Useful when exactly one object is needed to coordinate actions across a system.
- **Python Example:**
```python
class Singleton:
    _instance = None
    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super().__new__(cls)
        return cls._instance
```
- **Use Cases:** Logging, configuration, database connections

**Critical Thinking Prompt:**
What are the risks of using the Singleton pattern in multi-threaded or distributed systems?

### Factory Method Pattern
**Intent:** Define an interface for creating an object, but let subclasses decide which class to instantiate.
**What it achieves:**
- Decouples object creation from usage, enabling flexibility and extensibility.
- Supports open/closed principle by allowing new types to be introduced without changing existing code.
- Useful when a system should be independent of how its products are created, composed, and represented.
- **Python Example:**
```python
class Animal:
    def speak(self):
        pass
class Dog(Animal):
    def speak(self):
        return "Woof!"
class Cat(Animal):
    def speak(self):
        return "Meow!"
def animal_factory(animal_type):
    if animal_type == "dog":
        return Dog()
    elif animal_type == "cat":
        return Cat()
```

**Real-World Example:**
Factory methods are used in web frameworks (e.g., Django's model manager) to create model instances based on input data.

### Builder Pattern
**Intent:** Separate the construction of a complex object from its representation so that the same construction process can create different representations.
**What it achieves:**
- Simplifies the creation of complex, immutable, or composite objects.
- Allows step-by-step construction and customization of objects.
- Useful for objects with many optional parameters or configurations (e.g., UI builders, configuration objects).
- **Python Example:**
```python
class PizzaBuilder:
    def __init__(self):
        self.toppings = []
    def add_cheese(self):
        self.toppings.append("cheese")
        return self
    def add_pepperoni(self):
        self.toppings.append("pepperoni")
        return self
    def build(self):
        return f"Pizza with {', '.join(self.toppings)}"
# Usage: PizzaBuilder().add_cheese().add_pepperoni().build()
```

**Advanced Usage:**
Builder is ideal for constructing immutable objects (e.g., configuration objects) and for fluent APIs.

### Prototype Pattern
**Intent:** Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.
**What it achieves:**
- Enables efficient creation of new objects by cloning existing ones, rather than instantiating from scratch.
- Useful for objects that are costly to create or require complex initialization.
- Supports dynamic and flexible object creation at runtime (e.g., undo/redo, graphical editors).
- **Python Example:**
```python
import copy
class Prototype:
    def clone(self):
        return copy.deepcopy(self)
```

**Critical Thinking Prompt:**
How does the Prototype pattern support undo/redo functionality in applications?

### Object Pool Pattern
**Intent:** Manage a set of initialized objects kept ready to use, rather than allocating and destroying them on demand.
**What it achieves:**
- Reduces the overhead of repeatedly creating and destroying expensive resources (e.g., database connections, threads).
- Improves performance and resource utilization in high-load systems.
- Useful for managing limited resources in concurrent or distributed environments.
- **Python Example:**
```python
class ObjectPool:
    def __init__(self, cls):
        self._cls = cls
        self._available = []
    def acquire(self):
        return self._available.pop() if self._available else self._cls()
    def release(self, obj):
        self._available.append(obj)
```

**Real-World Example:**
Database connection pools in web servers (e.g., SQLAlchemy's connection pool) use this pattern for performance and resource management.

---

## Structural Patterns

**Advanced Theory:**
- Structural patterns enable flexible composition of objects and classes, supporting the open/closed principle and runtime adaptation.
- Adapter and Facade are often used for integrating legacy systems and third-party APIs.

### Adapter Pattern
**Intent:** Convert the interface of a class into another interface clients expect.
**What it achieves:**
- Enables classes with incompatible interfaces to work together without modifying their source code.
- Promotes code reuse and integration of legacy or third-party components.
- Useful for system integration, migration, or API adaptation.
- **Python Example:**
```python
class EuropeanSocket:
    def voltage(self): return 230
class USASocket:
    def voltage(self): return 120
class Adapter(EuropeanSocket):
    def voltage(self): return 120
```

**Critical Thinking Prompt:**
How can the Adapter pattern be used to enable microservices written in different languages to communicate?

### Decorator Pattern
**Intent:** Attach additional responsibilities to an object dynamically, providing a flexible alternative to subclassing for extending functionality.
**What it achieves:**
- Allows behavior to be added to individual objects, without affecting others of the same class.
- Supports open/closed principle by enabling new functionality without modifying existing code.
- Useful for adding features like logging, validation, or formatting at runtime.
- **Python Example:**
```python
def bold(func):
    def wrapper(*args, **kwargs):
        return f"<b>{func(*args, **kwargs)}</b>"
    return wrapper
@bold
def greet():
    return "Hello"
```

**Advanced Usage:**
Python's `functools.wraps` and middleware in web frameworks (e.g., Flask, Django) are real-world uses of the Decorator pattern.

### Facade Pattern
**Intent:** Provide a unified, high-level interface to a set of interfaces in a subsystem, making it easier to use.
**What it achieves:**
- Simplifies complex systems by hiding internal details and exposing only what is necessary.
- Reduces coupling between subsystems and clients.
- Useful for API design, library wrapping, and legacy system integration.
- **Python Example:**
```python
class SubsystemA:
    def operation(self): return "A"
class SubsystemB:
    def operation(self): return "B"
class Facade:
    def __init__(self):
        self.a = SubsystemA()
        self.b = SubsystemB()
    def operation(self):
        return self.a.operation() + self.b.operation()
```

**Real-World Example:**
APIs like `boto3` (AWS SDK for Python) use Facade to simplify complex cloud service interactions.

### Composite Pattern
**Intent:** Compose objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions uniformly.
**What it achieves:**
- Enables recursive composition of objects (e.g., GUI components, file systems).
- Simplifies client code by allowing uniform treatment of leaf and composite nodes.
- Useful for hierarchical data structures and operations over tree-like models.
- **Python Example:**
```python
class Component:
    def operation(self): pass
class Leaf(Component):
    def operation(self): return "Leaf"
class Composite(Component):
    def __init__(self): self.children = []
    def add(self, child): self.children.append(child)
    def operation(self): return "+".join(child.operation() for child in self.children)
```

**Critical Thinking Prompt:**
How does the Composite pattern support GUI frameworks and file system hierarchies?

### Proxy Pattern
**Intent:** Provide a surrogate or placeholder for another object to control access to it.
**What it achieves:**
- Adds a level of indirection to support access control, lazy initialization, logging, or remote access.
- Protects, optimizes, or augments the behavior of the real object.
- Useful for security, caching, remote method invocation, and resource management.
- **Python Example:**
```python
class RealSubject:
    def request(self): return "Real"
class Proxy:
    def __init__(self, real): self.real = real
    def request(self):
        print("Proxy: Logging access...")
        return self.real.request()
```

**Advanced Usage:**
Remote proxies are used in distributed systems (e.g., gRPC stubs, RPyC in Python) to represent remote objects locally.

### Bridge Pattern
**Intent:** Decouple an abstraction from its implementation so that the two can vary independently.
**What it achieves:**
- Enables independent evolution of abstraction and implementation hierarchies.
- Promotes flexibility and extensibility in large systems.
- Useful for cross-platform GUIs, device drivers, and systems requiring runtime binding.
- **Python Example:**
```python
class DrawingAPI:
    def draw_circle(self, x, y, r): pass
class DrawingAPI1(DrawingAPI):
    def draw_circle(self, x, y, r): print(f"API1: Drawing circle at {x},{y} radius {r}")
class Circle:
    def __init__(self, x, y, r, api):
        self.x, self.y, self.r, self.api = x, y, r, api
    def draw(self): self.api.draw_circle(self.x, self.y, self.r)
```

**Real-World Example:**
Bridge is used in GUI toolkits to separate platform-independent abstractions from platform-specific implementations.

### Flyweight Pattern
**Intent:** Use sharing to support large numbers of fine-grained objects efficiently.
**What it achieves:**
- Reduces memory footprint by sharing common state among many objects.
- Useful for applications with many similar objects (e.g., text editors, games, caches).
- Separates intrinsic (shared) and extrinsic (unique) state for efficiency.
- **Python Example:**
```python
class Flyweight:
    _instances = {}
    def __new__(cls, key):
        if key not in cls._instances:
            cls._instances[key] = super().__new__(cls)
        return cls._instances[key]
```

**Critical Thinking Prompt:**
How does the Flyweight pattern enable efficient text rendering in editors and games?

---

## Behavioral Patterns

**Advanced Theory:**
- Behavioral patterns decouple sender and receiver, encapsulate requests, and enable dynamic assignment of responsibilities.
- Observer and Mediator are foundational for event-driven and publish/subscribe architectures.

### Observer Pattern
**Intent:** Define a one-to-many dependency so that when one object changes state, all its dependents are notified and updated automatically.
**What it achieves:**
- Enables event-driven programming and decouples subjects from observers.
- Supports dynamic subscription and notification of changes.
- Useful for GUIs, data binding, and publish/subscribe systems.
- **Python Example:**
```python
class Subject:
    def __init__(self): self.observers = []
    def attach(self, obs): self.observers.append(obs)
    def notify(self, msg):
        for obs in self.observers:
            obs.update(msg)
class Observer:
    def update(self, msg): print(f"Received: {msg}")
```

**Real-World Example:**
GUI frameworks (e.g., Qt, Tkinter) use Observer for event handling and data binding.

### Strategy Pattern
**Intent:** Define a family of algorithms, encapsulate each one, and make them interchangeable at runtime.
**What it achieves:**
- Allows the algorithm to vary independently from clients that use it.
- Promotes code reuse and flexibility by separating algorithm selection from implementation.
- Useful for sorting, validation, payment processing, and runtime configuration.
- **Python Example:**
```python
def quicksort(arr): return sorted(arr)
def bubblesort(arr): return sorted(arr)  # Placeholder
class Sorter:
    def __init__(self, strategy): self.strategy = strategy
    def sort(self, arr): return self.strategy(arr)
```

**Critical Thinking Prompt:**
How does the Strategy pattern support runtime configuration of algorithms in data processing pipelines?

### Command Pattern
**Intent:** Encapsulate a request as an object, thereby allowing for parameterization, queuing, logging, and undoable operations.
**What it achieves:**
- Decouples the sender and receiver of a request.
- Enables support for undo/redo, macro recording, and transactional behavior.
- Useful for GUI actions, task scheduling, and workflow engines.
- **Python Example:**
```python
class Command:
    def execute(self): pass
class LightOnCommand(Command):
    def execute(self): print("Light on")
class Invoker:
    def __init__(self, cmd): self.cmd = cmd
    def run(self): self.cmd.execute()
```

**Advanced Usage:**
Command pattern is used in undo/redo systems, macro recording, and GUI action handling (e.g., menu commands).

### State Pattern
**Intent:** Allow an object to alter its behavior when its internal state changes, appearing to change its class.
**What it achieves:**
- Encapsulates state-specific behavior and transitions.
- Simplifies code by removing complex conditional logic.
- Useful for state machines, workflow engines, and protocol implementations.
- **Python Example:**
```python
class State:
    def handle(self): pass
class OnState(State):
    def handle(self): print("On")
class OffState(State):
    def handle(self): print("Off")
class Context:
    def __init__(self, state): self.state = state
    def request(self): self.state.handle()
```

**Real-World Example:**
State machines in workflow engines and game development use this pattern to manage transitions.

### Template Method Pattern
**Intent:** Define the skeleton of an algorithm in a method, deferring some steps to subclasses.
**What it achieves:**
- Promotes code reuse by capturing invariant parts of an algorithm.
- Allows subclasses to customize specific steps without changing the overall structure.
- Useful for frameworks, data processing pipelines, and extensible libraries.
- **Python Example:**
```python
class AbstractClass:
    def template_method(self):
        self.step1()
        self.step2()
    def step1(self): pass
    def step2(self): pass
```

**Critical Thinking Prompt:**
How does the Template Method pattern enable framework extensibility?

### Chain of Responsibility Pattern
**Intent:** Allow a request to be passed along a chain of handlers until one handles it.
**What it achieves:**
- Decouples senders and receivers by giving multiple objects a chance to handle a request.
- Supports flexible and dynamic assignment of responsibilities.
- Useful for event propagation, middleware, and validation chains.
- **Python Example:**
```python
class Handler:
    def __init__(self, successor=None): self.successor = successor
    def handle(self, req):
        if self.successor: self.successor.handle(req)
```

**Advanced Usage:**
Middleware in web frameworks (e.g., Django, Express.js) use this pattern for request/response processing.

### Mediator Pattern
**Intent:** Define an object that encapsulates how a set of objects interact, promoting loose coupling.
**What it achieves:**
- Centralizes complex communications and control logic between objects.
- Reduces direct dependencies and simplifies maintenance.
- Useful for GUI dialogs, chat systems, and workflow coordination.
- **Python Example:**
```python
class Mediator:
    def notify(self, sender, event): pass
class Component:
    def __init__(self, mediator): self.mediator = mediator
```

**Real-World Example:**
Chat systems and GUI dialog management use Mediator to centralize communication.

### Memento Pattern
**Intent:** Capture and externalize an object's internal state so that it can be restored later, without violating encapsulation.
**What it achieves:**
- Enables undo/redo and checkpoint functionality.
- Preserves encapsulation by hiding implementation details from other objects.
- Useful for editors, games, and transactional systems.
- **Python Example:**
```python
class Memento:
    def __init__(self, state): self.state = state
class Originator:
    def __init__(self): self.state = None
    def save(self): return Memento(self.state)
    def restore(self, m): self.state = m.state
```

**Critical Thinking Prompt:**
How does the Memento pattern support version control and undo/redo features?

### Visitor Pattern
**Intent:** Represent an operation to be performed on elements of an object structure, allowing new operations without changing the classes of the elements.
**What it achieves:**
- Separates algorithms from object structures.
- Facilitates adding new operations without modifying existing classes.
- Useful for compilers, interpreters, and reporting systems.
- **Python Example:**
```python
class Visitor:
    def visit(self, element): pass
class Element:
    def accept(self, visitor): visitor.visit(self)
```

**Advanced Usage:**
Compilers and interpreters use Visitor for traversing and operating on abstract syntax trees (ASTs).

### Iterator Pattern
**Intent:** Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
**What it achieves:**
- Enables uniform traversal of different data structures.
- Promotes encapsulation by hiding collection internals.
- Useful for collections, data streams, and custom containers.
- **Python Example:**
```python
class Iterator:
    def __init__(self, collection): self._collection = collection; self._index = 0
    def __next__(self):
        if self._index < len(self._collection):
            val = self._collection[self._index]
            self._index += 1
            return val
        raise StopIteration
    def __iter__(self): return self
```

**Real-World Example:**
Python's built-in iterators and generators are practical applications of this pattern.

---

## Modern Pattern Applications
- Patterns in functional, concurrent, and distributed programming
- Microservices and cloud-native patterns
- Framework-specific patterns (MVC, MVVM, Dependency Injection, Repository)

**Research Trend:**
- Patterns for reactive programming (e.g., Observer, Command) are being adapted for event-driven and serverless architectures.

---

## Anti-Patterns and Code Smells
- Common anti-patterns: God Object, Spaghetti Code, Golden Hammer, etc.
- Code smells: Long Method, Large Class, Duplicate Code, etc.
- Refactoring techniques: Extract Method, Move Method, Replace Conditional with Polymorphism

**Advanced Insights:**
- Anti-patterns are often the result of misunderstood or misapplied patterns.
- Automated tools (e.g., SonarQube, pylint) can detect code smells and suggest refactorings.

**Critical Thinking Prompt:**
How can regular code reviews and static analysis help prevent anti-patterns in large teams?

---

## Pattern Selection and Implementation
- Choosing the right pattern: problem analysis, trade-offs, combinations
- Best practices: prefer composition, program to interfaces, testability

**Advanced Theory:**
- Pattern selection should consider system requirements, performance, maintainability, and team expertise.
- Patterns can be combined (e.g., Composite + Iterator in tree structures).

---

## Practical Pattern Examples
- Real-world case studies: e-commerce, games, web apps
- Pattern-based design exercises
- Refactoring legacy code to use patterns

### Case Study: E-Commerce Checkout System
- **Scenario:** An e-commerce platform needs a flexible checkout process supporting multiple payment methods, discounts, and promotions.
- **Patterns Used:** Strategy (for payment algorithms), Decorator (for discounts), Command (for order actions), Observer (for inventory updates).
- **Outcome:** The system is extensible, testable, and easy to maintain as new features are added.

### Research Trend: Pattern Mining in Open Source
- Automated tools are being developed to detect and recommend patterns in large codebases, aiding in software modernization.

---

## Review Questions
1. What is the difference between a design pattern and an architectural pattern?
2. Give an example of when to use the Builder pattern.
3. How does the Decorator pattern differ from inheritance?
4. What are the main benefits of the Observer pattern?
5. Identify a code smell and suggest a refactoring technique.

6. How can the Strategy pattern be used in machine learning pipelines?
7. What are the risks of overusing the Singleton pattern?
8. How do anti-patterns emerge in agile development environments?
9. Discuss the role of design patterns in microservices architectures.
10. How can automated tools assist in pattern detection and code quality improvement?

---

## Answers and Explanations
1. **Design patterns** address class/object-level design; **architectural patterns** address system-level structure.
2. Use Builder when constructing complex objects step by step (e.g., building a pizza with various toppings).
3. Decorator adds behavior at runtime; inheritance adds at compile time and is less flexible.
4. Observer enables decoupled event notification, supporting extensibility and modularity.
5. Example: Long Method; refactor by Extract Method.

6. Strategy enables dynamic selection of preprocessing, feature extraction, and model algorithms in ML workflows.
7. Overuse of Singleton can lead to hidden dependencies, global state, and testing difficulties.
8. Anti-patterns can arise from rapid prototyping, lack of documentation, or unclear requirements in agile teams.
9. Patterns like Proxy, Adapter, and Observer are essential for service communication, monitoring, and extensibility in microservices.
10. Automated tools can identify pattern usage, code smells, and suggest refactorings, improving maintainability and consistency.

---

## Further Reading and Multimedia Resources
- [Design Patterns: Elements of Reusable Object-Oriented Software (GoF)](https://www.oreilly.com/library/view/design-patterns-elements/0201633612/)
- [Refactoring Guru: Design Patterns](https://refactoring.guru/design-patterns)
- [YouTube: "Design Patterns in Python" (Corey Schafer)](https://www.youtube.com/watch?v=JeznW_7DlB0)
- [YouTube: "Design Patterns - Computerphile"](https://www.youtube.com/watch?v=NU_1StN5Tkk)
- [Martin Fowler: Patterns](https://martinfowler.com/eaaCatalog/)

- [Automated Detection of Design Patterns in Source Code (IEEE TSE, 2023)](https://ieeexplore.ieee.org/document/10098765)
- [Pattern-Oriented Software Architecture (POSA) Series](https://www.wiley.com/en-us/Pattern+Oriented+Software+Architecture+Series-c-3126)
- [YouTube: "Anti-Patterns in Software Design" (Academind)](https://www.youtube.com/watch?v=JrvsDJm6ZzI)
- [SonarQube: Continuous Code Quality](https://www.sonarqube.org/)

---

*End of 4.3 Software Design Patterns*
