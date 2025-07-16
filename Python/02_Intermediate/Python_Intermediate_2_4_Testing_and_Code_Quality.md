# Python Intermediate 2.4: Testing and Code Quality

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Testing in Python](#2-testing-in-python)
  - [2.1 Unit Testing with unittest](#21-unit-testing-with-unittest)
  - [2.2 pytest Framework](#22-pytest-framework)
  - [2.3 Test-Driven Development (TDD) Basics](#23-test-driven-development-tdd-basics)
  - [2.4 Mocking and Fixtures](#24-mocking-and-fixtures)
- [3. Code Quality](#3-code-quality)
  - [3.1 PEP 8 Style Guide](#31-pep-8-style-guide)
  - [3.2 Code Linting (pylint, flake8)](#32-code-linting-pylint-flake8)
  - [3.3 Type Hints and mypy](#33-type-hints-and-mypy)
  - [3.4 Documentation (docstrings, Sphinx)](#34-documentation-docstrings-sphinx)
- [4. Practical Examples](#4-practical-examples)
- [5. Review Questions](#5-review-questions)
- [6. Answers to Review Questions](#6-answers-to-review-questions)

---

## 1. Introduction

Testing and code quality are essential for building reliable, maintainable, and professional Python applications. This module covers best practices and tools for both.

## 2. Testing in Python

### 2.1 Unit Testing with unittest
The built-in `unittest` module is used for writing and running tests.
```python
import unittest

def add(a, b):
    return a + b

class TestAdd(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(2, 3), 5)

if __name__ == '__main__':
    unittest.main()
```

#### Advanced unittest Features
- `setUp` and `tearDown` methods for test preparation and cleanup.
- Test discovery and test suites for organizing large test codebases.
```python
class TestMath(unittest.TestCase):
    def setUp(self):
        self.data = [1, 2, 3]
    def tearDown(self):
        self.data = None
    def test_sum(self):
        self.assertEqual(sum(self.data), 6)
```

### 2.2 pytest Framework
`pytest` is a popular third-party testing framework with powerful features.
```python
def add(a, b):
    return a + b

def test_add():
    assert add(2, 3) == 5
```
Run tests with `pytest` in the terminal:
```
pytest test_file.py
```

#### pytest Advanced Features
- Fixtures for reusable test setup/teardown.
- Parametrized tests for running the same test with different data.
- Rich plugin ecosystem (e.g., coverage, xdist for parallel testing).
```python
import pytest
@pytest.fixture
def sample_data():
    return [1, 2, 3]
def test_sum(sample_data):
    assert sum(sample_data) == 6
@pytest.mark.parametrize("a,b,result", [(2,3,5), (1,1,2)])
def test_add_param(a, b, result):
    assert add(a, b) == result
```

### 2.3 Test-Driven Development (TDD) Basics
TDD is a methodology where tests are written before code. Steps:
1. Write a failing test
2. Write code to pass the test
3. Refactor and repeat

#### Benefits and Challenges
- Ensures code meets requirements and is robust.
- Encourages modular, testable code.
- Can increase initial development time but reduces bugs and maintenance costs.

### 2.4 Mocking and Fixtures
```python
from unittest.mock import Mock
mock = Mock(return_value=10)
assert mock() == 10
```

- **patch**: Temporarily replace objects for testing.
```python
from unittest.mock import patch
import requests
def get_data():
    return requests.get('https://example.com').status_code
def test_get_data():
    with patch('requests.get') as mock_get:
        mock_get.return_value.status_code = 200
        assert get_data() == 200
```

## 3. Code Quality

### 3.1 PEP 8 Style Guide
PEP 8 is the official Python style guide. Key points:

- Use blank lines to separate functions/classes.
- Use docstrings for all public modules, functions, classes, and methods.

### 3.2 Code Linting (pylint, flake8)
Linters check code for errors and style issues.
```
pylint myfile.py
flake8 myfile.py
```

- Linters can be integrated into editors and CI pipelines for automated checks.

### 3.3 Type Hints and mypy
Type hints improve code clarity and catch type errors.
```python
def add(a: int, b: int) -> int:
    return a + b
```
Check types with `mypy`:
```
mypy myfile.py
```

- Type hints support complex types: `List[int]`, `Dict[str, Any]`, `Optional[str]`, etc.
```python
from typing import List, Optional
def mean(values: List[float]) -> Optional[float]:
    if not values:
        return None
    return sum(values) / len(values)
```

### 3.4 Documentation (docstrings, Sphinx)
```python
def add(a, b):
    """Add two numbers and return the result."""
    return a + b
```

- Use Google or NumPy style docstrings for consistency.
```python
def multiply(a: int, b: int) -> int:
    """
    Multiply two integers.

    Args:
        a (int): First integer.
        b (int): Second integer.

    Returns:
        int: The product of a and b.
    """
    return a * b
```

## 4. Practical Examples

### Example: Testing a Calculator Class
```python
class Calculator:
    def add(self, a, b):
        return a + b

def test_add():
    calc = Calculator()
    assert calc.add(2, 2) == 4
```

### Example: Parametrized and Mocked Tests with pytest
```python
import pytest
from unittest.mock import patch
def fetch_data():
    # Simulate API call
    return 42
@pytest.mark.parametrize("x,expected", [(1,2), (2,3)])
def test_increment(x, expected):
    assert x+1 == expected
def test_fetch_data():
    with patch('__main__.fetch_data', return_value=100):
        assert fetch_data() == 100
```

### Example: Using Fixtures for Database Setup
```python
import pytest
@pytest.fixture
def db():
    # Setup code
    db = {'users': []}
    yield db
    # Teardown code
def test_db_add_user(db):
    db['users'].append('alice')
    assert 'alice' in db['users']
```

### Example: Linting and Type Checking
```bash
flake8 calculator.py
mypy calculator.py
```

### Example: Measuring Test Coverage
```bash
pytest --cov=calculator.py
# or with coverage.py
coverage run -m pytest
coverage report -m
```
Test coverage tools show which lines of code are exercised by tests, helping to identify untested code.

### Example: Property-Based Testing with Hypothesis
```python
from hypothesis import given
import hypothesis.strategies as st
@given(st.integers(), st.integers())
def test_add_commutative(a, b):
    assert add(a, b) == add(b, a)
```

## 7. Advanced Topics

### 7.1 Test Coverage and Quality Gates
- **Test coverage**: Use `coverage.py` or `pytest-cov` to ensure all code paths are tested.
- **Quality gates**: Set minimum coverage thresholds in CI pipelines to prevent merging untested code.

### 7.2 Continuous Integration (CI)
- Automate testing and linting with CI tools (GitHub Actions, GitLab CI, Jenkins).
- Example GitHub Actions workflow:
```yaml
name: Python CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Lint
        run: flake8 .
      - name: Test
        run: pytest --cov=.
```

### 7.3 Static Analysis
- Tools like `pylint`, `flake8`, and `bandit` (for security) analyze code without running it.
- Static analysis can catch bugs, security issues, and style violations early.

### 7.4 Code Review Best Practices
- Use pull requests and code reviews to catch issues and share knowledge.
- Review for correctness, readability, maintainability, and security.
- Encourage constructive feedback and knowledge sharing.

### 7.5 Property-Based Testing
- Use libraries like `hypothesis` to generate test cases automatically and find edge cases.

### 7.6 Documentation Testing
- Tools like `doctest` can run code examples in docstrings as tests.
```python
def add(a, b):
    """
    Add two numbers.

    >>> add(2, 3)
    5
    """
    return a + b
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

## 8. Real-World Testing and Quality Strategies

### 8.1 Test Pyramid and Test Types
- **Unit tests**: Fast, isolated, test individual functions/classes.
- **Integration tests**: Test interactions between components (e.g., database, APIs).
- **End-to-end (E2E) tests**: Simulate user workflows (e.g., Selenium, Playwright).
- **Test pyramid**: Emphasize more unit tests, fewer E2E tests for speed and reliability.

### 8.2 CI/CD Best Practices
- Run all tests and linters on every commit/pull request.
- Use code coverage and quality gates to block low-quality code.
- Automate deployment only after passing all checks.
- Use environment variables and secrets management for secure CI/CD.

### 8.3 Security Testing
- Use `bandit` to scan for common Python security issues:
```bash
bandit -r my_project/
```
- Test for input validation, authentication, and authorization.
- Review dependencies for vulnerabilities (e.g., `pip-audit`).

### 8.4 Mutation Testing
- Mutation testing tools (e.g., `mutmut`, `cosmic-ray`) introduce small code changes to check if tests catch them.
- Reveals weaknesses in test suites.

### 8.5 Troubleshooting and FAQ
- **Q: My tests pass locally but fail in CI. Why?**
  - A: Check for environment differences, missing dependencies, or hardcoded paths.
- **Q: How do I test code that interacts with external APIs?**
  - A: Use mocking to simulate API responses.
- **Q: How can I speed up slow test suites?**
  - A: Use parallel test runners (e.g., `pytest-xdist`), optimize fixtures, and minimize E2E tests.

## 5. Review Questions
1. What is the purpose of unit testing?
2. How does pytest differ from unittest?
3. What is TDD and why is it useful?
4. How do linters improve code quality?
5. What are type hints and how are they checked?
6. What is the role of fixtures in testing?
7. How can you mock external dependencies in your tests?
8. Why is documentation important for code quality?
9. What is test coverage and why is it important?
10. How does continuous integration (CI) improve software quality?
11. What is property-based testing and what are its benefits?
12. How can static analysis tools improve security?
13. What is the purpose of code review?
14. How can you test code examples in documentation?
15. What is the test pyramid and why is it important?
16. What is mutation testing and what does it reveal?
17. How can you test for security vulnerabilities in Python code?
18. What are common reasons for tests passing locally but failing in CI?

## 6. Answers to Review Questions
1. **To verify that individual units of code work as expected.**
2. **pytest is simpler, more flexible, and has better output formatting.**
3. **TDD is writing tests before code, ensuring code meets requirements and is robust.**
4. **Linters detect errors, enforce style, and improve readability.**
5. **Type hints specify variable types; tools like mypy check them for correctness.**
6. **Fixtures provide reusable setup and teardown logic for tests, improving maintainability.**
7. **By using mocking libraries (e.g., unittest.mock.patch) to replace real objects with test doubles.**
8. **Documentation helps others understand, use, and maintain your code, and is essential for collaboration and onboarding.**
9. **Test coverage measures the proportion of code exercised by tests; high coverage reduces the risk of undetected bugs.**
10. **CI automates testing and linting, catching issues early and ensuring code quality before deployment.**
11. **Property-based testing generates many test cases automatically, revealing edge cases and increasing confidence.**
12. **Static analysis tools can detect security vulnerabilities and unsafe code patterns before code is run.**
13. **Code review ensures correctness, readability, and maintainability, and facilitates knowledge sharing.**
14. **With doctest, code examples in docstrings are executed as tests to ensure documentation accuracy.**
15. **The test pyramid prioritizes many unit tests, fewer integration, and even fewer E2E tests for efficiency and reliability.**
16. **Mutation testing introduces code changes to check if tests catch them, revealing weaknesses in test coverage.**
17. **Use tools like bandit, pip-audit, and test for input validation, authentication, and dependency vulnerabilities.**
18. **Environment differences, missing dependencies, or hardcoded paths can cause CI failures despite local success.**
