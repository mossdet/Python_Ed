# Python Intermediate 2.2: File Handling and Databases

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Advanced File Operations](#2-advanced-file-operations)
  - [2.1 Working with CSV Files](#21-working-with-csv-files)
  - [2.2 Working with JSON Files](#22-working-with-json-files)
  - [2.3 Working with XML Files](#23-working-with-xml-files)
  - [2.4 Binary File Operations](#24-binary-file-operations)
  - [2.5 Context Managers and `with` Statements](#25-context-managers-and-with-statements)
- [3. Database Interaction](#3-database-interaction)
  - [3.1 SQLite with Python](#31-sqlite-with-python)
  - [3.2 Database Design Principles](#32-database-design-principles)
  - [3.3 CRUD Operations](#33-crud-operations)
  - [3.4 Introduction to ORMs (SQLAlchemy Basics)](#34-introduction-to-orms-sqlalchemy-basics)
- [4. Practical Examples](#4-practical-examples)
- [5. Review Questions](#5-review-questions)
- [6. Answers to Review Questions](#6-answers-to-review-questions)

---

## 1. Introduction

This module explores advanced file operations and database management in Python, essential for data-driven applications and persistent storage.

## 2. Advanced File Operations

### 2.1 Working with CSV Files
CSV (Comma-Separated Values) is a common format for tabular data.
```python
import csv
with open('data.csv', 'r') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
```
Writing to CSV:
```python
with open('output.csv', 'w', newline='') as f:
    writer = csv.writer(f)
    writer.writerow(['Name', 'Age'])
    writer.writerow(['Alice', 30])
```

### 2.2 Working with JSON Files
JSON (JavaScript Object Notation) is widely used for data interchange.
```python
import json
# Reading JSON
with open('data.json', 'r') as f:
    data = json.load(f)
# Writing JSON
with open('output.json', 'w') as f:
    json.dump({'name': 'Bob', 'age': 25}, f)
```

### 2.3 Working with XML Files
XML is used for structured data exchange.
```python
import xml.etree.ElementTree as ET
# Parsing XML
root = ET.parse('data.xml').getroot()
for child in root:
    print(child.tag, child.attrib)
```

### 2.4 Binary File Operations
Binary files store non-text data (images, executables).
```python
with open('image.png', 'rb') as f:
    data = f.read()
with open('copy.png', 'wb') as f:
    f.write(data)
```

### 2.5 Context Managers and `with` Statements
Context managers ensure resources are managed safely.
```python
with open('file.txt', 'r') as f:
    content = f.read()
# File is automatically closed
```
Custom context manager:
```python
class ManagedFile:
    def __init__(self, filename):
        self.filename = filename
    def __enter__(self):
        self.file = open(self.filename, 'r')
        return self.file
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
with ManagedFile('file.txt') as f:
    print(f.read())
```

## 3. Database Interaction

### 3.1 SQLite with Python
SQLite is a lightweight, file-based database.
```python
import sqlite3
conn = sqlite3.connect('example.db')
c = conn.cursor()
c.execute('CREATE TABLE IF NOT EXISTS users (id INTEGER, name TEXT)')
c.execute('INSERT INTO users VALUES (?, ?)', (1, 'Alice'))
conn.commit()
for row in c.execute('SELECT * FROM users'):
    print(row)
conn.close()
```

### 3.2 Database Design Principles
- Normalize data to reduce redundancy
- Use primary keys for unique identification
- Design for scalability and maintainability

### 3.3 CRUD Operations
CRUD: Create, Read, Update, Delete
```python
# Create
c.execute('INSERT INTO users VALUES (?, ?)', (2, 'Bob'))
# Read
c.execute('SELECT * FROM users')
# Update
c.execute('UPDATE users SET name=? WHERE id=?', ('Charlie', 2))
# Delete
c.execute('DELETE FROM users WHERE id=?', (2,))
conn.commit()
```

### 3.4 Introduction to ORMs (SQLAlchemy Basics)
ORMs (Object-Relational Mappers) abstract database operations.
```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
engine = create_engine('sqlite:///example.db')
Base = declarative_base()
class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String)
Base.metadata.create_all(engine)
Session = sessionmaker(bind=engine)
session = Session()
user = User(id=3, name='Dave')
session.add(user)
session.commit()
```

## 4. Practical Examples

### Example: CSV to SQLite Importer
```python
import csv, sqlite3
conn = sqlite3.connect('import.db')
c = conn.cursor()
c.execute('CREATE TABLE IF NOT EXISTS data (name TEXT, age INTEGER)')
with open('data.csv', 'r') as f:
    reader = csv.reader(f)
    for row in reader:
        c.execute('INSERT INTO data VALUES (?, ?)', (row[0], int(row[1])))
conn.commit()
conn.close()
```

### Example: JSON Configuration Loader
```python
import json
with open('config.json', 'r') as f:
    config = json.load(f)
print(config)
```

## 5. Review Questions
1. What are the advantages of using context managers for file operations?
2. How does SQLite differ from other database systems?
3. What is the purpose of an ORM?
4. Describe the CRUD operations in SQL.
5. How would you import data from a CSV file into a database using Python?

## 6. Answers to Review Questions
1. **They ensure resources are managed safely and files are closed automatically.**
2. **SQLite is file-based, lightweight, and requires no server setup.**
3. **ORMs abstract database operations, allowing you to interact with databases using Python objects.**
4. **Create, Read, Update, Delete: fundamental operations for managing database records.**
5. **Read the CSV file, parse rows, and insert them into the database using SQL commands.**
