# API Automation Testing Project

A comprehensive API automation testing framework built with Python, Pytest, and Requests. This project automates testing of REST APIs with structured test organization, fixtures, and HTML reporting.

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running Tests](#running-tests)
- [Project Components](#project-components)
- [Test Cases](#test-cases)
- [Reports](#reports)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## 🎯 Overview

This project provides a robust framework for automating API testing using:
- **Pytest** - Testing framework
- **Requests** - HTTP library for API calls
- **Pytest-HTML** - HTML test reporting
- **JSONPlaceholder** - Mock API for testing

The framework is designed to be easily scalable and maintainable, following SDET (Software Development Engineer in Test) best practices.

## 📁 Project Structure

```
API Automation/
├── conftest.py                 # Pytest configuration and fixtures
├── pytest.ini                  # Pytest settings and CLI defaults
├── requirements.txt            # Python dependencies
├── README.md                   # This file
├── data/
│   └── test_data.json         # Test data and fixtures
├── reports/                    # Generated HTML test reports
│   └── report_YYYY-MM-DD.html # Timestamped test reports
├── tests/
│   └── test_users_api.py      # Test cases for Users API
└── utils/
    └── api_client.py          # Reusable API client wrapper
```

### Directory Descriptions

- **`conftest.py`** - Pytest configuration file containing:
  - Session-level setup/teardown fixtures
  - Test data loading fixtures
  - Custom pytest hooks

- **`utils/`** - Utility modules:
  - `api_client.py` - Centralized API client for HTTP operations (GET, POST, PUT, DELETE)

- **`tests/`** - Test modules:
  - `test_users_api.py` - API test cases for CRUD operations on users

- **`data/`** - Test data:
  - `test_data.json` - User data for creating test objects

- **`reports/`** - Generated test reports (created after test execution)

## 📦 Prerequisites

- Python 3.8 or higher
- pip (Python package manager)
- Virtual environment (recommended)

## 🚀 Installation

### 1. Clone or Extract the Project

Navigate to the project directory:
```bash
cd "path/to/API Automation"
```

### 2. Create a Virtual Environment

**On Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**On Windows (Command Prompt):**
```bash
python -m venv venv
venv\Scripts\activate.bat
```

**On macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Verify Installation

```bash
pytest --version
```

## ⚙️ Configuration

### Pytest Configuration (pytest.ini)

The `pytest.ini` file contains default CLI options:
```ini
[pytest]
addopts = --maxfail=3 --disable-warnings -v --html=reports/report.html --self-contained-html
```

**Options explained:**
- `--maxfail=3` - Stop after 3 test failures
- `--disable-warnings` - Suppress warnings
- `-v` - Verbose output
- `--html=reports/report.html` - Generate HTML report
- `--self-contained-html` - Create standalone HTML report

### Base API URL

The base URL is configured in `utils/api_client.py`:
```python
BASE_URL = "https://jsonplaceholder.typicode.com"
```

To change the API endpoint, modify this value in the `APIClient` class.

## 🧪 Running Tests

### Run All Tests

```bash
pytest
```

### Run with Verbose Output

```bash
pytest -v
```

### Run with Print Statements Displayed

```bash
pytest -s
```

### Run a Specific Test File

```bash
pytest tests/test_users_api.py
```

### Run a Specific Test Case

```bash
pytest tests/test_users_api.py::test_get_users
```

### Run Tests with Custom Report Path

```bash
pytest --html=reports/custom_report.html --self-contained-html
```

### Run Tests in Parallel (if pytest-xdist is installed)

```bash
pip install pytest-xdist
pytest -n auto
```

## 🔧 Project Components

### APIClient Class (`utils/api_client.py`)

A wrapper class around the `requests` library providing a clean interface for API calls.

**Methods:**
- `get(endpoint)` - GET request to the API
- `post(endpoint, data)` - POST request with JSON data
- `put(endpoint, data)` - PUT request for updates
- `delete(endpoint)` - DELETE request

**Example Usage:**
```python
api_client = APIClient()
response = api_client.get("users")
response = api_client.post("users", {"name": "John", "email": "john@example.com"})
```

### Fixtures (`conftest.py`)

- **`api_client`** - Module-scoped fixture providing an APIClient instance
- **`load_user_data`** - Session-scoped fixture loading test data from JSON
- **Setup/Teardown** - Session-level setup and cleanup hooks

### Test Data (`data/test_data.json`)

Contains test fixtures for creating users:
```json
{
    "new_user": {
      "name": "subrat",
      "username": "darknight",
      "email": "subrat@example.com"
    }
}
```

## 📝 Test Cases

### test_users_api.py

#### 1. `test_get_users()`
- **Purpose:** Verify retrieving list of users
- **Method:** GET `/users`
- **Assertions:**
  - Response status is 200
  - Response contains users

#### 2. `test_create_users()`
- **Purpose:** Verify user creation with unique email
- **Method:** POST `/users`
- **Features:** 
  - Uses UUID for unique email generation
  - Validates created user data
- **Assertions:**
  - Response status is 201 (Created)
  - User name matches expected value
  - ID is returned in response

#### 3. `test_update_users()`
- **Purpose:** Verify user update functionality
- **Method:** PUT `/users/{id}`
- **Assertions:**
  - Response status is 200
  - Updated name matches sent data

#### 4. `test_delete_users()`
- **Purpose:** Verify user deletion
- **Method:** DELETE `/users/{id}`
- **Assertions:**
  - Response status is 200

## 📊 Reports

Test reports are automatically generated in the `reports/` directory with timestamps.

### Accessing Reports

After running tests, open the generated HTML report:
```bash
reports/report_YYYY-MM-DD_HH-MM-SS.html
```

**Report Features:**
- Test execution status (Pass/Fail)
- Execution time for each test
- Print statements output
- Response JSON for debugging
- Summary statistics

## ✅ Best Practices

1. **Use Fixtures** - Leverage pytest fixtures for setup/teardown and data
2. **Unique Test Data** - Use UUID or timestamps for unique identifiers
3. **Meaningful Assertions** - Include descriptive assertions
4. **Centralized API Client** - Use the APIClient class for all HTTP calls
5. **Test Organization** - Group related tests in modules
6. **Data Separation** - Keep test data in JSON files
7. **Error Handling** - Check response status codes and content
8. **CI/CD Integration** - Configure for automated pipeline execution

## 🐛 Troubleshooting

### Virtual Environment Not Activating

**Windows PowerShell Error:** "cannot be loaded because running scripts is disabled"

**Solution:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Module Not Found Error

**Error:** `ModuleNotFoundError: No module named 'requests'`

**Solution:**
```bash
pip install -r requirements.txt
```

### Port Already in Use

**Solution:** Change the base URL to a different port in `api_client.py`

### Tests Failing with Connection Error

**Solution:** Verify internet connection and that the API endpoint is accessible:
```bash
curl https://jsonplaceholder.typicode.com/users
```

### Report Not Generated

**Solution:** Ensure the `reports/` directory exists:
```bash
mkdir reports
```


