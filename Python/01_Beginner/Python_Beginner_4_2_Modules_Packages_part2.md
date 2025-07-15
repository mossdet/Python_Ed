# Python Beginner 4.2: Modules and Packages Part 2

## Table of Contents
- [1. Introduction to Modules](Python_Beginner_4_2_Modules_Packages_part1.md#1-introduction-to-modules)
- [2. Understanding Python's Module System](Python_Beginner_4_2_Modules_Packages_part1.md#2-understanding-pythons-module-system)
- [3. Importing Modules](Python_Beginner_4_2_Modules_Packages_part1.md#3-importing-modules)
- [4. Creating Custom Modules](Python_Beginner_4_2_Modules_Packages_part1.md#4-creating-custom-modules)
- [5. Module Search Path and PYTHONPATH](Python_Beginner_4_2_Modules_Packages_part1.md#5-module-search-path-and-pythonpath)
- [6. Package Structure and Organization](Python_Beginner_4_2_Modules_Packages_part1.md#6-package-structure-and-organization)
- [7. The __init__.py File](Python_Beginner_4_2_Modules_Packages_part2.md#7-the-__init__py-file)
- [8. Relative and Absolute Imports](Python_Beginner_4_2_Modules_Packages_part2.md#8-relative-and-absolute-imports)
- [9. Built-in Modules](Python_Beginner_4_2_Modules_Packages_part2.md#9-built-in-modules)
- [10. Third-Party Package Management with pip](Python_Beginner_4_2_Modules_Packages_part2.md#10-third-party-package-management-with-pip)
- [11. Virtual Environments](Python_Beginner_4_2_Modules_Packages_part2.md#11-virtual-environments)
- [12. Module Documentation and Introspection](Python_Beginner_4_2_Modules_Packages_part2.md#12-module-documentation-and-introspection)
- [13. Best Practices for Module Design](Python_Beginner_4_2_Modules_Packages_part2.md#13-best-practices-for-module-design)
- [14. Common Pitfalls and Solutions](Python_Beginner_4_2_Modules_Packages_part2.md#14-common-pitfalls-and-solutions)
- [15. Advanced Module Concepts](Python_Beginner_4_2_Modules_Packages_part2.md#15-advanced-module-concepts)
- [16. Practical Examples](Python_Beginner_4_2_Modules_Packages_part2.md#16-practical-examples)
- [17. Review Questions](Python_Beginner_4_2_Modules_Packages_part2.md#17-review-questions)
- [18. Answers to Review Questions](Python_Beginner_4_2_Modules_Packages_part2.md#18-answers-to-review-questions)

---

## 8. Relative and Absolute Imports

Understanding the difference between relative and absolute imports is crucial for organizing Python packages effectively. This section explores both import types, their use cases, advantages, and best practices.

### 8.1 Understanding Import Types

Python supports two types of imports for accessing modules within packages:

1. **Absolute Imports**: Reference modules using their full path from the package root
2. **Relative Imports**: Reference modules relative to the current module's location

### 8.2 Absolute Imports

Absolute imports specify the complete path from the top-level package to the target module.

#### 8.2.1 Basic Absolute Import Syntax

```python
# Absolute import syntax
import package.module
from package.module import function
from package.subpackage.module import ClassName
```

#### 8.2.2 Absolute Import Examples

Let's create a comprehensive package structure to demonstrate absolute imports:

```
data_processing/
├── __init__.py
├── core/
│   ├── __init__.py
│   ├── processors.py
│   └── validators.py
├── utils/
│   ├── __init__.py
│   ├── helpers.py
│   └── formatters.py
├── io/
│   ├── __init__.py
│   ├── readers.py
│   └── writers.py
└── tests/
    ├── __init__.py
    └── test_processors.py
```

```python
# data_processing/core/processors.py
"""
Core data processing functions.
"""

from typing import List, Dict, Any
import json

class DataProcessor:
    """Main data processing class."""
    
    def __init__(self):
        self.processed_count = 0
    
    def process_data(self, data: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
        """Process a list of data dictionaries."""
        processed = []
        for item in data:
            processed_item = self._process_item(item)
            processed.append(processed_item)
        
        self.processed_count += len(processed)
        return processed
    
    def _process_item(self, item: Dict[str, Any]) -> Dict[str, Any]:
        """Process a single data item."""
        # Add timestamp
        item['processed_at'] = '2024-01-01T12:00:00Z'
        # Add processing flag
        item['processed'] = True
        return item

def clean_data(data: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
    """Clean data by removing None values."""
    cleaned = []
    for item in data:
        cleaned_item = {k: v for k, v in item.items() if v is not None}
        cleaned.append(cleaned_item)
    return cleaned
```

```python
# data_processing/utils/helpers.py
"""
Utility helper functions.
"""

from typing import Any, Dict, List
import re

def validate_email(email: str) -> bool:
    """Validate email address format."""
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

def normalize_text(text: str) -> str:
    """Normalize text by removing extra whitespace and converting to lowercase."""
    return ' '.join(text.lower().split())

def flatten_dict(nested_dict: Dict[str, Any], prefix: str = '') -> Dict[str, Any]:
    """Flatten a nested dictionary."""
    flat_dict = {}
    for key, value in nested_dict.items():
        new_key = f"{prefix}.{key}" if prefix else key
        if isinstance(value, dict):
            flat_dict.update(flatten_dict(value, new_key))
        else:
            flat_dict[new_key] = value
    return flat_dict
```

```python
# data_processing/io/readers.py
"""
Data reading functions using absolute imports.
"""

import json
import csv
from typing import List, Dict, Any

# Absolute imports from other modules in the package
from data_processing.core.processors import DataProcessor, clean_data
from data_processing.utils.helpers import validate_email, normalize_text

class DataReader:
    """Data reader with processing capabilities."""
    
    def __init__(self):
        self.processor = DataProcessor()
    
    def read_json(self, filepath: str) -> List[Dict[str, Any]]:
        """Read and process JSON data."""
        with open(filepath, 'r') as f:
            data = json.load(f)
        
        # Clean data using absolute import
        cleaned_data = clean_data(data)
        
        # Process data
        processed_data = self.processor.process_data(cleaned_data)
        
        return processed_data
    
    def read_csv(self, filepath: str) -> List[Dict[str, Any]]:
        """Read and process CSV data."""
        data = []
        with open(filepath, 'r', newline='') as f:
            reader = csv.DictReader(f)
            for row in reader:
                # Normalize text fields using absolute import
                if 'name' in row:
                    row['name'] = normalize_text(row['name'])
                # Validate email if present
                if 'email' in row:
                    row['email_valid'] = validate_email(row['email'])
                data.append(row)
        
        return self.processor.process_data(data)
```

#### 8.2.3 Advantages of Absolute Imports

1. **Clarity**: Explicit path makes it clear where modules come from
2. **Consistency**: Same import statement works from anywhere in the package
3. **IDE Support**: Better autocomplete and refactoring support
4. **Debugging**: Easier to trace dependencies and debug import issues

#### 8.2.4 Absolute Import Best Practices

```python
# data_processing/core/validators.py
"""
Data validation functions demonstrating absolute import best practices.
"""

from typing import List, Dict, Any, Optional
import re
from datetime import datetime

# Group imports by category
# Standard library imports first
import json
import logging

# Third-party imports (if any)
# import requests
# import pandas as pd

# Local package imports - use absolute imports
from data_processing.utils.helpers import validate_email, normalize_text
from data_processing.core.processors import DataProcessor

# Configure logging
logger = logging.getLogger(__name__)

class DataValidator:
    """Comprehensive data validator."""
    
    def __init__(self):
        self.processor = DataProcessor()
        self.validation_rules = {
            'email': validate_email,
            'text': lambda x: len(normalize_text(x)) > 0
        }
    
    def validate_record(self, record: Dict[str, Any]) -> Dict[str, Any]:
        """Validate a single record."""
        validation_results = {}
        
        for field, value in record.items():
            if field in self.validation_rules:
                try:
                    validation_results[f"{field}_valid"] = self.validation_rules[field](value)
                except Exception as e:
                    logger.error(f"Validation error for field {field}: {e}")
                    validation_results[f"{field}_valid"] = False
        
        return validation_results
    
    def validate_batch(self, records: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
        """Validate a batch of records."""
        validated_records = []
        
        for record in records:
            validation_results = self.validate_record(record)
            # Merge validation results with original record
            validated_record = {**record, **validation_results}
            validated_records.append(validated_record)
        
        return validated_records
```

### 8.3 Relative Imports

Relative imports specify module locations relative to the current module's position in the package hierarchy.

#### 8.3.1 Relative Import Syntax

```python
# Relative import syntax
from . import module              # Same directory
from .module import function      # Same directory
from ..module import function     # Parent directory
from ..package.module import function  # Parent's sibling directory
from ...module import function    # Grandparent directory
```

#### 8.3.2 Relative Import Examples

```python
# data_processing/io/writers.py
"""
Data writing functions using relative imports.
"""

import json
import csv
from typing import List, Dict, Any

# Relative imports
from ..core.processors import DataProcessor, clean_data  # Parent directory
from ..utils.helpers import normalize_text               # Parent's sibling
from .readers import DataReader                          # Same directory

class DataWriter:
    """Data writer with processing capabilities."""
    
    def __init__(self):
        self.processor = DataProcessor()
        self.reader = DataReader()
    
    def write_json(self, data: List[Dict[str, Any]], filepath: str) -> None:
        """Write data to JSON file after processing."""
        # Clean data using relative import
        cleaned_data = clean_data(data)
        
        # Process data
        processed_data = self.processor.process_data(cleaned_data)
        
        with open(filepath, 'w') as f:
            json.dump(processed_data, f, indent=2)
    
    def write_csv(self, data: List[Dict[str, Any]], filepath: str) -> None:
        """Write data to CSV file after processing."""
        if not data:
            return
        
        # Normalize text fields using relative import
        normalized_data = []
        for item in data:
            normalized_item = item.copy()
            if 'name' in normalized_item:
                normalized_item['name'] = normalize_text(normalized_item['name'])
            normalized_data.append(normalized_item)
        
        # Process data
        processed_data = self.processor.process_data(normalized_data)
        
        with open(filepath, 'w', newline='') as f:
            if processed_data:
                writer = csv.DictWriter(f, fieldnames=processed_data[0].keys())
                writer.writeheader()
                writer.writerows(processed_data)
```

#### 8.3.3 Complex Relative Import Examples

```python
# data_processing/core/advanced_processors.py
"""
Advanced processing functions demonstrating complex relative imports.
"""

from typing import List, Dict, Any, Optional
import logging

# Relative imports from same directory
from .processors import DataProcessor, clean_data
from .validators import DataValidator

# Relative imports from parent's sibling directories
from ..utils.helpers import validate_email, normalize_text, flatten_dict
from ..utils.formatters import format_date, format_currency
from ..io.readers import DataReader
from ..io.writers import DataWriter

logger = logging.getLogger(__name__)

class AdvancedDataProcessor(DataProcessor):
    """Advanced data processor with validation and formatting."""
    
    def __init__(self):
        super().__init__()
        self.validator = DataValidator()
        self.reader = DataReader()
        self.writer = DataWriter()
    
    def process_with_validation(self, data: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
        """Process data with comprehensive validation."""
        # Clean data first
        cleaned_data = clean_data(data)
        
        # Validate data
        validated_data = self.validator.validate_batch(cleaned_data)
        
        # Process data
        processed_data = self.process_data(validated_data)
        
        # Format specific fields
        formatted_data = []
        for item in processed_data:
            formatted_item = item.copy()
            
            # Format dates if present
            if 'date' in formatted_item:
                try:
                    formatted_item['date'] = format_date(formatted_item['date'])
                except Exception as e:
                    logger.warning(f"Date formatting error: {e}")
            
            # Format currency if present
            if 'amount' in formatted_item:
                try:
                    formatted_item['amount'] = format_currency(formatted_item['amount'])
                except Exception as e:
                    logger.warning(f"Currency formatting error: {e}")
            
            formatted_data.append(formatted_item)
        
        return formatted_data
    
    def process_file(self, input_path: str, output_path: str) -> None:
        """Process data from file and write to output."""
        # Read data
        data = self.reader.read_json(input_path)
        
        # Process with validation
        processed_data = self.process_with_validation(data)
        
        # Write results
        self.writer.write_json(processed_data, output_path)
```

### 8.4 Comparison: Absolute vs Relative Imports

#### 8.4.1 Advantages and Disadvantages

| Aspect | Absolute Imports | Relative Imports |
|--------|------------------|-------------------|
| **Clarity** | Very clear, explicit path | Can be confusing with many dots |
| **Refactoring** | Requires updates when package structure changes | Automatically adjusts to structure changes |
| **IDE Support** | Excellent autocompletion | Good but sometimes limited |
| **Debugging** | Easy to trace dependencies | Can be harder to follow |
| **Maintainability** | Easier for large teams | Good for small, stable packages |
| **Performance** | Slightly faster (no path resolution) | Minimal overhead |

#### 8.4.2 When to Use Each Type

```python
# data_processing/examples/import_strategies.py
"""
Examples of when to use absolute vs relative imports.
"""

# Use absolute imports for:
# 1. Imports from deeply nested modules
from data_processing.core.processors import DataProcessor
from data_processing.utils.helpers import validate_email
from data_processing.io.readers import DataReader

# 2. Imports that are used across multiple modules
from data_processing.core.validators import DataValidator

# 3. Main package interfaces
from data_processing import __version__

# Use relative imports for:
# 1. Imports within the same package level
from .local_utilities import helper_function

# 2. Imports from closely related modules
from ..core.processors import clean_data

# 3. Internal package structure that might change
from ..utils.formatters import format_output

class ExampleProcessor:
    """Example showing mixed import strategies."""
    
    def __init__(self):
        # Absolute imports for main components
        self.processor = DataProcessor()
        self.validator = DataValidator()
        
        # These would be defined in the same module or closely related modules
        self.local_helper = helper_function
        self.formatter = format_output
    
    def process_example(self, data):
        """Example processing method."""
        # Use absolute imports for core functionality
        cleaned = clean_data(data)
        
        # Use relative imports for local utilities
        formatted = self.formatter(cleaned)
        
        return self.processor.process_data(formatted)
```

### 8.5 Common Import Patterns

#### 8.5.1 Package-Level Imports

```python
# data_processing/__init__.py
"""
Package-level imports demonstrating both absolute and relative imports.
"""

# Absolute imports for main package interface
from data_processing.core.processors import DataProcessor
from data_processing.core.validators import DataValidator
from data_processing.io.readers import DataReader
from data_processing.io.writers import DataWriter

# Relative imports for package organization
from .utils.helpers import validate_email, normalize_text
from .utils.formatters import format_date, format_currency

# Package metadata
__version__ = "1.0.0"
__author__ = "Python Student"

# Define public API
__all__ = [
    'DataProcessor',
    'DataValidator', 
    'DataReader',
    'DataWriter',
    'validate_email',
    'normalize_text',
    'format_date',
    'format_currency'
]

# Package-level convenience functions
def create_processor():
    """Create a configured data processor."""
    return DataProcessor()

def create_validator():
    """Create a configured data validator."""
    return DataValidator()

# Add convenience function to public API
__all__.append('create_processor')
__all__.append('create_validator')
```

#### 8.5.2 Conditional Imports

```python
# data_processing/core/optional_features.py
"""
Optional features with conditional imports.
"""

import sys
from typing import Optional, Any

# Conditional absolute imports
try:
    from data_processing.external.numpy_support import NumpyProcessor
    HAS_NUMPY_SUPPORT = True
except ImportError:
    HAS_NUMPY_SUPPORT = False

try:
    from data_processing.external.pandas_support import PandasProcessor
    HAS_PANDAS_SUPPORT = True
except ImportError:
    HAS_PANDAS_SUPPORT = False

# Conditional relative imports
try:
    from ..external.advanced_analytics import AdvancedAnalytics
    HAS_ADVANCED_ANALYTICS = True
except ImportError:
    HAS_ADVANCED_ANALYTICS = False

class OptionalFeatureManager:
    """Manager for optional features."""
    
    def __init__(self):
        self.features = {
            'numpy': HAS_NUMPY_SUPPORT,
            'pandas': HAS_PANDAS_SUPPORT,
            'advanced_analytics': HAS_ADVANCED_ANALYTICS
        }
    
    def get_available_features(self) -> dict:
        """Get available optional features."""
        return self.features.copy()
    
    def create_numpy_processor(self) -> Optional[Any]:
        """Create numpy processor if available."""
        if HAS_NUMPY_SUPPORT:
            return NumpyProcessor()
        return None
    
    def create_pandas_processor(self) -> Optional[Any]:
        """Create pandas processor if available."""
        if HAS_PANDAS_SUPPORT:
            return PandasProcessor()
        return None
    
    def create_advanced_analytics(self) -> Optional[Any]:
        """Create advanced analytics if available."""
        if HAS_ADVANCED_ANALYTICS:
            return AdvancedAnalytics()
        return None
```

### 8.6 Import Resolution and Debugging

#### 8.6.1 Understanding Import Resolution

```python
# data_processing/debug/import_resolver.py
"""
Tools for debugging import resolution.
"""

import sys
import importlib.util
from typing import List, Dict, Any

def trace_import_resolution(module_name: str) -> Dict[str, Any]:
    """Trace how Python resolves a module import."""
    print(f"Tracing import resolution for: {module_name}")
    
    # Check if module is already imported
    if module_name in sys.modules:
        print(f"✓ Module already in sys.modules: {sys.modules[module_name]}")
        return {"status": "cached", "module": sys.modules[module_name]}
    
    # Try to find the module spec
    try:
        spec = importlib.util.find_spec(module_name)
        if spec is None:
            print(f"✗ Module spec not found")
            return {"status": "not_found"}
        
        print(f"✓ Module spec found:")
        print(f"  Name: {spec.name}")
        print(f"  Origin: {spec.origin}")
        print(f"  Package: {spec.parent}")
        print(f"  Submodule search locations: {spec.submodule_search_locations}")
        
        return {
            "status": "found",
            "spec": spec,
            "name": spec.name,
            "origin": spec.origin,
            "package": spec.parent
        }
    
    except Exception as e:
        print(f"✗ Error finding module spec: {e}")
        return {"status": "error", "error": str(e)}

def debug_relative_import(current_module: str, relative_import: str) -> Dict[str, Any]:
    """Debug relative import resolution."""
    print(f"Debugging relative import: {relative_import} from {current_module}")
    
    # Parse the relative import
    if relative_import.startswith('.'):
        dots = len(relative_import) - len(relative_import.lstrip('.'))
        module_part = relative_import[dots:]
        
        print(f"  Dots: {dots}")
        print(f"  Module part: {module_part}")
        
        # Calculate the absolute module name
        current_parts = current_module.split('.')
        if dots > len(current_parts):
            print(f"✗ Too many dots ({dots}) for current module depth ({len(current_parts)})")
            return {"status": "error", "error": "Too many dots"}
        
        parent_parts = current_parts[:-dots] if dots > 0 else current_parts
        if module_part:
            absolute_name = '.'.join(parent_parts + [module_part])
        else:
            absolute_name = '.'.join(parent_parts)
        
        print(f"  Resolved absolute name: {absolute_name}")
        
        return trace_import_resolution(absolute_name)
    
    else:
        print(f"  Not a relative import, treating as absolute")
        return trace_import_resolution(relative_import)

# Example usage
if __name__ == "__main__":
    # Debug absolute import
    trace_import_resolution("data_processing.core.processors")
    
    # Debug relative import
    debug_relative_import("data_processing.io.writers", "..core.processors")
```

#### 8.6.2 Common Import Issues and Solutions

```python
# data_processing/debug/import_issues.py
"""
Common import issues and their solutions.
"""

import sys
import os
from typing import List, Dict, Any

class ImportIssueDetector:
    """Detect and diagnose common import issues."""
    
    def __init__(self):
        self.issues = []
    
    def check_circular_imports(self, module_name: str) -> List[str]:
        """Check for potential circular import issues."""
        issues = []
        
        # This is a simplified check - in practice, you'd need more sophisticated analysis
        if module_name in sys.modules:
            module = sys.modules[module_name]
            if hasattr(module, '__file__') and module.__file__:
                try:
                    with open(module.__file__, 'r') as f:
                        content = f.read()
                        # Look for imports that might create cycles
                        lines = content.split('\n')
                        for i, line in enumerate(lines):
                            if 'import' in line and not line.strip().startswith('#'):
                                # This is a simplified check
                                if module_name.split('.')[0] in line:
                                    issues.append(f"Potential circular import at line {i+1}: {line.strip()}")
                except Exception as e:
                    issues.append(f"Error reading module file: {e}")
        
        return issues
    
    def check_missing_init_files(self, package_path: str) -> List[str]:
        """Check for missing __init__.py files."""
        issues = []
        
        if not os.path.exists(package_path):
            return [f"Package path does not exist: {package_path}"]
        
        for root, dirs, files in os.walk(package_path):
            # Skip hidden directories and __pycache__
            dirs[:] = [d for d in dirs if not d.startswith('.') and d != '__pycache__']
            
            # Check if directory contains Python files but no __init__.py
            python_files = [f for f in files if f.endswith('.py') and f != '__init__.py']
            if python_files and '__init__.py' not in files:
                issues.append(f"Missing __init__.py in directory with Python files: {root}")
        
        return issues
    
    def check_import_path_issues(self) -> List[str]:
        """Check for common import path issues."""
        issues = []
        
        # Check if current directory is in sys.path
        current_dir = os.getcwd()
        if current_dir not in sys.path:
            issues.append(f"Current directory not in sys.path: {current_dir}")
        
        # Check for non-existent paths in sys.path
        for path in sys.path:
            if path and not os.path.exists(path):
                issues.append(f"Non-existent path in sys.path: {path}")
        
        return issues
    
    def diagnose_import_failure(self, module_name: str) -> Dict[str, Any]:
        """Comprehensive diagnosis of import failure."""
        diagnosis = {
            'module_name': module_name,
            'issues': [],
            'suggestions': []
        }
        
        # Check if it's a built-in module
        if module_name in sys.builtin_module_names:
            diagnosis['issues'].append("Module is built-in but import failed")
            diagnosis['suggestions'].append("Check Python installation")
            return diagnosis
        
        # Check various potential issues
        circular_issues = self.check_circular_imports(module_name)
        if circular_issues:
            diagnosis['issues'].extend(circular_issues)
            diagnosis['suggestions'].append("Refactor to avoid circular imports")
        
        path_issues = self.check_import_path_issues()
        if path_issues:
            diagnosis['issues'].extend(path_issues)
            diagnosis['suggestions'].append("Check sys.path configuration")
        
        # Check if module file exists
        module_parts = module_name.split('.')
        for path in sys.path:
            if not path:
                continue
            
            # Check for module file
            potential_file = os.path.join(path, *module_parts) + '.py'
            if os.path.exists(potential_file):
                diagnosis['suggestions'].append(f"Module file exists at: {potential_file}")
                break
            
            # Check for package directory
            potential_dir = os.path.join(path, *module_parts)
            if os.path.exists(potential_dir):
                init_file = os.path.join(potential_dir, '__init__.py')
                if os.path.exists(init_file):
                    diagnosis['suggestions'].append(f"Package directory exists at: {potential_dir}")
                else:
                    diagnosis['issues'].append(f"Package directory missing __init__.py: {potential_dir}")
                break
        
        return diagnosis

# Example usage
if __name__ == "__main__":
    detector = ImportIssueDetector()
    
    # Diagnose import failure
    diagnosis = detector.diagnose_import_failure("data_processing.nonexistent.module")
    print("Diagnosis:", diagnosis)
    
    # Check for missing __init__.py files
    issues = detector.check_missing_init_files("./data_processing")
    print("Missing __init__.py files:", issues)
```

### 8.7 Best Practices for Import Organization

#### 8.7.1 Import Organization Standards

```python
# data_processing/examples/import_organization.py
"""
Best practices for organizing imports.
"""

# Standard library imports (alphabetical order)
import json
import logging
import os
import sys
from datetime import datetime
from typing import Dict, List, Any, Optional

# Third-party imports (alphabetical order)
# import numpy as np
# import pandas as pd
# import requests

# Local package imports - absolute imports (alphabetical order)
from data_processing.core.processors import DataProcessor
from data_processing.core.validators import DataValidator
from data_processing.io.readers import DataReader
from data_processing.io.writers import DataWriter

# Local package imports - relative imports (if used)
from ..utils.helpers import normalize_text, validate_email
from ..utils.formatters import format_date

# Configure logging
logger = logging.getLogger(__name__)

class WellOrganizedClass:
    """Class demonstrating well-organized imports."""
    
    def __init__(self):
        # Use imported classes
        self.processor = DataProcessor()
        self.validator = DataValidator()
        self.reader = DataReader()
        self.writer = DataWriter()
    
    def process_data(self, data: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
        """Process data using imported functions."""
        # Use imported functions
        processed = []
        for item in data:
            # Normalize text fields
            if 'text' in item:
                item['text'] = normalize_text(item['text'])
            
            # Validate email fields
            if 'email' in item:
                item['email_valid'] = validate_email(item['email'])
            
            # Format date fields
            if 'date' in item:
                item['formatted_date'] = format_date(item['date'])
            
            processed.append(item)
        
        return self.processor.process_data(processed)
```

#### 8.7.2 Import Guidelines

```python
# data_processing/guidelines/import_guidelines.py
"""
Comprehensive import guidelines and examples.
"""

# GUIDELINE 1: Import Order
# 1. Standard library imports
# 2. Third-party library imports  
# 3. Local application imports

# GUIDELINE 2: Import Style
# Preferred: Explicit imports
from data_processing.core.processors import DataProcessor
from data_processing.utils.helpers import validate_email

# Avoid: Wildcard imports (except in specific cases)
# from data_processing.core.processors import *  # Avoid this

# GUIDELINE 3: Absolute vs Relative
# Use absolute imports for:
# - Main package interfaces
# - Imports from distant modules
# - Public APIs

# Use relative imports for:
# - Internal package structure
# - Closely related modules
# - Private implementation details

# GUIDELINE 4: Import Aliases
# Use standard aliases for common libraries
# import numpy as np
# import pandas as pd

# Use descriptive aliases for long module names
from data_processing.core.advanced_processors import AdvancedDataProcessor as ADP

# GUIDELINE 5: Conditional Imports
# Handle optional dependencies gracefully
try:
    from data_processing.external.optional_feature import OptionalProcessor
    HAS_OPTIONAL = True
except ImportError:
    HAS_OPTIONAL = False

class ImportGuidelinesExample:
    """Example class following import guidelines."""
    
    def __init__(self):
        self.processor = DataProcessor()
        self.advanced_processor = ADP() if HAS_OPTIONAL else None
    
    def process_with_guidelines(self, data):
        """Process data following import guidelines."""
        # Basic processing with explicitly imported function
        if validate_email("test@example.com"):
            processed = self.processor.process_data(data)
        
        # Advanced processing if available
        if self.advanced_processor:
            processed = self.advanced_processor.process_with_validation(processed)
        
        return processed
```

### 8.8 Performance Considerations

#### 8.8.1 Import Performance Analysis

```python
# data_processing/performance/import_performance.py
"""
Analysis of import performance implications.
"""

import time
import sys
from typing import Dict, Any

class ImportPerformanceAnalyzer:
    """Analyze import performance."""
    
    def __init__(self):
        self.import_times = {}
    
    def measure_import_time(self, module_name: str) -> float:
        """Measure time to import a module."""
        # Remove from sys.modules if present to force reimport
        if module_name in sys.modules:
            del sys.modules[module_name]
        
        start_time = time.time()
        try:
            __import__(module_name)
            end_time = time.time()
            import_time = end_time - start_time
            self.import_times[module_name] = import_time
            return import_time
        except ImportError:
            return -1
    
    def compare_import_strategies(self) -> Dict[str, Any]:
        """Compare performance of different import strategies."""
        results = {}
        
        # Test absolute imports
        absolute_time = self.measure_import_time('data_processing.core.processors')
        results['absolute_import'] = absolute_time
        
        # Test relative imports (simulated)
        # Note: This is a simplified example
        relative_time = self.measure_import_time('data_processing.io.writers')
        results['relative_import'] = relative_time
        
        # Test module vs function imports
        module_import_time = self.measure_import_time('data_processing.utils.helpers')
        results['module_import'] = module_import_time
        
        return results
    
    def analyze_import_overhead(self) -> Dict[str, Any]:
        """Analyze overhead of different import patterns."""
        analysis = {}
        
        # Measure overhead of importing entire modules vs specific functions
        start_time = time.time()
        from data_processing.core.processors import DataProcessor
        specific_import_time = time.time() - start_time
        
        # Clear the import
        if 'data_processing.core.processors' in sys.modules:
            del sys.modules['data_processing.core.processors']
        
        start_time = time.time()
        import data_processing.core.processors
        module_import_time = time.time() - start_time
        
        analysis['specific_import_time'] = specific_import_time
        analysis['module_import_time'] = module_import_time
        analysis['overhead_ratio'] = specific_import_time / module_import_time if module_import_time > 0 else 0
        
        return analysis

# Example usage
if __name__ == "__main__":
    analyzer = ImportPerformanceAnalyzer()
    
    # Compare import strategies
    comparison = analyzer.compare_import_strategies()
    print("Import strategy comparison:", comparison)
    
    # Analyze import overhead
    overhead = analyzer.analyze_import_overhead()
    print("Import overhead analysis:", overhead)
```

#### 8.8.2 Optimization Strategies

```python
# data_processing/optimization/import_optimization.py
"""
Strategies for optimizing import performance.
"""

import sys
from typing import Any, Dict, Callable

class LazyImportOptimizer:
    """Optimize imports using lazy loading."""
    
    def __init__(self):
        self._cached_modules = {}
        self._import_functions = {}
    
    def register_lazy_import(self, name: str, import_func: Callable) -> None:
        """Register a lazy import function."""
        self._import_functions[name] = import_func
    
    def get_module(self, name: str) -> Any:
        """Get module, importing lazily if needed."""
        if name not in self._cached_modules:
            if name in self._import_functions:
                self._cached_modules[name] = self._import_functions[name]()
            else:
                raise ValueError(f"No lazy import registered for {name}")
        
        return self._cached_modules[name]

# Global lazy import optimizer
lazy_optimizer = LazyImportOptimizer()

# Register lazy imports
lazy_optimizer.register_lazy_import(
    'data_processor',
    lambda: __import__('data_processing.core.processors', fromlist=['DataProcessor']).DataProcessor
)

lazy_optimizer.register_lazy_import(
    'data_validator', 
    lambda: __import__('data_processing.core.validators', fromlist=['DataValidator']).DataValidator
)

class OptimizedDataManager:
    """Data manager with optimized imports."""
    
    def __init__(self):
        # Don't import modules until needed
        self._processor = None
        self._validator = None
    
    @property
    def processor(self):
        """Lazy-loaded processor."""
        if self._processor is None:
            self._processor = lazy_optimizer.get_module('data_processor')()
        return self._processor
    
    @property
    def validator(self):
        """Lazy-loaded validator."""
        if self._validator is None:
            self._validator = lazy_optimizer.get_module('data_validator')()
        return self._validator
    
    def process_data(self, data):
        """Process data using lazy-loaded modules."""
        # Modules are only imported when first accessed
        validated = self.validator.validate_batch(data)
        processed = self.processor.process_data(validated)
        return processed
```

