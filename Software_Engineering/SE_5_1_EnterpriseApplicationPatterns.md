# 5.1 Enterprise Application Patterns

## Table of Contents
- [Introduction to Enterprise Patterns](#introduction-to-enterprise-patterns)
- [Domain Logic Patterns](#domain-logic-patterns)
  - [Transaction Script Pattern](#transaction-script-pattern)
  - [Domain Model Pattern](#domain-model-pattern)
  - [Table Module Pattern](#table-module-pattern)
  - [Service Layer Pattern](#service-layer-pattern)
- [Data Source Architectural Patterns](#data-source-architectural-patterns)
  - [Repository Pattern](#repository-pattern)
  - [Data Access Object (DAO) Pattern](#data-access-object-dao-pattern)
  - [Active Record Pattern](#active-record-pattern)
  - [Data Mapper Pattern](#data-mapper-pattern)
- [Object-Relational Behavioral Patterns](#object-relational-behavioral-patterns)
  - [Unit of Work Pattern](#unit-of-work-pattern)
  - [Identity Map Pattern](#identity-map-pattern)
  - [Lazy Loading Patterns](#lazy-loading-patterns)
- [Advanced Examples and Case Studies](#advanced-examples-and-case-studies)
- [Review Questions](#review-questions)
- [Answers and Explanations](#answers-and-explanations)
- [Further Reading and Multimedia Resources](#further-reading-and-multimedia-resources)

---

## Introduction to Enterprise Patterns

Enterprise application patterns address recurring problems in the design and implementation of large-scale business systems. They help manage complexity, ensure maintainability, and support scalability.

**Key Concepts:**
- Separation of concerns between domain logic, data access, and service layers
- Encapsulation of business rules and workflows
- Reusability and testability of components

**Advanced Perspective:**
- Modern enterprise patterns are foundational for microservices, DDD, and cloud-native architectures.
- Patterns are often combined for robust, maintainable solutions (e.g., Repository + Unit of Work).

**Critical Thinking Prompt:**
How do enterprise patterns support the evolution of legacy systems to modern architectures?

---

## Domain Logic Patterns

### Transaction Script Pattern
- **Intent:** Organize business logic as a set of procedures (scripts), each handling a single transaction or workflow.
- **What it achieves:**
  - Simple, procedural approach for small or legacy systems
  - Easy to implement and understand
- **Limitations:**
  - Can lead to code duplication and poor maintainability as complexity grows
- **Python Example:**
```python
def transfer_funds(account_from, account_to, amount):
    if account_from.balance >= amount:
        account_from.balance -= amount
        account_to.balance += amount
```

### Domain Model Pattern
- **Intent:** Encapsulate business logic in rich domain objects with behavior and state.
- **What it achieves:**
  - Promotes object-oriented design and encapsulation
  - Supports complex business rules and validation
- **Advanced Usage:**
  - Enables DDD, testability, and code reuse
- **Python Example:**
```python
class Account:
    def __init__(self, balance):
        self.balance = balance
    def transfer(self, other, amount):
        if self.balance >= amount:
            self.balance -= amount
            other.balance += amount
```

### Table Module Pattern
- **Intent:** Use a single class to handle all rows in a database table, rather than one object per row.
- **What it achieves:**
  - Centralizes business logic for a table
  - Useful for data-centric or reporting applications
- **Python Example:**
```python
class EmployeeTable:
    def __init__(self, db):
        self.db = db
    def get_salary(self, emp_id):
        return self.db.query_salary(emp_id)
```

### Service Layer Pattern
- **Intent:** Define a layer that encapsulates application business logic, exposing services to clients.
- **What it achieves:**
  - Decouples business logic from presentation and data access
  - Supports transaction management and security
- **Python Example:**
```python
class PaymentService:
    def transfer(self, from_acc, to_acc, amount):
        # ...business logic, validation, transaction...
        pass
```

---

## Data Source Architectural Patterns

### Repository Pattern
- **Intent:** Encapsulate data access logic, providing a collection-like interface for domain objects.
- **What it achieves:**
  - Decouples domain logic from data storage
  - Supports testability and swapping data sources
- **Python Example:**
```python
class AccountRepository:
    def __init__(self, db):
        self.db = db
    def get(self, acc_id):
        # ...fetch from db...
        pass
    def save(self, account):
        # ...save to db...
        pass
```

### Data Access Object (DAO) Pattern
- **Intent:** Abstract and encapsulate all access to the data source.
- **What it achieves:**
  - Provides a uniform API for data operations
  - Supports multiple data sources and persistence strategies
- **Python Example:**
```python
class EmployeeDAO:
    def __init__(self, db):
        self.db = db
    def find_by_id(self, emp_id):
        # ...query db...
        pass
```

### Active Record Pattern
- **Intent:** Wrap a database row with an object, combining data and behavior.
- **What it achieves:**
  - Simplifies CRUD operations
  - Common in ORMs (e.g., Django, Rails)
- **Python Example:**
```python
class User:
    def save(self):
        # ...save self to db...
        pass
    @classmethod
    def find(cls, user_id):
        # ...fetch from db...
        pass
```

### Data Mapper Pattern
- **Intent:** Separate domain objects from database access by using a mapper class.
- **What it achieves:**
  - Decouples domain logic from persistence
  - Supports complex mapping and testability
- **Python Example:**
```python
class UserMapper:
    def __init__(self, db):
        self.db = db
    def load(self, user_id):
        # ...map db row to User object...
        pass
    def save(self, user):
        # ...map User object to db row...
        pass
```

---

## Object-Relational Behavioral Patterns

### Unit of Work Pattern
- **Intent:** Track changes to objects during a business transaction and coordinate the writing out of changes.
- **What it achieves:**
  - Ensures atomicity and consistency
  - Reduces database round-trips
- **Python Example:**
```python
class UnitOfWork:
    def __init__(self):
        self.new_objects = []
        self.dirty_objects = []
        self.removed_objects = []
    def commit(self):
        # ...persist all changes...
        pass
```

### Identity Map Pattern
- **Intent:** Ensure each object is loaded only once per session, maintaining object identity.
- **What it achieves:**
  - Prevents duplicate objects in memory
  - Supports caching and consistency
- **Python Example:**
```python
class IdentityMap:
    def __init__(self):
        self._objects = {}
    def get(self, obj_id):
        return self._objects.get(obj_id)
    def add(self, obj_id, obj):
        self._objects[obj_id] = obj
```

### Lazy Loading Patterns
- **Intent:** Delay the loading of an object or collection until it is needed.
- **What it achieves:**
  - Improves performance and resource usage
  - Supports large datasets and complex object graphs
- **Python Example:**
```python
class LazyLoader:
    def __init__(self, load_func):
        self._load_func = load_func
        self._loaded = False
        self._value = None
    def get(self):
        if not self._loaded:
            self._value = self._load_func()
            self._loaded = True
        return self._value
```

---

## Advanced Examples and Case Studies

### Case Study 1: Refactoring a Monolithic ERP System
- **Scenario:** A legacy ERP system suffers from tight coupling and slow feature delivery.
- **Solution:**
  - Introduce Service Layer and Repository patterns to decouple business logic and data access.
  - Gradually migrate to Domain Model and Unit of Work for complex workflows.
- **Outcome:** Improved maintainability, testability, and readiness for microservices migration.

### Case Study 2: Building a Scalable E-Commerce Platform
- **Scenario:** An online retailer needs to support rapid growth and high transaction volumes.
- **Solution:**
  - Use Repository, Identity Map, and Lazy Loading for efficient data access.
  - Apply Service Layer for business logic and transaction management.
- **Outcome:** High scalability, robust data integrity, and easier onboarding for new developers.

---

## Review Questions
1. What are the main differences between Transaction Script and Domain Model patterns?
2. How does the Repository pattern improve testability?
3. When would you use Active Record vs. Data Mapper?
4. What problems does the Unit of Work pattern solve?
5. How does Lazy Loading benefit large-scale applications?
6. Explain the role of Identity Map in object-relational mapping.
7. How can Service Layer support migration to microservices?
8. Discuss the trade-offs of using Table Module in modern systems.
9. How do these patterns relate to Domain-Driven Design (DDD)?
10. Give a real-world example of combining multiple enterprise patterns.

---

## Answers and Explanations
1. **Transaction Script** is procedural and simple, best for small apps; **Domain Model** encapsulates business logic in objects, better for complex domains.
2. Repository decouples domain logic from data access, enabling use of mocks/stubs in tests.
3. **Active Record** is simple and direct, best for CRUD-heavy apps; **Data Mapper** is better for complex domains and testability.
4. Unit of Work coordinates changes and ensures atomic commits, reducing database round-trips.
5. Lazy Loading defers expensive operations, improving performance for large datasets.
6. Identity Map ensures object uniqueness and supports caching in ORM systems.
7. Service Layer centralizes business logic, making it easier to split into microservices.
8. Table Module is simple but can become a bottleneck; Domain Model is preferred for complex logic.
9. These patterns support DDD by structuring code around business concepts and boundaries.
10. E-commerce: Repository + Unit of Work for orders, Service Layer for checkout, Lazy Loading for product images.

---

## Further Reading and Multimedia Resources
- [Patterns of Enterprise Application Architecture (Martin Fowler)](https://martinfowler.com/books/eaa.html)
- [Domain-Driven Design (Eric Evans)](https://domainlanguage.com/ddd/)
- [YouTube: "Enterprise Application Patterns" (Academind)](https://www.youtube.com/watch?v=2yko4TbC8cI)
- [YouTube: "Repository Pattern Explained" (CodeOpinion)](https://www.youtube.com/watch?v=rtXpYpZdOzM)
- [YouTube: "Unit of Work Pattern" (Nick Chapsas)](https://www.youtube.com/watch?v=6vQ5B7F2g0g)
- [Microsoft Docs: Data Access Patterns](https://docs.microsoft.com/en-us/azure/architecture/patterns/data-access)

---

*End of 5.1 Enterprise Application Patterns*
