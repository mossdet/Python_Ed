# Python Beginner 5.1: Projects to Build

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Calculator](#2-calculator)
- [3. To-Do List](#3-to-do-list)
- [4. Number Guessing Game](#4-number-guessing-game)
- [5. Simple Password Generator](#5-simple-password-generator)
- [6. Basic File Organizer](#6-basic-file-organizer)
- [7. Review Questions](#7-review-questions)
- [8. Answers to Review Questions](#8-answers-to-review-questions)

---

## 1. Introduction

Building small projects is the best way to consolidate your Python skills. Each project below is designed to reinforce concepts from previous modules, including syntax, control flow, data structures, error handling, and modules.

## 2. Calculator
A calculator project is a classic way to reinforce Python fundamentals. It should support addition, subtraction, multiplication, and division, with robust error handling and modular design.

### Requirements
- Accept user input for numbers and operations
- Validate input and handle errors (e.g., division by zero)
- Modularize each operation as a function
- Optionally, support continuous calculations and history

### Example Implementation
```python
# calculator.py
def add(a, b): return a + b
def subtract(a, b): return a - b
def multiply(a, b): return a * b
def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero.")
    return a / b

def main():
    print("Calculator")
    while True:
        try:
            a = float(input("First number: "))
            b = float(input("Second number: "))
            op = input("Operation (+, -, *, /): ")
            if op == '+': print(add(a, b))
            elif op == '-': print(subtract(a, b))
            elif op == '*': print(multiply(a, b))
            elif op == '/': print(divide(a, b))
            else: print("Invalid operation.")
        except Exception as e:
            print(f"Error: {e}")
        if input("Continue? (y/n): ").lower() != 'y': break
if __name__ == "__main__": main()
```

---

## 3. To-Do List
A file-based to-do list project teaches persistence, file I/O, and list management.

### Requirements
- Add, view, and remove tasks
- Save tasks to a file and load them on startup
- Handle file errors gracefully

### Example Implementation
```python
# todo.py
def load_tasks(filename):
    try:
        with open(filename, 'r') as f:
            return [line.strip() for line in f]
    except FileNotFoundError:
        return []
def save_tasks(tasks, filename):
    with open(filename, 'w') as f:
        for task in tasks:
            f.write(task + '\n')
def main():
    filename = 'tasks.txt'
    tasks = load_tasks(filename)
    while True:
        print("\nTo-Do List:")
        for i, task in enumerate(tasks, 1):
            print(f"{i}. {task}")
        action = input("Add (a), Remove (r), Quit (q): ").lower()
        if action == 'a':
            tasks.append(input("New task: "))
        elif action == 'r':
            idx = int(input("Task number to remove: ")) - 1
            if 0 <= idx < len(tasks): tasks.pop(idx)
            else: print("Invalid task number.")
        elif action == 'q': break
        else: print("Invalid action.")
        save_tasks(tasks, filename)
if __name__ == "__main__": main()
```

---

## 4. Number Guessing Game
This project covers loops, conditionals, random number generation, and user interaction.

### Requirements
- Generate a random number
- Prompt user for guesses and provide feedback
- Track number of attempts

### Example Implementation
```python
# guessing_game.py
import random
def main():
    number = random.randint(1, 100)
    attempts = 0
    print("Guess the number between 1 and 100!")
    while True:
        guess = int(input("Your guess: "))
        attempts += 1
        if guess < number: print("Too low!")
        elif guess > number: print("Too high!")
        else:
            print(f"Correct! Attempts: {attempts}")
            break
if __name__ == "__main__": main()
```

---

## 5. Simple Password Generator
This project demonstrates string manipulation, randomization, and function design.

### Requirements
- Generate passwords of specified length
- Use letters, digits, and symbols
- Optionally avoid ambiguous characters

### Example Implementation
```python
# password_generator.py
import random, string
def generate_password(length=12):
    chars = string.ascii_letters + string.digits + string.punctuation
    return ''.join(random.choice(chars) for _ in range(length))
def main():
    length = int(input("Password length: "))
    print(f"Generated password: {generate_password(length)}")
if __name__ == "__main__": main()
```

---

## 6. Basic File Organizer
This project covers file operations, directory management, and dictionary usage.

### Requirements
- Scan a directory for files
- Move files into folders by type (extension)
- Handle file errors and naming conflicts

### Example Implementation
```python
# file_organizer.py
import os, shutil
def organize_files(directory):
    for filename in os.listdir(directory):
        if os.path.isfile(os.path.join(directory, filename)):
            ext = filename.split('.')[-1]
            folder = os.path.join(directory, ext)
            os.makedirs(folder, exist_ok=True)
            shutil.move(os.path.join(directory, filename), os.path.join(folder, filename))
def main():
    directory = input("Directory to organize: ")
    organize_files(directory)
    print("Files organized by type.")
if __name__ == "__main__": main()
```

---

## 7. Review Questions (Advanced)
1. How would you refactor the calculator to support more operations and better error handling?
2. What are the advantages of storing tasks in a file versus in memory?
3. How can you make the number guessing game more challenging or interactive?
4. What security considerations should you keep in mind for a password generator?
5. How would you extend the file organizer to handle nested directories or duplicate files?

## 8. Answers to Review Questions
1. **Use a dictionary to map operations to functions, add input validation, and handle edge cases (e.g., non-numeric input, division by zero).**
2. **File storage allows persistence across sessions, sharing, and backup; memory storage is lost when the program exits.**
3. **Add difficulty levels, hints, a timer, or multiplayer support.**
4. **Avoid predictable patterns, use cryptographically secure random generators, and allow user customization of character sets.**
5. **Recursively scan directories, check for existing files before moving, and handle naming conflicts by renaming or skipping duplicates.**
