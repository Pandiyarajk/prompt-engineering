# Module 4: Prompt Engineering for Automation Testing

## Lesson 1: Test Automation Code Generation

---

## 🎯 Lesson Overview

**Time Required:** 90 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Modules 1-3 completed, basic Python knowledge

In this lesson, you'll learn:
- How to generate complete test automation code with AI
- Creating Selenium WebDriver test scripts
- Building API test automation
- Generating mobile tests with Appium
- Best practices for code generation and review

---

## 📚 Introduction

AI can dramatically accelerate test automation by generating code for test scripts, frameworks, and utilities. Instead of writing boilerplate code from scratch, you can generate complete, working test automation code in minutes.

### What AI Can Generate for You

```
┌─────────────────────────────────────────────────────────────────┐
│  AI-GENERATED AUTOMATION CODE                                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  📝 Test Scripts                                                 │
│     - Selenium WebDriver tests                                  │
│     - Playwright tests                                          │
│     - Cypress tests                                             │
│     - pytest/unittest tests                                     │
│                                                                  │
│  🏗️ Frameworks                                                   │
│     - Page Object Model classes                                 │
│     - Base classes and utilities                                │
│     - Configuration management                                  │
│     - Test fixtures                                             │
│                                                                  │
│  🔌 API Tests                                                    │
│     - REST API test automation                                  │
│     - Schema validation                                         │
│     - Authentication handling                                   │
│                                                                  │
│  📱 Mobile Tests                                                 │
│     - Appium test scripts                                       │
│     - iOS and Android automation                                │
│     - Mobile gestures and interactions                          │
│                                                                  │
│  ⚙️ Infrastructure                                               │
│     - CI/CD configurations                                      │
│     - Docker setups                                             │
│     - Reporting configurations                                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔧 The Code Generation Framework

### The Process

```
┌──────────────┐    ┌───────────────┐    ┌──────────────┐
│ REQUIREMENTS │    │   AI PROMPT   │    │  GENERATED   │
│              │ → │               │ → │    CODE      │
│ - Feature    │    │ - Role        │    │              │
│ - Tech stack │    │ - Context     │    │ - Complete   │
│ - Patterns   │    │ - Task        │    │ - Working    │
│ - Standards  │    │ - Format      │    │ - Documented │
└──────────────┘    └───────────────┘    └──────┬───────┘
                                                 │
                                                 ▼
┌──────────────┐    ┌───────────────┐    ┌──────────────┐
│   INTEGRATE  │    │    REFINE     │    │    REVIEW    │
│              │ ← │               │ ← │              │
│ - Add to     │    │ - Customize   │    │ - Test run   │
│   project    │    │ - Optimize    │    │ - Code check │
│ - Connect    │    │ - Add more    │    │ - Security   │
└──────────────┘    └───────────────┘    └──────────────┘
```

### The Core Prompt Template

This template works for most automation code generation:

```
Act as a Test Automation Engineer expert in [framework/language].

Task: Generate [type of code] for [test scenario]

Technical Requirements:
- Language: [Python/Java/JavaScript/TypeScript]
- Framework: [Selenium/Playwright/Cypress/Appium]
- Test Framework: [pytest/unittest/Jest/Mocha]
- Design Pattern: [Page Object Model/Screenplay/etc]

Application Details:
[URL, element locators, expected behaviors]

Requirements:
[List specific requirements]

Include:
- Complete working code
- All imports and dependencies
- Error handling
- Comments and docstrings
- Type hints (if applicable)
- Example usage

Follow best practices for:
- Code organization
- Naming conventions
- Maintainability
- [Your coding standards]
```

---

## 🌐 Selenium WebDriver Test Generation

### Example 1: Complete Login Test with Page Object Model

**The Prompt:**

```
Act as a Senior Test Automation Engineer expert in Selenium with Python.

Task: Create a complete Selenium test script for user login

Application Details:
- URL: https://practice.example.com/login
- Username field: id="username"
- Password field: id="password"
- Login button: id="login-button"
- Success indicator: element with class="welcome-message"
- Error message: element with class="error-alert"

Requirements:
- Use Page Object Model pattern
- Use pytest framework
- Include setup and teardown with fixtures
- Add explicit waits (WebDriverWait)
- Handle both success and failure cases
- Include logging
- Use Chrome WebDriver
- Support headless mode via command line

Include these files:
1. pages/base_page.py - Base page class
2. pages/login_page.py - Login page object
3. tests/test_login.py - Test file with pytest
4. conftest.py - Fixtures
5. pytest.ini - Configuration
6. requirements.txt - Dependencies

Follow PEP 8 standards and include detailed docstrings.
```

**Expected AI Output:**

### File: pages/base_page.py

```python
"""
Base Page class containing common methods for all page objects.
"""
import logging
from typing import Tuple
from selenium.webdriver.remote.webdriver import WebDriver
from selenium.webdriver.remote.webelement import WebElement
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException, NoSuchElementException

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)


class BasePage:
    """Base class for all page objects."""
    
    def __init__(self, driver: WebDriver, timeout: int = 10):
        """
        Initialize the base page.
        
        Args:
            driver: Selenium WebDriver instance
            timeout: Default wait timeout in seconds
        """
        self.driver = driver
        self.timeout = timeout
        self.wait = WebDriverWait(driver, timeout)
        logger.info(f"Initialized {self.__class__.__name__}")
    
    def find_element(self, locator: Tuple[str, str]) -> WebElement:
        """
        Find an element with explicit wait.
        
        Args:
            locator: Tuple of (By method, locator string)
            
        Returns:
            WebElement if found
            
        Raises:
            TimeoutException if element not found within timeout
        """
        try:
            element = self.wait.until(
                EC.presence_of_element_located(locator)
            )
            logger.debug(f"Found element: {locator}")
            return element
        except TimeoutException:
            logger.error(f"Element not found: {locator}")
            raise
    
    def click_element(self, locator: Tuple[str, str]) -> None:
        """
        Click an element with explicit wait for clickability.
        
        Args:
            locator: Tuple of (By method, locator string)
        """
        try:
            element = self.wait.until(
                EC.element_to_be_clickable(locator)
            )
            element.click()
            logger.info(f"Clicked element: {locator}")
        except TimeoutException:
            logger.error(f"Element not clickable: {locator}")
            raise
    
    def enter_text(self, locator: Tuple[str, str], text: str) -> None:
        """
        Enter text into an element.
        
        Args:
            locator: Tuple of (By method, locator string)
            text: Text to enter
        """
        element = self.find_element(locator)
        element.clear()
        element.send_keys(text)
        logger.info(f"Entered text into: {locator}")
    
    def get_element_text(self, locator: Tuple[str, str]) -> str:
        """
        Get text from an element.
        
        Args:
            locator: Tuple of (By method, locator string)
            
        Returns:
            Text content of the element
        """
        element = self.find_element(locator)
        return element.text
    
    def is_element_visible(self, locator: Tuple[str, str]) -> bool:
        """
        Check if element is visible.
        
        Args:
            locator: Tuple of (By method, locator string)
            
        Returns:
            True if visible, False otherwise
        """
        try:
            self.wait.until(EC.visibility_of_element_located(locator))
            return True
        except TimeoutException:
            return False
    
    def wait_for_url_contains(self, text: str) -> bool:
        """
        Wait for URL to contain specific text.
        
        Args:
            text: Text to check in URL
            
        Returns:
            True if URL contains text within timeout
        """
        try:
            self.wait.until(EC.url_contains(text))
            return True
        except TimeoutException:
            return False
    
    def take_screenshot(self, name: str) -> str:
        """
        Take a screenshot and save it.
        
        Args:
            name: Name for the screenshot file
            
        Returns:
            Path to saved screenshot
        """
        filename = f"screenshots/{name}.png"
        self.driver.save_screenshot(filename)
        logger.info(f"Screenshot saved: {filename}")
        return filename
```

### File: pages/login_page.py

```python
"""
Login Page object representing the login page.
"""
from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webdriver import WebDriver
from pages.base_page import BasePage, logger


class LoginPage(BasePage):
    """Page object for the Login page."""
    
    # Page URL
    URL = "https://practice.example.com/login"
    
    # Locators
    USERNAME_FIELD = (By.ID, "username")
    PASSWORD_FIELD = (By.ID, "password")
    LOGIN_BUTTON = (By.ID, "login-button")
    WELCOME_MESSAGE = (By.CLASS_NAME, "welcome-message")
    ERROR_ALERT = (By.CLASS_NAME, "error-alert")
    
    def __init__(self, driver: WebDriver, timeout: int = 10):
        """Initialize Login Page."""
        super().__init__(driver, timeout)
    
    def navigate(self) -> 'LoginPage':
        """
        Navigate to the login page.
        
        Returns:
            Self for method chaining
        """
        self.driver.get(self.URL)
        logger.info(f"Navigated to: {self.URL}")
        return self
    
    def enter_username(self, username: str) -> 'LoginPage':
        """
        Enter username.
        
        Args:
            username: Username to enter
            
        Returns:
            Self for method chaining
        """
        self.enter_text(self.USERNAME_FIELD, username)
        return self
    
    def enter_password(self, password: str) -> 'LoginPage':
        """
        Enter password.
        
        Args:
            password: Password to enter
            
        Returns:
            Self for method chaining
        """
        self.enter_text(self.PASSWORD_FIELD, password)
        return self
    
    def click_login(self) -> None:
        """Click the login button."""
        self.click_element(self.LOGIN_BUTTON)
    
    def login(self, username: str, password: str) -> None:
        """
        Perform complete login action.
        
        Args:
            username: Username to use
            password: Password to use
        """
        logger.info(f"Attempting login with username: {username}")
        self.enter_username(username)
        self.enter_password(password)
        self.click_login()
    
    def is_login_successful(self) -> bool:
        """
        Check if login was successful.
        
        Returns:
            True if welcome message is visible
        """
        return self.is_element_visible(self.WELCOME_MESSAGE)
    
    def get_welcome_message(self) -> str:
        """
        Get the welcome message text.
        
        Returns:
            Welcome message text
        """
        return self.get_element_text(self.WELCOME_MESSAGE)
    
    def is_error_displayed(self) -> bool:
        """
        Check if error message is displayed.
        
        Returns:
            True if error alert is visible
        """
        return self.is_element_visible(self.ERROR_ALERT)
    
    def get_error_message(self) -> str:
        """
        Get the error message text.
        
        Returns:
            Error message text
        """
        return self.get_element_text(self.ERROR_ALERT)
```

### File: tests/test_login.py

```python
"""
Test cases for Login functionality.
"""
import pytest
from pages.login_page import LoginPage


class TestLogin:
    """Test suite for login functionality."""
    
    @pytest.fixture(autouse=True)
    def setup(self, driver):
        """Set up test with login page."""
        self.login_page = LoginPage(driver)
        self.login_page.navigate()
    
    # ==================== POSITIVE TESTS ====================
    
    def test_valid_login(self):
        """Test login with valid credentials."""
        self.login_page.login("testuser", "Password123!")
        
        assert self.login_page.is_login_successful(), \
            "Login should be successful with valid credentials"
        assert "Welcome" in self.login_page.get_welcome_message(), \
            "Welcome message should be displayed"
    
    def test_login_with_remember_me(self, driver):
        """Test login with remember me option checked."""
        # This test assumes there's a remember me checkbox
        self.login_page.login("testuser", "Password123!")
        
        assert self.login_page.is_login_successful(), \
            "Login with remember me should succeed"
    
    # ==================== NEGATIVE TESTS ====================
    
    def test_invalid_password(self):
        """Test login with invalid password."""
        self.login_page.login("testuser", "wrongpassword")
        
        assert self.login_page.is_error_displayed(), \
            "Error message should be displayed for invalid password"
    
    def test_invalid_username(self):
        """Test login with non-existent username."""
        self.login_page.login("nonexistent@test.com", "Password123!")
        
        assert self.login_page.is_error_displayed(), \
            "Error message should be displayed for invalid username"
    
    def test_empty_username(self):
        """Test login with empty username."""
        self.login_page.login("", "Password123!")
        
        assert self.login_page.is_error_displayed(), \
            "Error message should be displayed for empty username"
    
    def test_empty_password(self):
        """Test login with empty password."""
        self.login_page.login("testuser", "")
        
        assert self.login_page.is_error_displayed(), \
            "Error message should be displayed for empty password"
    
    def test_empty_credentials(self):
        """Test login with empty username and password."""
        self.login_page.login("", "")
        
        assert self.login_page.is_error_displayed(), \
            "Error message should be displayed for empty credentials"
    
    # ==================== EDGE CASE TESTS ====================
    
    @pytest.mark.parametrize("username,password", [
        ("test@example.com", "Password123!"),  # Valid email format
        ("test.user@example.com", "Pass@1234"),  # Email with dots
        ("user", "Pass12345678901234567890"),  # Long password
    ])
    def test_various_valid_credentials(self, username, password):
        """Test login with various valid credential formats."""
        self.login_page.login(username, password)
        # Adjust assertion based on whether these are actual valid users
    
    # ==================== SECURITY TESTS ====================
    
    def test_sql_injection_username(self):
        """Test SQL injection in username field."""
        self.login_page.login("' OR '1'='1", "password")
        
        # Should show error, not login
        assert self.login_page.is_error_displayed() or \
               not self.login_page.is_login_successful(), \
            "SQL injection should not bypass login"
    
    def test_xss_in_username(self):
        """Test XSS in username field."""
        self.login_page.login("<script>alert('xss')</script>", "password")
        
        # Should show error, input should be sanitized
        assert self.login_page.is_error_displayed(), \
            "XSS attempt should be handled safely"
```

### File: conftest.py

```python
"""
Pytest configuration and fixtures.
"""
import pytest
import os
from datetime import datetime
from selenium import webdriver
from selenium.webdriver.chrome.options import Options as ChromeOptions
from selenium.webdriver.firefox.options import Options as FirefoxOptions
from selenium.webdriver.chrome.service import Service as ChromeService
from webdriver_manager.chrome import ChromeDriverManager


def pytest_addoption(parser):
    """Add command line options."""
    parser.addoption(
        "--browser",
        action="store",
        default="chrome",
        help="Browser to use: chrome or firefox"
    )
    parser.addoption(
        "--headless",
        action="store_true",
        default=False,
        help="Run browser in headless mode"
    )


@pytest.fixture(scope="session")
def browser(request):
    """Get browser name from command line."""
    return request.config.getoption("--browser")


@pytest.fixture(scope="session")
def headless(request):
    """Get headless option from command line."""
    return request.config.getoption("--headless")


@pytest.fixture(scope="function")
def driver(browser, headless):
    """
    Create and configure WebDriver instance.
    
    Yields:
        WebDriver instance
    """
    driver = None
    
    if browser.lower() == "chrome":
        chrome_options = ChromeOptions()
        if headless:
            chrome_options.add_argument("--headless")
        chrome_options.add_argument("--no-sandbox")
        chrome_options.add_argument("--disable-dev-shm-usage")
        chrome_options.add_argument("--window-size=1920,1080")
        
        service = ChromeService(ChromeDriverManager().install())
        driver = webdriver.Chrome(service=service, options=chrome_options)
        
    elif browser.lower() == "firefox":
        firefox_options = FirefoxOptions()
        if headless:
            firefox_options.add_argument("--headless")
        driver = webdriver.Firefox(options=firefox_options)
    
    else:
        raise ValueError(f"Unsupported browser: {browser}")
    
    driver.maximize_window()
    driver.implicitly_wait(5)
    
    yield driver
    
    # Teardown
    driver.quit()


@pytest.fixture(scope="function", autouse=True)
def screenshot_on_failure(request, driver):
    """Take screenshot on test failure."""
    yield
    
    # Check if test failed
    if request.node.rep_call.failed:
        # Create screenshots directory if it doesn't exist
        os.makedirs("screenshots", exist_ok=True)
        
        # Generate filename
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        test_name = request.node.name
        filename = f"screenshots/{test_name}_{timestamp}.png"
        
        # Take screenshot
        driver.save_screenshot(filename)
        print(f"\nScreenshot saved: {filename}")


@pytest.hookimpl(tryfirst=True, hookwrapper=True)
def pytest_runtest_makereport(item, call):
    """Hook to access test results in fixtures."""
    outcome = yield
    rep = outcome.get_result()
    setattr(item, f"rep_{rep.when}", rep)
```

### File: pytest.ini

```ini
[pytest]
# Test discovery
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*

# Output settings
addopts = -v --tb=short --strict-markers

# Markers
markers =
    smoke: Quick smoke tests
    regression: Full regression tests
    slow: Tests that take a long time

# Logging
log_cli = true
log_cli_level = INFO
log_cli_format = %(asctime)s [%(levelname)s] %(message)s
log_cli_date_format = %H:%M:%S
```

### File: requirements.txt

```
selenium>=4.15.0
pytest>=7.4.0
pytest-html>=4.1.0
webdriver-manager>=4.0.0
python-dotenv>=1.0.0
```

---

### Example 2: Data-Driven Test

**The Prompt:**

```
Act as a Test Automation Engineer specializing in data-driven testing.

Create a data-driven Selenium test using pytest and parametrize.

Test Scenario: Registration form validation

Form Fields:
- Email (id="email")
- Password (id="password")
- Confirm Password (id="confirm-password")
- Terms checkbox (id="terms")
- Submit button (id="submit")
- Success message (class="success-message")
- Error message (class="error-message")

Create a CSV file with test data covering:
- Valid registration
- Invalid email formats
- Password too short
- Passwords don't match
- Terms not accepted

Requirements:
- Read test data from CSV file
- Use @pytest.mark.parametrize
- Clear assertions for each case
- Take screenshots on failure
- Generate HTML report

Include:
- Test file
- CSV data file
- Utility for CSV reading
- pytest-html configuration
```

**Expected Output - Test Data CSV:**

```csv
# test_data/registration_data.csv
test_id,email,password,confirm_password,accept_terms,expected_result,expected_message
TC001,valid@test.com,Password123!,Password123!,true,success,Registration successful
TC002,invalid-email,Password123!,Password123!,true,error,Invalid email format
TC003,test@test.com,short,short,true,error,Password must be at least 8 characters
TC004,test@test.com,Password123!,Different456!,true,error,Passwords do not match
TC005,test@test.com,Password123!,Password123!,false,error,Please accept terms
TC006,,Password123!,Password123!,true,error,Email is required
TC007,test@test.com,,,true,error,Password is required
```

**Expected Output - Data-Driven Test:**

```python
"""
Data-driven tests for registration form.
"""
import pytest
import csv
from pathlib import Path
from pages.registration_page import RegistrationPage


def load_test_data():
    """Load test data from CSV file."""
    data = []
    csv_path = Path(__file__).parent.parent / "test_data" / "registration_data.csv"
    
    with open(csv_path, 'r') as file:
        reader = csv.DictReader(file)
        for row in reader:
            # Skip comment rows
            if not row['test_id'].startswith('#'):
                data.append((
                    row['test_id'],
                    row['email'],
                    row['password'],
                    row['confirm_password'],
                    row['accept_terms'].lower() == 'true',
                    row['expected_result'],
                    row['expected_message']
                ))
    return data


class TestRegistration:
    """Data-driven registration tests."""
    
    @pytest.fixture(autouse=True)
    def setup(self, driver):
        """Set up test with registration page."""
        self.reg_page = RegistrationPage(driver)
        self.reg_page.navigate()
    
    @pytest.mark.parametrize(
        "test_id,email,password,confirm_password,accept_terms,expected_result,expected_message",
        load_test_data(),
        ids=lambda x: x if isinstance(x, str) and x.startswith('TC') else ''
    )
    def test_registration(
        self, test_id, email, password, confirm_password, 
        accept_terms, expected_result, expected_message
    ):
        """Test registration with various data combinations."""
        # Fill form
        self.reg_page.enter_email(email)
        self.reg_page.enter_password(password)
        self.reg_page.enter_confirm_password(confirm_password)
        
        if accept_terms:
            self.reg_page.check_terms()
        
        # Submit
        self.reg_page.click_submit()
        
        # Verify result
        if expected_result == 'success':
            assert self.reg_page.is_success_displayed(), \
                f"{test_id}: Expected success but got error"
            assert expected_message in self.reg_page.get_success_message(), \
                f"{test_id}: Success message mismatch"
        else:
            assert self.reg_page.is_error_displayed(), \
                f"{test_id}: Expected error but got success"
            assert expected_message in self.reg_page.get_error_message(), \
                f"{test_id}: Error message mismatch"
```

---

## 🔌 API Test Automation

### Example 1: REST API Testing

**The Prompt:**

```
Act as an API Test Automation Expert using Python requests library.

Create comprehensive API tests for a User Management API.

API Endpoints:
1. POST /api/v1/users - Create user
2. GET /api/v1/users/{id} - Get user
3. PUT /api/v1/users/{id} - Update user
4. DELETE /api/v1/users/{id} - Delete user

Base URL: https://api.example.com
Authentication: Bearer token in header

User Schema:
{
  "id": "string (UUID, read-only)",
  "email": "string (required, unique)",
  "firstName": "string (required)",
  "lastName": "string (optional)",
  "age": "integer (optional, 13-120)",
  "createdAt": "string (ISO date, read-only)"
}

Requirements:
- Use pytest framework
- Test all CRUD operations
- Validate response status codes
- Validate response schema with jsonschema
- Test error scenarios (404, 400, 401)
- Use fixtures for test data and cleanup
- Use environment variables for base URL and token
- Add logging

Include:
1. api/base_client.py - Base API client
2. api/user_api.py - User API client
3. tests/test_users_api.py - CRUD tests
4. conftest.py with fixtures
5. schemas/user_schema.json - JSON Schema
6. .env.example file
7. requirements.txt
```

**Expected Output - Base API Client:**

```python
"""
Base API client with common functionality.
"""
import logging
import os
from typing import Dict, Any, Optional
import requests
from dotenv import load_dotenv

load_dotenv()

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)


class BaseAPIClient:
    """Base class for API clients."""
    
    def __init__(self, base_url: Optional[str] = None):
        """
        Initialize API client.
        
        Args:
            base_url: API base URL (defaults to env variable)
        """
        self.base_url = base_url or os.getenv("API_BASE_URL")
        self.token = os.getenv("API_TOKEN")
        self.session = requests.Session()
        self._setup_session()
    
    def _setup_session(self) -> None:
        """Configure session with default headers."""
        self.session.headers.update({
            "Content-Type": "application/json",
            "Accept": "application/json",
        })
        if self.token:
            self.session.headers["Authorization"] = f"Bearer {self.token}"
    
    def _log_request(self, method: str, url: str, **kwargs) -> None:
        """Log request details."""
        logger.info(f"Request: {method} {url}")
        if kwargs.get("json"):
            logger.debug(f"Body: {kwargs['json']}")
    
    def _log_response(self, response: requests.Response) -> None:
        """Log response details."""
        logger.info(f"Response: {response.status_code}")
        logger.debug(f"Body: {response.text[:500]}")
    
    def get(self, endpoint: str, **kwargs) -> requests.Response:
        """
        Make GET request.
        
        Args:
            endpoint: API endpoint (without base URL)
            **kwargs: Additional request arguments
            
        Returns:
            Response object
        """
        url = f"{self.base_url}{endpoint}"
        self._log_request("GET", url, **kwargs)
        response = self.session.get(url, **kwargs)
        self._log_response(response)
        return response
    
    def post(self, endpoint: str, data: Dict = None, **kwargs) -> requests.Response:
        """
        Make POST request.
        
        Args:
            endpoint: API endpoint
            data: Request body as dict
            **kwargs: Additional request arguments
            
        Returns:
            Response object
        """
        url = f"{self.base_url}{endpoint}"
        self._log_request("POST", url, json=data, **kwargs)
        response = self.session.post(url, json=data, **kwargs)
        self._log_response(response)
        return response
    
    def put(self, endpoint: str, data: Dict = None, **kwargs) -> requests.Response:
        """Make PUT request."""
        url = f"{self.base_url}{endpoint}"
        self._log_request("PUT", url, json=data, **kwargs)
        response = self.session.put(url, json=data, **kwargs)
        self._log_response(response)
        return response
    
    def delete(self, endpoint: str, **kwargs) -> requests.Response:
        """Make DELETE request."""
        url = f"{self.base_url}{endpoint}"
        self._log_request("DELETE", url, **kwargs)
        response = self.session.delete(url, **kwargs)
        self._log_response(response)
        return response
```

**Expected Output - API Tests:**

```python
"""
API tests for User Management endpoints.
"""
import pytest
import uuid
from jsonschema import validate, ValidationError
from api.user_api import UserAPI


class TestUserAPI:
    """Test suite for User API."""
    
    @pytest.fixture(autouse=True)
    def setup(self, user_api):
        """Set up test with API client."""
        self.api = user_api
    
    # ==================== CREATE USER TESTS ====================
    
    def test_create_user_success(self, valid_user_data):
        """Test creating a user with valid data."""
        response = self.api.create_user(valid_user_data)
        
        assert response.status_code == 201
        data = response.json()
        assert data["email"] == valid_user_data["email"]
        assert data["firstName"] == valid_user_data["firstName"]
        assert "id" in data
        assert "createdAt" in data
    
    def test_create_user_missing_email(self, valid_user_data):
        """Test creating user without email."""
        del valid_user_data["email"]
        response = self.api.create_user(valid_user_data)
        
        assert response.status_code == 400
        assert "email" in response.json().get("error", "").lower()
    
    def test_create_user_invalid_email(self, valid_user_data):
        """Test creating user with invalid email format."""
        valid_user_data["email"] = "invalid-email"
        response = self.api.create_user(valid_user_data)
        
        assert response.status_code == 400
    
    def test_create_user_duplicate_email(self, valid_user_data, created_user):
        """Test creating user with existing email."""
        valid_user_data["email"] = created_user["email"]
        response = self.api.create_user(valid_user_data)
        
        assert response.status_code == 409  # Conflict
    
    # ==================== GET USER TESTS ====================
    
    def test_get_user_success(self, created_user):
        """Test getting an existing user."""
        response = self.api.get_user(created_user["id"])
        
        assert response.status_code == 200
        data = response.json()
        assert data["id"] == created_user["id"]
        assert data["email"] == created_user["email"]
    
    def test_get_user_not_found(self):
        """Test getting a non-existent user."""
        fake_id = str(uuid.uuid4())
        response = self.api.get_user(fake_id)
        
        assert response.status_code == 404
    
    def test_get_user_invalid_id(self):
        """Test getting user with invalid ID format."""
        response = self.api.get_user("invalid-id-format")
        
        assert response.status_code in [400, 404]
    
    # ==================== UPDATE USER TESTS ====================
    
    def test_update_user_success(self, created_user):
        """Test updating an existing user."""
        update_data = {"firstName": "UpdatedName"}
        response = self.api.update_user(created_user["id"], update_data)
        
        assert response.status_code == 200
        assert response.json()["firstName"] == "UpdatedName"
    
    def test_update_user_not_found(self, valid_user_data):
        """Test updating a non-existent user."""
        fake_id = str(uuid.uuid4())
        response = self.api.update_user(fake_id, valid_user_data)
        
        assert response.status_code == 404
    
    # ==================== DELETE USER TESTS ====================
    
    def test_delete_user_success(self, created_user):
        """Test deleting an existing user."""
        response = self.api.delete_user(created_user["id"])
        
        assert response.status_code in [200, 204]
        
        # Verify user is deleted
        get_response = self.api.get_user(created_user["id"])
        assert get_response.status_code == 404
    
    def test_delete_user_not_found(self):
        """Test deleting a non-existent user."""
        fake_id = str(uuid.uuid4())
        response = self.api.delete_user(fake_id)
        
        assert response.status_code == 404
    
    # ==================== AUTHENTICATION TESTS ====================
    
    def test_unauthorized_request(self, user_api_no_auth):
        """Test request without authentication."""
        response = user_api_no_auth.get_user("any-id")
        
        assert response.status_code == 401
    
    # ==================== SCHEMA VALIDATION TESTS ====================
    
    def test_user_response_schema(self, created_user, user_schema):
        """Test that user response matches expected schema."""
        response = self.api.get_user(created_user["id"])
        data = response.json()
        
        try:
            validate(instance=data, schema=user_schema)
        except ValidationError as e:
            pytest.fail(f"Schema validation failed: {e.message}")
```

---

## 📱 Mobile Automation (Appium)

**The Prompt:**

```
Act as a Mobile Test Automation Engineer expert in Appium.

Create Appium tests for a mobile login feature.

Platform: Android
App Package: com.example.myapp
App Activity: .MainActivity

Elements (use resource-id):
- Username field: com.example.myapp:id/username_input
- Password field: com.example.myapp:id/password_input
- Login button: com.example.myapp:id/login_button
- Error message: com.example.myapp:id/error_text
- Welcome message: com.example.myapp:id/welcome_text

Requirements:
- Use Python with Appium 2.0
- Page Object Model pattern
- pytest framework
- Run on Android emulator (Pixel 4, API 30)
- Include desired capabilities
- Handle explicit waits
- Take screenshots on failure
- Add gesture support (scroll, swipe)

Include:
1. Appium desired capabilities setup
2. Base mobile page class
3. Login page object
4. Test cases
5. conftest.py with fixtures
6. Requirements.txt
```

**Expected Output - Mobile Page Object:**

```python
"""
Login Page object for mobile app.
"""
from appium.webdriver.common.appiumby import AppiumBy
from pages.base_mobile_page import BaseMobilePage


class LoginPage(BaseMobilePage):
    """Page object for mobile login screen."""
    
    # Locators (Android resource-id)
    USERNAME_FIELD = (AppiumBy.ID, "com.example.myapp:id/username_input")
    PASSWORD_FIELD = (AppiumBy.ID, "com.example.myapp:id/password_input")
    LOGIN_BUTTON = (AppiumBy.ID, "com.example.myapp:id/login_button")
    ERROR_TEXT = (AppiumBy.ID, "com.example.myapp:id/error_text")
    WELCOME_TEXT = (AppiumBy.ID, "com.example.myapp:id/welcome_text")
    
    def enter_username(self, username: str) -> 'LoginPage':
        """Enter username."""
        self.enter_text(self.USERNAME_FIELD, username)
        return self
    
    def enter_password(self, password: str) -> 'LoginPage':
        """Enter password."""
        self.enter_text(self.PASSWORD_FIELD, password)
        return self
    
    def tap_login(self) -> None:
        """Tap login button."""
        self.tap_element(self.LOGIN_BUTTON)
    
    def login(self, username: str, password: str) -> None:
        """Perform complete login."""
        self.enter_username(username)
        self.enter_password(password)
        self.hide_keyboard()  # Hide keyboard before tapping
        self.tap_login()
    
    def is_error_displayed(self) -> bool:
        """Check if error message is displayed."""
        return self.is_element_visible(self.ERROR_TEXT)
    
    def get_error_message(self) -> str:
        """Get error message text."""
        return self.get_element_text(self.ERROR_TEXT)
    
    def is_welcome_displayed(self) -> bool:
        """Check if welcome message is displayed."""
        return self.is_element_visible(self.WELCOME_TEXT)
```

---

## ✅ Best Practices for Code Generation

### DO's ✅

| Practice | Why It Matters |
|----------|----------------|
| **Specify exact versions** | Ensures compatibility |
| **Request error handling** | Makes code robust |
| **Ask for docstrings** | Documents the code |
| **Request type hints** | Improves maintainability |
| **Specify coding standards** | Consistent style |
| **Ask for complete code** | Ready to run |
| **Request example usage** | Easy to understand |

### DON'Ts ❌

| Anti-Pattern | Risk |
|--------------|------|
| **Using without review** | Bugs and security issues |
| **Production without testing** | Runtime failures |
| **Skipping error handling** | Unstable tests |
| **Ignoring security** | Vulnerabilities |
| **Copy without understanding** | Maintenance nightmare |

---

## 🔍 Code Review Checklist

After AI generates code, verify:

```
┌─────────────────────────────────────────────────────────────────┐
│  CODE REVIEW CHECKLIST                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  FUNCTIONALITY                                                   │
│  [ ] Does it run without errors?                                │
│  [ ] Are all imports available?                                 │
│  [ ] Does it do what was requested?                             │
│                                                                  │
│  QUALITY                                                         │
│  [ ] Is error handling present?                                 │
│  [ ] Are there meaningful variable names?                       │
│  [ ] Is the code DRY (Don't Repeat Yourself)?                  │
│                                                                  │
│  SECURITY                                                        │
│  [ ] No hardcoded credentials?                                  │
│  [ ] No sensitive data in logs?                                 │
│  [ ] Proper input validation?                                   │
│                                                                  │
│  MAINTAINABILITY                                                 │
│  [ ] Is it well-documented?                                     │
│  [ ] Does it follow team standards?                             │
│  [ ] Are there unit tests?                                      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 💻 Practical Exercises

### Exercise 4.1: Generate a Page Object

**Task:** Create a page object for a product detail page.

**Elements:**
- Product name: class="product-title"
- Price: class="product-price"
- Add to cart button: id="add-to-cart"
- Quantity input: id="quantity"
- Success message: class="cart-success"

**Write your prompt:**
```
[Your prompt here]
```

---

### Exercise 4.2: Generate API Tests

**Task:** Create API tests for a products endpoint.

**Endpoints:**
- GET /api/products - List products
- GET /api/products/{id} - Get product
- POST /api/products - Create product (admin only)
- PUT /api/products/{id} - Update product
- DELETE /api/products/{id} - Delete product

**Write your prompt:**
```
[Your prompt here]
```

---

### Exercise 4.3: Generate Test Fixtures

**Task:** Create pytest fixtures for:
- WebDriver setup (Chrome, Firefox)
- API client
- Test data generator
- Screenshot on failure

**Write your prompt:**
```
[Your prompt here]
```

---

## 📝 Key Takeaways

```
┌─────────────────────────────────────────────────────────────────┐
│  LESSON 1 KEY TAKEAWAYS                                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✅ AI can generate complete test automation code               │
│                                                                  │
│  ✅ Be specific about framework, language, and version          │
│                                                                  │
│  ✅ Request design patterns (Page Object Model)                 │
│                                                                  │
│  ✅ Always include error handling and logging                   │
│                                                                  │
│  ✅ Review and test ALL generated code                          │
│                                                                  │
│  ✅ Use AI for boilerplate, customize for your needs            │
│                                                                  │
│  ✅ Generate utilities and frameworks, not just tests           │
│                                                                  │
│  ✅ Request documentation and examples                          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ Lesson 1 Checklist

Before moving on, ensure you can:

- [ ] Write prompts to generate Selenium WebDriver tests
- [ ] Generate Page Object Model classes
- [ ] Create data-driven tests with parametrize
- [ ] Generate API test automation code
- [ ] Create mobile test scripts with Appium
- [ ] Review and validate generated code
- [ ] Complete Exercises 4.1-4.3

---

## 🔗 Next Steps

Continue to:
- [Lesson 2: Framework Building](./02-framework-building.md)
- [Lesson 3: CI/CD and Advanced Patterns](./03-cicd-and-advanced-patterns.md)

---

**Great work! Continue to learn about building complete automation frameworks! 🚀**
