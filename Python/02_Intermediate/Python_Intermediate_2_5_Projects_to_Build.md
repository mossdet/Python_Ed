# Python Intermediate 2.5: Projects to Build

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Project 1: Personal Finance Tracker](#2-project-1-personal-finance-tracker)
- [3. Project 2: Weather App](#3-project-2-weather-app)
- [4. Project 3: Web Scraper](#4-project-3-web-scraper)
- [5. Project 4: Blog Website](#5-project-4-blog-website)
- [6. Project 5: Task Management API](#6-project-5-task-management-api)
- [7. Review Questions](#7-review-questions)
- [8. Answers to Review Questions](#8-answers-to-review-questions)
- [9. Project Development Workflow and Best Practices](#9-project-development-workflow-and-best-practices)

---

## 1. Introduction

Building real-world projects is essential for consolidating intermediate Python skills. The following projects integrate OOP, file handling, APIs, databases, and web development.

## 2. Project 1: Personal Finance Tracker
- **Skills:** OOP, file handling, data analysis
- **Features:** Add, edit, and delete transactions; categorize expenses; generate reports.
- **Tech:** CSV/SQLite for storage, matplotlib for charts.
- **Sample Structure:**
```python
import csv
from datetime import datetime

class Transaction:
    def __init__(self, date, amount, category, description=""):
        self.date = datetime.strptime(date, "%Y-%m-%d")
        self.amount = float(amount)
        self.category = category
        self.description = description

class Account:
    def __init__(self, name):
        self.name = name
        self.transactions = []
    def add_transaction(self, transaction):
        self.transactions.append(transaction)
    def save_to_csv(self, filename):
        with open(filename, 'w', newline='') as f:
            writer = csv.writer(f)
            writer.writerow(["date", "amount", "category", "description"])
            for t in self.transactions:
                writer.writerow([t.date.strftime("%Y-%m-%d"), t.amount, t.category, t.description])
    def load_from_csv(self, filename):
        with open(filename, 'r') as f:
            reader = csv.DictReader(f)
            for row in reader:
                self.add_transaction(Transaction(row['date'], row['amount'], row['category'], row.get('description', '')))
```
- **Extension:** Add a GUI with Tkinter or a web interface with Flask.

#### Step-by-Step Implementation
1. Design the `Transaction` and `Account` classes.
2. Implement file I/O for CSV/SQLite storage.
3. Build functions for adding, editing, deleting, and listing transactions.
4. Add reporting features (monthly/annual summaries, charts with matplotlib).
5. Write unit tests for all business logic.
6. (Optional) Add a CLI, GUI, or web interface.

#### Key Design Decisions
- Choose between flat-file (CSV) and database (SQLite) based on scalability needs.
- Use pandas for advanced data analysis and reporting.

#### Testing Strategies
- Mock file/database operations in tests.
- Test edge cases: negative amounts, invalid dates, duplicate entries.

#### Real-World Tips
- Encrypt sensitive data if storing locally.
- Allow import/export for easy migration.

### Example: Adding a GUI with Tkinter (Personal Finance Tracker)
```python
import tkinter as tk
from tkinter import ttk, messagebox

class FinanceApp(tk.Tk):
    def __init__(self, account):
        super().__init__()
        self.account = account
        self.title("Personal Finance Tracker")
        self.geometry("400x300")
        self.create_widgets()
    def create_widgets(self):
        self.amount_var = tk.StringVar()
        self.category_var = tk.StringVar()
        ttk.Label(self, text="Amount:").pack()
        ttk.Entry(self, textvariable=self.amount_var).pack()
        ttk.Label(self, text="Category:").pack()
        ttk.Entry(self, textvariable=self.category_var).pack()
        ttk.Button(self, text="Add Transaction", command=self.add_transaction).pack()
        self.listbox = tk.Listbox(self)
        self.listbox.pack(fill=tk.BOTH, expand=True)
        self.refresh_list()
    def add_transaction(self):
        try:
            amount = float(self.amount_var.get())
            category = self.category_var.get()
            t = Transaction(datetime.now().strftime("%Y-%m-%d"), amount, category)
            self.account.add_transaction(t)
            self.refresh_list()
        except Exception as e:
            messagebox.showerror("Error", str(e))
    def refresh_list(self):
        self.listbox.delete(0, tk.END)
        for t in self.account.transactions:
            self.listbox.insert(tk.END, f"{t.date.date()} | {t.amount} | {t.category}")

# Usage:
# account = Account("MyAccount")
# app = FinanceApp(account)
# app.mainloop()
```

## 3. Project 2: Weather App
- **Skills:** API integration, JSON handling
- **Features:** Fetch and display weather data for a city; error handling for invalid input.
- **Tech:** requests, OpenWeatherMap API.
- **Sample Code:**
```python
import requests
import os

def get_weather(city):
    api_key = os.getenv('OPENWEATHER_API_KEY')
    url = f'https://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric'
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        return data['weather'][0]['description'], data['main']['temp']
    else:
        return None, None

if __name__ == "__main__":
    city = input("Enter city: ")
    desc, temp = get_weather(city)
    if desc:
        print(f"Weather: {desc}, Temp: {temp}°C")
    else:
        print("City not found or API error.")
```
- **Extension:** Build a GUI or web dashboard.

#### Step-by-Step Implementation
1. Register for an API key at OpenWeatherMap.
2. Write a function to fetch and parse weather data for a city.
3. Handle errors (invalid city, network issues, API limits).
4. Display results in the console, GUI, or web app.
5. Add support for multiple cities and units.
6. Write tests for API and parsing logic (mock API responses).

#### Key Design Decisions
- Use environment variables for API keys.
- Implement caching to avoid hitting rate limits.

#### Testing Strategies
- Use unittest.mock or pytest to simulate API responses.
- Test for invalid input, network failures, and API changes.

#### Real-World Tips
- Respect API usage policies.
- Provide clear error messages to users.

### Example: Adding a GUI with Tkinter (Weather App)
```python
import tkinter as tk
from tkinter import ttk
import requests
import os

def get_weather(city):
    api_key = os.getenv('OPENWEATHER_API_KEY')
    url = f'https://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric'
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        return data['weather'][0]['description'], data['main']['temp']
    else:
        return None, None

class WeatherApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Weather App")
        self.geometry("300x150")
        self.city_var = tk.StringVar()
        ttk.Label(self, text="City:").pack()
        ttk.Entry(self, textvariable=self.city_var).pack()
        ttk.Button(self, text="Get Weather", command=self.show_weather).pack()
        self.result = ttk.Label(self, text="")
        self.result.pack()
    def show_weather(self):
        desc, temp = get_weather(self.city_var.get())
        if desc:
            self.result.config(text=f"{desc}, {temp}°C")
        else:
            self.result.config(text="City not found or API error.")

# Usage:
# app = WeatherApp()
# app.mainloop()
```

## 4. Project 3: Web Scraper
- **Skills:** requests, Beautiful Soup, data parsing
- **Features:** Scrape data from a website; save to CSV or database.
- **Tech:** requests, beautifulsoup4, pandas.
- **Sample Code:**
```python
import requests
from bs4 import BeautifulSoup
import csv

def scrape_titles(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    titles = [item.text.strip() for item in soup.find_all('h2')]
    return titles

def save_to_csv(data, filename):
    with open(filename, 'w', newline='') as f:
        writer = csv.writer(f)
        writer.writerow(["title"])
        for row in data:
            writer.writerow([row])

if __name__ == "__main__":
    url = 'https://example.com'
    titles = scrape_titles(url)
    save_to_csv(titles, 'titles.csv')
```
- **Extension:** Add scheduling or notifications.

#### Step-by-Step Implementation
1. Identify target website and data to extract.
2. Write scraping logic with requests and BeautifulSoup.
3. Handle pagination and dynamic content (Selenium/Playwright if needed).
4. Clean and deduplicate data.
5. Save results to CSV or database.
6. Add configuration for scraping targets.
7. Write tests for parsing and data cleaning functions.

#### Key Design Decisions
- Respect robots.txt and site terms of service.
- Use user-agent headers and rate limiting.

#### Testing Strategies
- Use sample HTML files for parser tests.
- Monitor for site structure changes.

#### Real-World Tips
- Schedule scrapes during off-peak hours.
- Log errors and notify on failures.

## 5. Project 4: Blog Website
- **Skills:** Flask/Django, database integration
- **Features:** User authentication, CRUD for posts, comments.
- **Tech:** Flask/Django, SQLite/PostgreSQL.
- **Sample Structure:**
```python
from flask import Flask, render_template, request, redirect, url_for
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///blog.db'
db = SQLAlchemy(app)

class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)

@app.route('/')
def home():
    posts = Post.query.all()
    return render_template('index.html', posts=posts)

@app.route('/post/<int:post_id>')
def post_detail(post_id):
    post = Post.query.get_or_404(post_id)
    return render_template('post_detail.html', post=post)

@app.route('/create', methods=['GET', 'POST'])
def create_post():
    if request.method == 'POST':
        title = request.form['title']
        content = request.form['content']
        new_post = Post(title=title, content=content)
        db.session.add(new_post)
        db.session.commit()
        return redirect(url_for('home'))
    return render_template('create_post.html')

if __name__ == '__main__':
    app.run(debug=True)
```
- **Extension:** Add REST API or deploy to the cloud.

#### Step-by-Step Implementation
1. Set up Flask/Django project and database models.
2. Implement user authentication and authorization.
3. Build CRUD views for posts and comments.
4. Add features: search, tags, pagination, image uploads.
5. Write unit and integration tests for models, views, and forms.
6. Deploy to a cloud platform.

#### Key Design Decisions
- Choose Flask for flexibility, Django for built-in features.
- Use ORM for database abstraction.

#### Testing Strategies
- Use test databases and fixtures.
- Test permissions and edge cases (e.g., unauthorized access).

#### Real-World Tips
- Regularly back up the database.
- Use HTTPS and secure user data.

### Note: GUI for Blog Website
For the Blog Website, the GUI is typically the web interface itself (HTML templates rendered by Flask/Django). For desktop GUI, consider using Tkinter for a simple dashboard, but most real-world blog platforms use web UIs or the Django admin interface for management.

## 6. Project 5: Task Management API
- **Skills:** RESTful API, authentication
- **Features:** Create, update, delete, and list tasks; user authentication.
- **Tech:** Flask/Django REST Framework, JWT.
- **Sample Structure:**
```python
from flask import Flask, jsonify, request
from flask_jwt_extended import JWTManager, create_access_token, jwt_required

app = Flask(__name__)
app.config['JWT_SECRET_KEY'] = 'super-secret'  # Change this in production!
jwt = JWTManager(app)

tasks = []

@app.route('/login', methods=['POST'])
def login():
    username = request.json.get('username')
    password = request.json.get('password')
    # Replace with real user validation
    if username == 'admin' and password == 'password':
        access_token = create_access_token(identity=username)
        return jsonify(access_token=access_token)
    return jsonify({'msg': 'Bad credentials'}), 401

@app.route('/tasks', methods=['GET', 'POST'])
@jwt_required()
def manage_tasks():
    if request.method == 'GET':
        return jsonify(tasks)
    elif request.method == 'POST':
        task = request.json
        tasks.append(task)
        return jsonify(task), 201

if __name__ == '__main__':
    app.run(debug=True)
```
- **Extension:** Add frontend with React or Vue.js.

#### Step-by-Step Implementation
1. Set up Flask/Django REST project and database models.
2. Implement JWT authentication and RBAC.
3. Build CRUD endpoints for tasks.
4. Add features: deadlines, priorities, notifications, API docs.
5. Write automated API and security tests.
6. Deploy and document the API.

#### Key Design Decisions
- Use JWT for stateless authentication.
- Version API for future changes.

#### Testing Strategies
- Use Postman or pytest for endpoint testing.
- Test for unauthorized access and invalid data.

#### Real-World Tips
- Document API endpoints with Swagger/OpenAPI.
- Monitor API usage and errors in production.
