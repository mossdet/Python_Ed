# Python Intermediate 2.3: Web Development and APIs

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Web Frameworks](#2-web-frameworks)
  - [2.1 Flask Basics: Routes, Templates, Forms](#21-flask-basics-routes-templates-forms)
  - [2.2 Django Introduction: Models, Views, Templates](#22-django-introduction-models-views-templates)
  - [2.3 RESTful API Concepts](#23-restful-api-concepts)
- [3. Working with APIs](#3-working-with-apis)
  - [3.1 HTTP Requests (requests library)](#31-http-requests-requests-library)
  - [3.2 JSON Handling](#32-json-handling)
  - [3.3 API Authentication](#33-api-authentication)
  - [3.4 Creating Simple APIs](#34-creating-simple-apis)
- [4. Practical Examples](#4-practical-examples)
- [5. Review Questions](#5-review-questions)
- [6. Answers to Review Questions](#6-answers-to-review-questions)

---

## 1. Introduction

This module introduces web development in Python using popular frameworks and covers the fundamentals of working with APIs for modern applications.

## 2. Web Frameworks

### 2.1 Flask Basics: Routes, Templates, Forms
Flask is a lightweight web framework.
```python
from flask import Flask, render_template, request
app = Flask(__name__)
@app.route('/')
def home():
    return 'Hello, Flask!'
@app.route('/greet', methods=['GET', 'POST'])
def greet():
    if request.method == 'POST':
        name = request.form['name']
        return f'Hello, {name}!'
    return '''<form method="post"><input name="name"><input type="submit"></form>'''
if __name__ == '__main__':
    app.run(debug=True)
```
Templates (HTML files) are stored in a `templates/` folder and rendered with `render_template()`.

### 2.2 Django Introduction: Models, Views, Templates
Django is a full-featured web framework.
- **Models** define data structure:
```python
from django.db import models
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
```
- **Views** handle logic:
```python
def post_list(request):
    posts = Post.objects.all()
    return render(request, 'post_list.html', {'posts': posts})
```
- **Templates** are HTML files with Django template language.

### 2.3 RESTful API Concepts
REST (Representational State Transfer) is an architectural style for APIs.
- Uses HTTP methods: GET, POST, PUT, DELETE
- Stateless, resource-based

## 3. Working with APIs

### 3.1 HTTP Requests (requests library)
The `requests` library is used for HTTP communication.
```python
import requests
response = requests.get('https://api.github.com')
print(response.status_code)
print(response.json())
```

### 3.2 JSON Handling
APIs often use JSON for data exchange.
```python
import json
# Convert Python dict to JSON string
json_str = json.dumps({'key': 'value'})
# Parse JSON string to Python dict
data = json.loads(json_str)
```

### 3.3 API Authentication
Common methods:
- API keys (passed in headers or query params)
- OAuth (token-based)
```python
headers = {'Authorization': 'Bearer <token>'}
requests.get('https://api.example.com/data', headers=headers)
```

### 3.4 Creating Simple APIs
Flask can be used to create APIs:
```python
from flask import Flask, jsonify, request
app = Flask(__name__)
@app.route('/api/square', methods=['POST'])
def square():
    data = request.get_json()
    n = data['number']
    return jsonify({'result': n*n})
```

## 4. Practical Examples

### Example: Simple Weather API Client
```python
import requests
city = 'London'
url = f'https://api.openweathermap.org/data/2.5/weather?q={city}&appid=YOUR_API_KEY'
response = requests.get(url)
data = response.json()
print(data['weather'][0]['description'])
```

### Example: Flask RESTful API
```python
from flask import Flask, jsonify
app = Flask(__name__)
@app.route('/api/hello')
def hello():
    return jsonify({'message': 'Hello, API!'})
```

## 5. Review Questions
1. What are the main differences between Flask and Django?
2. How do you make a GET request to an API in Python?
3. What is REST and why is it important for APIs?
4. How can you secure an API endpoint?
5. How do you parse JSON data from an API response?

## 6. Answers to Review Questions
1. **Flask is lightweight and flexible; Django is full-featured and follows a strict structure.**
2. **Use the `requests.get()` function with the API URL.**
3. **REST is an architectural style for stateless, resource-based APIs, enabling scalable web services.**
4. **Use authentication (API keys, OAuth), HTTPS, and proper access controls.**
5. **Use `response.json()` or the `json` module to parse JSON data.**
