# Python Advanced 5.1: Advanced Web Development

## Table of Contents
- [Introduction](#introduction)
- [Advanced Django/Flask](#advanced-djangoflask)
  - [Custom Middleware](#custom-middleware)
  - [Advanced Authentication and Authorization](#advanced-authentication-and-authorization)
  - [Caching Strategies](#caching-strategies)
  - [WebSocket Implementation](#websocket-implementation)
- [Microservices](#microservices)
  - [Service-Oriented Architecture](#service-oriented-architecture)
  - [API Gateways](#api-gateways)
  - [Message Queues (Celery, RabbitMQ)](#message-queues-celery-rabbitmq)
  - [Distributed Systems Concepts](#distributed-systems-concepts)
- [Review Questions](#review-questions)
- [Answers to Review Questions](#answers-to-review-questions)

---

## Introduction
Advanced web development in Python involves building scalable, maintainable, and high-performance web applications and services. This module covers advanced topics in Django and Flask, real-time communication, microservices, and distributed systems, with a focus on best practices and production-grade solutions.

## Advanced Django/Flask

### Custom Middleware
- Middleware allows you to process requests and responses globally.
- Use cases: logging, authentication, request throttling, error handling.
- Example (Django):
  ```python
  class CustomHeaderMiddleware:
      def __init__(self, get_response):
          self.get_response = get_response
      def __call__(self, request):
          response = self.get_response(request)
          response['X-Custom-Header'] = 'Value'
          return response
  ```
- Example (Flask):
  ```python
  from flask import Flask, request
  app = Flask(__name__)
  @app.before_request
def before():
      # Custom logic here
      pass
  ```

### Advanced Authentication and Authorization
- OAuth2, JWT, SSO, and custom permission systems
- Django: `django-rest-framework`, `django-oauth-toolkit`
- Flask: `flask-jwt-extended`, `Authlib`
- Role-based access control (RBAC), multi-factor authentication (MFA)

### Caching Strategies
- Use Redis, Memcached for in-memory caching
- Django: `django-redis`, Flask: `Flask-Caching`
- Cache views, templates, querysets, and API responses
- Invalidate cache on data changes

### WebSocket Implementation
- Real-time communication for chat, notifications, live updates
- Django: `channels`, Flask: `flask-socketio`
- Use cases: collaborative apps, dashboards, games

## Microservices

### Service-Oriented Architecture
- Decompose monoliths into independent, loosely coupled services
- Each service has its own database and API
- Enables independent deployment and scaling

### API Gateways
- Central entry point for all client requests
- Handles routing, authentication, rate limiting, and aggregation
- Tools: Kong, NGINX, AWS API Gateway

### Message Queues (Celery, RabbitMQ)
- Asynchronous task processing and communication between services
- Celery: distributed task queue for Python
- RabbitMQ, Redis, Kafka as brokers
- Use cases: background jobs, notifications, ETL pipelines

### Distributed Systems Concepts
- Service discovery, load balancing, fault tolerance
- Circuit breakers, retries, timeouts
- Observability: logging, metrics, tracing
- Data consistency and eventual consistency

## Review Questions
1. What is middleware and how is it used in Django and Flask?
2. Describe advanced authentication mechanisms for web applications.
3. How does caching improve web application performance?
4. What are WebSockets and when should you use them?
5. What are the benefits of a microservices architecture?
6. How do API gateways enhance security and scalability?
7. What role do message queues play in distributed systems?
8. Explain the importance of observability in microservices.

## Answers to Review Questions
1. Middleware is code that processes requests/responses globally. In Django, it's a class in the middleware stack; in Flask, it's functions registered with hooks like `before_request`.
2. Advanced authentication includes OAuth2, JWT, SSO, and RBAC. These provide secure, flexible, and scalable access control.
3. Caching stores frequently accessed data in memory, reducing database load and improving response times. It can be applied at various levels (view, template, data).
4. WebSockets enable real-time, bidirectional communication between client and server, useful for chat, notifications, and live updates.
5. Microservices allow independent development, deployment, and scaling of services, improving agility and fault isolation.
6. API gateways centralize routing, authentication, and rate limiting, improving security and scalability.
7. Message queues enable asynchronous processing and decouple services, increasing reliability and scalability.
8. Observability (logging, metrics, tracing) is essential for monitoring, debugging, and maintaining distributed systems.
