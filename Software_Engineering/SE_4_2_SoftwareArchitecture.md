# 4.2 Software Architecture

## Table of Contents
- [Introduction to Software Architecture](#introduction-to-software-architecture)
- [Architectural Styles and Patterns](#architectural-styles-and-patterns)
- [Layered Architecture](#layered-architecture)
- [Client-Server Architecture](#client-server-architecture)
- [Microservices Architecture](#microservices-architecture)
- [Service-Oriented Architecture (SOA)](#service-oriented-architecture-soa)
- [Advanced Examples and Case Studies](#advanced-examples-and-case-studies)
- [Review Questions](#review-questions)
- [Answers and Explanations](#answers-and-explanations)
- [Further Reading and Multimedia Resources](#further-reading-and-multimedia-resources)

---

## Introduction to Software Architecture

**Definition:** Software architecture is the high-level structure of a software system, encompassing the set of significant decisions about the organization of the system, selection of structural elements and their interfaces, and the behavior of these elements.

**Importance:**
- Provides a blueprint for system construction and evolution
- Facilitates communication among stakeholders
- Enables reasoning about system qualities (performance, scalability, security, etc.)
- Supports maintainability and extensibility

**Key Concepts:**
- Architectural decisions
- Views and viewpoints (logical, physical, process, development)
- Architectural documentation (C4 model, UML, ADRs)

---

## Architectural Styles and Patterns

**Architectural Style:** A family of architectures sharing certain characteristics (e.g., layered, client-server, microservices).

**Architectural Pattern:** A reusable solution to a recurring architectural problem (e.g., MVC, event-driven, pipe-and-filter).

**Common Styles and Patterns:**
- Layered
- Client-Server
- Microservices
- Service-Oriented Architecture (SOA)
- Event-Driven
- Pipe-and-Filter
- Broker
- Peer-to-Peer

---

## Layered Architecture

**Description:** Organizes the system into layers with well-defined responsibilities and strict rules for interaction (e.g., presentation, business logic, data access).

**Advantages:**
- Separation of concerns
- Easier maintenance and testing
- Supports incremental development

**Disadvantages:**
- Potential performance overhead
- Rigid structure can hinder flexibility

**Example:**
- Web applications (MVC: Model-View-Controller)

**Python Example:**
```python
# Simple layered architecture example
class DataAccessLayer:
    def get_user(self, user_id):
        # Simulate database access
        return {"id": user_id, "name": "Alice"}

class BusinessLogicLayer:
    def __init__(self, dal):
        self.dal = dal
    def get_user_profile(self, user_id):
        user = self.dal.get_user(user_id)
        return f"Profile: {user['name']} (ID: {user['id']})"

class PresentationLayer:
    def __init__(self, bll):
        self.bll = bll
    def display_user(self, user_id):
        print(self.bll.get_user_profile(user_id))

# Usage
if __name__ == "__main__":
    dal = DataAccessLayer()
    bll = BusinessLogicLayer(dal)
    pl = PresentationLayer(bll)
    pl.display_user(1)
```

---

## Client-Server Architecture

**Description:** Divides the system into clients (requesters of services) and servers (providers of services).

**Advantages:**
- Centralized control and management
- Scalability (multiple clients)
- Security (centralized data)

**Disadvantages:**
- Single point of failure (server)
- Network dependency

**Example:**
- Web browsers (clients) and web servers

**Python Example:**
```python
# Simple client-server using sockets
import socket

# Server
# ...existing code...
# Client
# ...existing code...
```
*See advanced code in the case studies section below.*

---

## Microservices Architecture

**Description:** Structures an application as a collection of loosely coupled, independently deployable services, each responsible for a specific business capability.

**Advantages:**
- Scalability and flexibility
- Technology heterogeneity
- Fault isolation
- Independent deployment

**Disadvantages:**
- Increased complexity (distributed system)
- Data consistency challenges
- DevOps and monitoring overhead

**Example:**
- E-commerce platform with separate services for user management, product catalog, orders, payments

**Python Example:**
```python
# Microservices are typically implemented as separate processes/services.
# Example: Flask-based microservice for user management
from flask import Flask, jsonify
app = Flask(__name__)

@app.route('/user/<int:user_id>')
def get_user(user_id):
    return jsonify({"id": user_id, "name": "Alice"})

if __name__ == "__main__":
    app.run(port=5000)
```

---

## Service-Oriented Architecture (SOA)

**Description:** An architectural style where services are provided to other components by application components, through a communication protocol over a network.

**Key Features:**
- Standardized service contracts (WSDL, REST, SOAP)
- Loose coupling
- Service abstraction
- Reusability

**Comparison with Microservices:**
- SOA is broader, often uses ESB (Enterprise Service Bus)
- Microservices are more granular, independently deployable

**Example:**
- Enterprise systems integrating CRM, ERP, and HR services

---

## Advanced Examples and Case Studies

### Case Study 1: Migrating from Monolith to Microservices
- **Scenario:** A legacy e-commerce application faces scalability and deployment challenges.
- **Solution:**
  - Identify business domains (user, product, order, payment)
  - Decompose monolith into microservices
  - Use API Gateway for routing
  - Implement service discovery and monitoring
- **Outcome:** Improved scalability, faster deployments, but increased operational complexity.

### Case Study 2: Layered Architecture in Banking Systems
- **Scenario:** A bank needs to separate concerns for security and maintainability.
- **Solution:**
  - Presentation layer (web/mobile UI)
  - Business logic layer (transaction processing)
  - Data access layer (database, audit logs)
- **Outcome:** Easier updates, better security, but some performance overhead.

### Advanced Python Example: RESTful Microservice with Flask
```python
from flask import Flask, request, jsonify
app = Flask(__name__)

users = {}

@app.route('/users', methods=['POST'])
def create_user():
    data = request.get_json()
    user_id = len(users) + 1
    users[user_id] = data['name']
    return jsonify({"id": user_id, "name": data['name']}), 201

@app.route('/users/<int:user_id>', methods=['GET'])
def get_user(user_id):
    name = users.get(user_id)
    if name:
        return jsonify({"id": user_id, "name": name})
    return jsonify({"error": "User not found"}), 404

if __name__ == "__main__":
    app.run(port=5001)
```

---

## Review Questions
1. What is the difference between an architectural style and an architectural pattern?
2. Describe the main advantages and disadvantages of layered architecture.
3. How does microservices architecture differ from SOA?
4. What are the main challenges in migrating from a monolithic to a microservices architecture?
5. Give a real-world example where client-server architecture is preferred.

---

## Answers and Explanations
1. **Architectural style** is a broad structural approach (e.g., layered, client-server), while an **architectural pattern** is a reusable solution to a specific problem within a style (e.g., MVC in layered architecture).
2. **Advantages:** Separation of concerns, maintainability, testability. **Disadvantages:** Performance overhead, rigidity.
3. **Microservices** are more granular, independently deployable, and often use lightweight protocols. **SOA** is broader, may use ESB, and services are larger in scope.
4. **Challenges:** Service decomposition, data consistency, distributed transactions, monitoring, deployment automation.
5. **Example:** Web applications, where browsers (clients) interact with web servers.

---

## Further Reading and Multimedia Resources
- [Software Architecture Patterns (Martin Fowler)](https://martinfowler.com/architecture/)
- [The Architecture of Open Source Applications](http://aosabook.org/en/index.html)
- [Microservices vs. SOA](https://www.redhat.com/en/topics/microservices/soa-vs-microservices)
- [YouTube: "Software Architecture Fundamentals" (O'Reilly)](https://www.youtube.com/watch?v=2yko4TbC8cI)
- [YouTube: "Microservices Explained" (TechWorld with Nana)](https://www.youtube.com/watch?v=rv4LlmLmVWk)
- [YouTube: "Layered Architecture Pattern" (Academind)](https://www.youtube.com/watch?v=Qkz_Cb2p1aA)

---

*End of 4.2 Software Architecture*
