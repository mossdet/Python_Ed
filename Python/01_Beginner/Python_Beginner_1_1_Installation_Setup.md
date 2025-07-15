# Python Beginner 1.1: Installation & Setup

## Table of Contents
1. [Learning Objectives](#learning-objectives)
2. [Python Installation](#1-python-installation)
3. [Integrated Development Environment (IDE) Setup](#2-integrated-development-environment-ide-setup)
4. [Virtual Environments](#3-virtual-environments)
5. [Development Environment Configuration](#4-development-environment-configuration)
6. [Testing Your Setup](#5-testing-your-setup)
7. [Troubleshooting Common Issues](#6-troubleshooting-common-issues)
8. [Best Practices](#7-best-practices)
9. [Performance Considerations](#8-performance-considerations)
10. [Review Questions](#review-questions)
11. [Practical Exercises](#practical-exercises)
12. [Next Steps](#next-steps)
13. [Additional Resources](#additional-resources)

## Learning Objectives
By the end of this module, you will be able to:
- Install Python on different operating systems
- Set up and configure an Integrated Development Environment (IDE)
- Understand and use the Python interpreter effectively
- Create, manage, and activate virtual environments
- Configure your development environment for optimal Python programming

## 1. Python Installation

### 1.1 Understanding Python Versions

Python has two major version branches:
- **Python 2.x** (Legacy, deprecated as of January 1, 2020)
- **Python 3.x** (Current, actively maintained)

For this course, we'll use Python 3.11+ as it offers:
- Enhanced performance optimizations
- Improved error messages
- Better type hinting support
- Security improvements
- Advanced debugging capabilities

### 1.2 Installation Methods

#### 1.2.1 Official Python Distribution

**Windows:**
1. Visit https://python.org/downloads/
2. Download the latest Python 3.11+ installer
3. Run the installer with "Add Python to PATH" checked
4. Verify installation: `python --version`

**macOS:**
1. Option 1: Download from python.org (recommended)
2. Option 2: Use Homebrew: `brew install python@3.11`
3. Verify: `python3 --version`

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install python3.11 python3.11-pip python3.11-venv
```

**Linux (CentOS/RHEL):**
```bash
sudo yum install python3.11 python3.11-pip
```

#### 1.2.2 Anaconda Distribution

Anaconda provides a comprehensive Python distribution with:
- Pre-installed scientific libraries
- Conda package manager
- Jupyter notebooks
- Spyder IDE

**Installation:**
1. Download Anaconda from https://anaconda.com/download
2. Follow platform-specific installation instructions
3. Verify: `conda --version`

### 1.3 Python Interpreter Basics

#### 1.3.1 Interactive Mode (REPL)

The Python interpreter provides a Read-Eval-Print Loop (REPL):

```python
# Start interpreter
$ python

# Interactive session
>>> print("Hello, World!")
Hello, World!
>>> 2 + 3
5
>>> import sys
>>> sys.version
'3.11.0 (main, Oct 24 2022, 18:26:48) [MSC v.1933 64 bit (AMD64)]'
```

#### 1.3.2 Script Execution

Create a file `hello.py`:
```python
#!/usr/bin/env python3
"""
My first Python script
"""

def main():
    print("Hello, World!")
    print("Python is awesome!")

if __name__ == "__main__":
    main()
```

Execute: `python hello.py`

#### 1.3.3 Command Line Arguments

```python
import sys

def main():
    print(f"Script name: {sys.argv[0]}")
    print(f"Arguments: {sys.argv[1:]}")
    
    if len(sys.argv) > 1:
        print(f"First argument: {sys.argv[1]}")

if __name__ == "__main__":
    main()
```

Execute: `python script.py arg1 arg2`

## 2. Integrated Development Environment (IDE) Setup

### 2.1 Visual Studio Code

**Installation:**
1. Download from https://code.visualstudio.com/
2. Install platform-specific package

**Python Extension Setup:**
1. Install Python extension (ms-python.python)
2. Install Pylance for advanced language support
3. Configure interpreter: `Ctrl+Shift+P` → "Python: Select Interpreter"

**Essential Extensions:**
- Python (ms-python.python)
- Pylance (ms-python.vscode-pylance)
- Python Docstring Generator
- autoDocstring
- Python Test Explorer

**VS Code Configuration:**
```json
{
    "python.defaultInterpreterPath": "/usr/bin/python3",
    "python.linting.enabled": true,
    "python.linting.pylintEnabled": true,
    "python.formatting.provider": "black",
    "python.testing.pytestEnabled": true,
    "editor.formatOnSave": true,
    "python.terminal.activateEnvironment": true
}
```

### 2.2 PyCharm

**Community Edition (Free):**
- Download from https://jetbrains.com/pycharm/
- Full-featured IDE with intelligent code completion
- Built-in debugger and testing tools
- Git integration

**Professional Edition Features:**
- Web framework support (Django, Flask)
- Database tools
- Scientific tools integration
- Remote development capabilities

### 2.3 Alternative IDEs

**IDLE (Built-in):**
- Ships with Python
- Basic but functional
- Good for beginners

**Thonny:**
- Beginner-friendly
- Excellent debugging visualization
- Simple interface

**Sublime Text with Python plugins:**
- Lightweight
- Highly customizable
- Excellent performance

## 3. Virtual Environments

### 3.1 Why Virtual Environments?

Virtual environments solve dependency conflicts by:
- Isolating project dependencies
- Managing different Python versions per project
- Preventing system-wide package pollution
- Enabling reproducible deployments

### 3.2 venv Module (Recommended)

**Creating Virtual Environment:**
```bash
# Create virtual environment
python -m venv myproject_env

# Activate (Windows)
myproject_env\Scripts\activate

# Activate (macOS/Linux)
source myproject_env/bin/activate

# Deactivate
deactivate
```

**Managing Dependencies:**
```bash
# Install packages
pip install requests numpy pandas

# Generate requirements
pip freeze > requirements.txt

# Install from requirements
pip install -r requirements.txt
```

### 3.3 Conda Environments

```bash
# Create environment
conda create -n myproject python=3.11

# Activate
conda activate myproject

# Install packages
conda install numpy pandas matplotlib

# Export environment
conda env export > environment.yml

# Create from file
conda env create -f environment.yml
```

### 3.4 Advanced Virtual Environment Management

**Using virtualenvwrapper:**
```bash
# Install
pip install virtualenvwrapper

# Create environment
mkvirtualenv myproject

# Work on project
workon myproject

# Remove environment
rmvirtualenv myproject
```

**Using pipenv:**
```bash
# Install
pip install pipenv

# Create Pipfile and virtual environment
pipenv install requests

# Activate shell
pipenv shell

# Install dev dependencies
pipenv install pytest --dev
```

## 4. Development Environment Configuration

### 4.1 Python Path Configuration

Understanding how Python finds modules:

```python
import sys
print(sys.path)
```

**Adding to Python Path:**
```python
import sys
sys.path.append('/path/to/your/modules')
```

**Environment Variables:**
```bash
export PYTHONPATH="${PYTHONPATH}:/path/to/your/modules"
```

### 4.2 Git Integration

**Basic Git Setup:**
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

**Python .gitignore:**
```
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/
```

### 4.3 Code Quality Tools

**Installing Essential Tools:**
```bash
pip install pylint black flake8 mypy pytest
```

**Configuration Files:**

**pyproject.toml:**
```toml
[tool.black]
line-length = 88
target-version = ['py311']

[tool.pylint]
max-line-length = 88
disable = "C0111,C0103,C0330"

[tool.mypy]
python_version = "3.11"
warn_return_any = true
warn_unused_configs = true
```

## 5. Testing Your Setup

### 5.1 Verification Script

Create `setup_test.py`:
```python
#!/usr/bin/env python3
"""
Python setup verification script
"""

import sys
import platform
import subprocess
import importlib.util

def check_python_version():
    """Check Python version"""
    version = sys.version_info
    print(f"Python version: {version.major}.{version.minor}.{version.micro}")
    
    if version.major == 3 and version.minor >= 11:
        print("✓ Python version is suitable")
        return True
    else:
        print("✗ Python version should be 3.11 or higher")
        return False

def check_pip():
    """Check pip installation"""
    try:
        import pip
        print(f"✓ pip is installed: {pip.__version__}")
        return True
    except ImportError:
        print("✗ pip is not installed")
        return False

def check_venv():
    """Check venv module"""
    try:
        import venv
        print("✓ venv module is available")
        return True
    except ImportError:
        print("✗ venv module is not available")
        return False

def check_essential_modules():
    """Check essential Python modules"""
    essential_modules = [
        'os', 'sys', 'json', 'csv', 'datetime', 
        'collections', 'itertools', 'functools'
    ]
    
    all_available = True
    for module in essential_modules:
        if importlib.util.find_spec(module):
            print(f"✓ {module} is available")
        else:
            print(f"✗ {module} is not available")
            all_available = False
    
    return all_available

def main():
    """Main verification function"""
    print("Python Development Environment Check")
    print("=" * 40)
    
    print(f"Operating System: {platform.system()} {platform.release()}")
    print(f"Architecture: {platform.architecture()[0]}")
    print(f"Python executable: {sys.executable}")
    print()
    
    checks = [
        check_python_version(),
        check_pip(),
        check_venv(),
        check_essential_modules()
    ]
    
    print("\nSummary:")
    print("=" * 40)
    
    if all(checks):
        print("✓ All checks passed! Your Python environment is ready.")
    else:
        print("✗ Some checks failed. Please review the issues above.")

if __name__ == "__main__":
    main()
```

## 6. Troubleshooting Common Issues

### 6.1 PATH Issues

**Problem:** Python not found in command line
**Solution:**
```bash
# Windows
set PATH=%PATH%;C:\Python311;C:\Python311\Scripts

# macOS/Linux
export PATH=$PATH:/usr/local/bin/python3
```

### 6.2 Permission Issues

**Problem:** Permission denied when installing packages
**Solution:**
```bash
# Use virtual environment (recommended)
python -m venv myenv
source myenv/bin/activate

# Or use --user flag
pip install --user package_name
```

### 6.3 SSL Certificate Issues

**Problem:** SSL certificate verification failed
**Solution:**
```bash
# Upgrade certificates
pip install --upgrade certifi

# Or use trusted host (temporary)
pip install --trusted-host pypi.org --trusted-host pypi.python.org package_name
```

### 6.4 Virtual Environment Activation Issues

**Problem:** Virtual environment not activating
**Solution:**
```bash
# Ensure correct path
ls venv/bin/activate  # Linux/macOS
dir venv\Scripts\activate.bat  # Windows

# Use full path
source /full/path/to/venv/bin/activate
```

## 7. Best Practices

### 7.1 Project Structure

```
myproject/
├── README.md
├── requirements.txt
├── setup.py
├── .gitignore
├── .env
├── src/
│   └── myproject/
│       ├── __init__.py
│       └── main.py
├── tests/
│   ├── __init__.py
│   └── test_main.py
├── docs/
└── venv/
```

### 7.2 Development Workflow

1. **Project Initialization:**
   ```bash
   mkdir myproject
   cd myproject
   python -m venv venv
   source venv/bin/activate
   ```

2. **Dependency Management:**
   ```bash
   pip install package_name
   pip freeze > requirements.txt
   ```

3. **Code Quality:**
   ```bash
   black src/
   pylint src/
   mypy src/
   ```

4. **Testing:**
   ```bash
   pytest tests/
   ```

### 7.3 Configuration Management

**Using environment variables:**
```python
import os
from dotenv import load_dotenv

load_dotenv()

DATABASE_URL = os.getenv('DATABASE_URL')
SECRET_KEY = os.getenv('SECRET_KEY', 'default-secret')
```

**.env file:**
```
DATABASE_URL=sqlite:///app.db
SECRET_KEY=your-secret-key
DEBUG=True
```

## 8. Performance Considerations

### 8.1 Python Optimization Flags

```bash
# Optimize Python bytecode
python -O script.py

# Remove docstrings and assertions
python -OO script.py

# Unbuffered output
python -u script.py
```

### 8.2 Memory Management

```python
import gc
import sys

# Check memory usage
print(f"Memory usage: {sys.getsizeof(object)} bytes")

# Force garbage collection
gc.collect()

# Monitor reference counts
import weakref
```

## Review Questions

### Theoretical Questions

1. **What are the key differences between Python 2 and Python 3, and why is Python 3 preferred?**

**Answer:** Python 3 introduces several breaking changes and improvements:
- **Unicode by default:** Python 3 uses Unicode (UTF-8) strings by default, while Python 2 uses ASCII
- **Print function:** `print()` is a function in Python 3, not a statement
- **Integer division:** `/` always returns float, `//` for integer division
- **Better error handling:** More informative error messages
- **Type annotations:** Built-in support for type hints
- **Performance improvements:** Better memory management and optimization
- **Security enhancements:** Improved security features
- **Active development:** Python 2 reached end-of-life in 2020

2. **Explain the concept of virtual environments and their importance in Python development.**

**Answer:** Virtual environments are isolated Python environments that allow:
- **Dependency isolation:** Different projects can use different package versions
- **System protection:** Prevents conflicts with system Python packages
- **Reproducibility:** Ensures consistent environments across different machines
- **Version management:** Different Python versions per project
- **Clean uninstallation:** Easy removal of project dependencies

3. **What is the Python interpreter, and how does it execute Python code?**

**Answer:** The Python interpreter is a program that reads and executes Python code:
- **Lexical analysis:** Breaks source code into tokens
- **Parsing:** Creates an Abstract Syntax Tree (AST)
- **Compilation:** Converts AST to bytecode
- **Execution:** Python Virtual Machine (PVM) executes bytecode
- **Interactive mode:** REPL for immediate code execution
- **Script mode:** Executes entire files

### Practical Questions

4. **How would you troubleshoot a situation where Python packages are not installing correctly?**

**Answer:** Troubleshooting steps:
1. **Check Python and pip versions:** `python --version`, `pip --version`
2. **Verify internet connection:** Try `pip install --upgrade pip`
3. **Check virtual environment:** Ensure proper activation
4. **Clear pip cache:** `pip cache purge`
5. **Use verbose mode:** `pip install -v package_name`
6. **Check permissions:** Use `--user` flag if needed
7. **Try alternative index:** `pip install -i https://pypi.org/simple/ package_name`
8. **Check firewall/proxy settings:** Configure proxy if needed

5. **Describe the process of setting up a Python project with proper dependency management.**

**Answer:** Complete setup process:
1. **Create project directory:** `mkdir myproject && cd myproject`
2. **Initialize virtual environment:** `python -m venv venv`
3. **Activate environment:** `source venv/bin/activate`
4. **Install dependencies:** `pip install requests flask`
5. **Create requirements file:** `pip freeze > requirements.txt`
6. **Set up project structure:** Create src/, tests/, docs/ directories
7. **Initialize git:** `git init`
8. **Create .gitignore:** Include venv/, __pycache__/, etc.
9. **Documentation:** Create README.md with setup instructions

### Advanced Questions

6. **What are the advantages and disadvantages of different Python IDEs?**

**Answer:** 
**VS Code:**
- Advantages: Lightweight, extensive extensions, excellent debugging, free
- Disadvantages: Requires configuration, can be resource-intensive with many extensions

**PyCharm:**
- Advantages: Full-featured, intelligent code completion, integrated tools
- Disadvantages: Resource-heavy, paid Professional version for web development

**IDLE:**
- Advantages: Built-in, simple, good for beginners
- Disadvantages: Limited features, no advanced debugging

**Jupyter:**
- Advantages: Interactive, great for data science, visualization
- Disadvantages: Not suitable for large applications, version control challenges

7. **Explain how Python's import system works and how PYTHONPATH affects it.**

**Answer:** Python's import system:
- **Module search order:** Built-in → sys.path directories → PYTHONPATH
- **sys.path contents:** Script directory, standard library, site-packages
- **PYTHONPATH:** Environment variable adding directories to sys.path
- **Import process:** Find module → Load → Execute → Cache in sys.modules
- **Package imports:** Uses __init__.py files to define packages
- **Relative imports:** Use relative paths within packages

8. **How would you optimize a Python development environment for a large team?**

**Answer:** Team optimization strategies:
- **Standardized Python version:** Use .python-version files
- **Unified dependency management:** requirements.txt + requirements-dev.txt
- **Code quality tools:** Pre-commit hooks with black, pylint, mypy
- **Containerization:** Docker for consistent environments
- **CI/CD pipeline:** Automated testing and deployment
- **Documentation standards:** Consistent docstring formats
- **Configuration management:** Environment variables and .env files
- **Monitoring:** Logging and error tracking setup

## Practical Exercises

### Exercise 1: Environment Setup Automation
Create a script that automatically sets up a Python development environment with virtual environment, essential packages, and configuration files.

### Exercise 2: Multi-Version Python Management
Set up and manage multiple Python versions on your system and demonstrate switching between them for different projects.

### Exercise 3: IDE Configuration
Configure your chosen IDE with custom settings, extensions, and shortcuts for optimal Python development productivity.

### Exercise 4: Project Template Creation
Create a template project structure with proper dependency management, testing setup, and documentation that can be reused for new projects.

---

## Next Steps

In the next module, we'll dive into Python's basic syntax and data types, building upon the solid foundation we've established in this setup module. You'll learn about variables, numbers, strings, and the fundamental building blocks of Python programming.

## Additional Resources

- [Python.org Official Documentation](https://docs.python.org/3/)
- [Real Python - Python Virtual Environments](https://realpython.com/python-virtual-environments-a-primer/)
- [VS Code Python Documentation](https://code.visualstudio.com/docs/python/python-tutorial)
- [PyCharm Documentation](https://www.jetbrains.com/help/pycharm/)
- [PEP 8 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)

---

*This module provides the foundation for all subsequent Python learning. Take time to properly set up your environment as it will significantly impact your learning efficiency and coding productivity.*
