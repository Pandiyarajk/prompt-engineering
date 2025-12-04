# Module 4: Prompt Engineering for Automation Testing

## Lesson 1: Test Automation Code Generation

### Introduction

AI can dramatically accelerate test automation by generating code for test scripts, frameworks, and utilities. This lesson teaches you how to prompt AI effectively for automation code.

### The Code Generation Framework

```
REQUIREMENTS → AI PROMPT → CODE → REVIEW → REFINE → INTEGRATE
```

### Basic Automation Code Generation

#### Prompt Template:
```
Act as a Test Automation Engineer expert in [framework/language].

Task: Generate [type of code] for [test scenario]

Technical Requirements:
- Language: [Python/Java/JavaScript]
- Framework: [Selenium/Playwright/Cypress/etc]
- Design Pattern: [Page Object Model/etc]
- Assertions: [pytest/unittest/etc]

Requirements:
[List specific requirements]

Include:
- Complete working code
- Imports and setup
- Error handling
- Comments and docstrings
- Example usage/execution

Follow best practices for:
- Code organization
- Naming conventions
- Maintainability
```

### Selenium WebDriver Test Generation

#### Example 1: Login Test

**Prompt:**
```
Act as a Senior Test Automation Engineer expert in Selenium with Python.

Task: Create a complete Selenium test script for user login

Application Details:
- URL: https://example.com/login
- Username field: id="username"
- Password field: id="password"
- Login button: id="login-button"
- Success indicator: dashboard page with class="user-dashboard"
- Error message: class="error-message"

Requirements:
- Use Page Object Model pattern
- Use pytest framework
- Include setup and teardown
- Add explicit waits
- Handle both success and failure cases
- Include logging
- Use Chrome WebDriver

Include:
1. Base Page class
2. Login Page class
3. Test file with pytest
4. conftest.py for fixtures
5. Requirements.txt

Follow PEP 8 standards and include detailed comments.
```

**Expected Output Structure:**
```
tests/
├── pages/
│   ├── base_page.py
│   └── login_page.py
├── tests/
│   └── test_login.py
├── conftest.py
└── requirements.txt
```

#### Example 2: Data-Driven Test

**Prompt:**
```
Act as a Test Automation Engineer specializing in data-driven testing.

Create a data-driven Selenium test using pytest and parametrize.

Test Scenario: Registration form validation

Form Fields:
- Email (id="email")
- Password (id="password")
- Confirm Password (id="confirm-password")
- Submit button (id="submit")

Test Data (CSV format):
email,password,confirm_password,expected_result
valid@test.com,Pass123!,Pass123!,success
invalid-email,Pass123!,Pass123!,error
valid@test.com,short,short,error
valid@test.com,Pass123!,Different1!,error

Requirements:
- Read test data from CSV file
- Use @pytest.mark.parametrize
- Clear assertions for each case
- Take screenshots on failure
- Generate HTML report

Include:
- Test file
- CSV data file
- Helper functions for screenshot
- pytest.ini configuration
```

### API Test Automation

#### Example 1: REST API Testing

**Prompt:**
```
Act as an API Test Automation Expert using Python requests library.

Create comprehensive API tests for a User Management API.

API Endpoints:
1. POST /api/v1/users - Create user
2. GET /api/v1/users/{id} - Get user
3. PUT /api/v1/users/{id} - Update user
4. DELETE /api/v1/users/{id} - Delete user

Authentication: Bearer token in header

User Schema:
{
  "email": "string",
  "firstName": "string",
  "lastName": "string",
  "age": integer
}

Requirements:
- Use pytest framework
- Test all CRUD operations
- Validate response status codes
- Validate response schema
- Test error scenarios (404, 400, 401)
- Use fixtures for test data
- Clean up test data after tests
- Use environment variables for base URL and token

Include:
1. Test file with all CRUD tests
2. conftest.py with fixtures
3. Helper functions for API calls
4. Schema validation using jsonschema
5. .env.example file
6. requirements.txt

Add detailed docstrings and follow REST API testing best practices.
```

#### Example 2: API Test Framework

**Prompt:**
```
Act as a Test Automation Architect.

Design and implement a scalable API testing framework.

Framework Requirements:
- Support for multiple environments (dev, staging, prod)
- Request/response logging
- Automatic retry on failure
- Response time assertions
- Schema validation
- Test data management
- HTML reporting
- CI/CD ready

Structure:
- Base API client class
- Environment configuration
- Utilities (logging, data generation, assertions)
- Test cases organized by feature
- Fixtures and conftest

Technologies:
- Python 3.9+
- pytest
- requests
- python-dotenv
- pytest-html

Provide:
1. Complete folder structure
2. Base classes and utilities
3. Example test cases
4. Configuration files
5. README with setup instructions
```

### Mobile Automation (Appium)

**Prompt:**
```
Act as a Mobile Test Automation Engineer expert in Appium.

Create Appium tests for a mobile login feature.

Platform: Android
App Package: com.example.app
App Activity: .LoginActivity

Elements:
- Username field: id="username_input"
- Password field: id="password_input"  
- Login button: id="login_button"
- Error text: id="error_text"

Requirements:
- Use Python with Appium
- Page Object Model pattern
- pytest framework
- Test on Android emulator
- Include desired capabilities
- Handle implicit/explicit waits
- Test both portrait and landscape
- Take screenshots on failure

Include:
1. Appium setup code
2. Base page class
3. Login page object
4. Test cases
5. conftest.py with fixtures
6. Requirements.txt

Add error handling and logging.
```

### Framework Components Generation

#### Generate Page Object Model

**Prompt:**
```
Act as a Test Automation Engineer expert in Page Object Model design pattern.

Create a Page Object Model structure for an e-commerce website.

Pages to model:
1. Home Page
2. Product Listing Page
3. Product Detail Page
4. Shopping Cart Page
5. Checkout Page

For each page object:
- Define locators as class variables
- Create methods for user actions
- Include wait strategies
- Add docstrings
- Follow single responsibility principle

Technology:
- Python
- Selenium WebDriver
- pytest

Also create:
1. Base Page class with common methods
2. Locator strategy (separate locators from page objects)
3. Example test using the page objects

Follow best practices for maintainability and reusability.
```

#### Generate Test Fixtures

**Prompt:**
```
Act as a pytest expert specializing in test fixtures.

Create comprehensive pytest fixtures for web automation testing.

Required Fixtures:
1. WebDriver setup/teardown
   - Support Chrome, Firefox, Edge
   - Headless option
   - Configurable via command line

2. Test data fixtures
   - User credentials
   - Test products
   - Dynamic data generation

3. Screenshot fixture
   - Capture on test failure
   - Save with test name and timestamp

4. Database fixture
   - Setup test database
   - Clean up after tests

5. API mock fixture
   - Mock external API calls

Include:
- conftest.py with all fixtures
- Fixture scope management (function, class, module, session)
- Fixture parametrization
- Fixture dependencies
- Command line options
- Example usage in tests

Add detailed comments explaining each fixture.
```

### Test Utilities Generation

#### Generate Test Data

**Prompt:**
```
Act as a Test Automation Engineer specializing in test data management.

Create a test data generator utility for an e-commerce application.

Data Types Needed:
- User data (name, email, phone, address)
- Product data (name, price, category, SKU)
- Order data (order ID, items, total)
- Payment data (card numbers, CVV)

Requirements:
- Use Faker library for realistic data
- Create data in multiple formats (dict, JSON, CSV)
- Support bulk generation
- Data validation
- Save to file option
- Configurable data sets

Include:
1. DataGenerator class
2. Methods for each data type
3. Bulk generation capability
4. Export to file functions
5. Example usage
6. Unit tests for the generator

Ensure generated data is realistic and varied.
```

#### Generate Utility Functions

**Prompt:**
```
Act as a Test Automation Engineer creating reusable utilities.

Create a utility module with commonly needed test automation functions.

Utility Functions Needed:

1. Wait Utilities:
   - wait_for_element
   - wait_for_text
   - wait_for_url
   - wait_until_clickable

2. Action Utilities:
   - safe_click (with retry)
   - safe_send_keys
   - select_dropdown
   - switch_to_frame/window

3. Assertion Utilities:
   - assert_element_text
   - assert_element_attribute
   - assert_url_contains
   - soft_assert (collect failures)

4. Screenshot Utilities:
   - capture_screenshot
   - capture_element_screenshot
   - compare_screenshots

5. Data Utilities:
   - read_json
   - read_csv
   - generate_random_string
   - generate_timestamp

Technology: Python, Selenium

Include:
- Complete utilities.py file
- Error handling for each function
- Detailed docstrings
- Type hints
- Unit tests for utilities
- Usage examples

Follow SOLID principles and make functions reusable.
```

### CI/CD Integration Code

**Prompt:**
```
Act as a DevOps Engineer specializing in test automation CI/CD.

Create CI/CD configuration for running Selenium tests.

Requirements:
- Platform: GitHub Actions
- Run tests on: Ubuntu latest
- Browsers: Chrome headless
- Test framework: pytest
- Generate test report
- Upload artifacts (screenshots, reports)
- Run on: push to main, pull requests
- Parallel execution
- Email notification on failure

Include:
1. .github/workflows/test.yml
2. Docker configuration (optional for consistency)
3. Script to setup test environment
4. Script to run tests locally same as CI

Also create similar configs for:
- Jenkins (Jenkinsfile)
- GitLab CI (.gitlab-ci.yml)

Add comments explaining each step.
```

### Advanced Patterns

#### Behavior-Driven Development (BDD)

**Prompt:**
```
Act as a BDD expert using behave framework in Python.

Create BDD test automation for a login feature.

Feature File:
- User login scenarios in Gherkin syntax
- Positive and negative scenarios
- Background steps
- Scenario outline for data-driven tests

Requirements:
- Use behave framework
- Selenium for web automation
- Page Object Model
- Step definitions in Python
- Environment setup (before/after hooks)
- Generate Allure report

Include:
1. features/login.feature (Gherkin)
2. steps/login_steps.py (step definitions)
3. pages/login_page.py (page object)
4. environment.py (hooks)
5. behave.ini (configuration)

Ensure steps are reusable and maintainable.
```

#### Visual Testing

**Prompt:**
```
Act as a Test Automation Engineer specializing in visual regression testing.

Create visual testing automation using Selenium and image comparison.

Requirements:
- Capture screenshots of key pages
- Compare with baseline images
- Detect visual differences
- Generate diff images highlighting changes
- Report differences with threshold
- Support for different screen sizes
- Ignore dynamic content

Technologies:
- Python
- Selenium
- PIL (Pillow) for image comparison
- pytest

Include:
1. VisualTest class
2. Screenshot capture utility
3. Image comparison function
4. Baseline management
5. Test cases for key pages
6. Configuration for thresholds
7. HTML report showing differences

Add documentation on setting up baselines.
```

### Exercise 4.1: Generate Automation Code

**Task:** Use AI to generate automation code

**Exercise 1: Page Object**
```
Create a page object for a registration form with:
- First name, last name, email, password fields
- Terms checkbox
- Submit button
- Success message
- Error messages

Technology: Python + Selenium
```

Your prompt:
```
[Write your prompt here]
```

**Exercise 2: API Test**
```
Create API tests for a product API:
- GET /products (list all)
- GET /products/{id} (get one)
- POST /products (create)
- PUT /products/{id} (update)
- DELETE /products/{id} (delete)

Include authentication and schema validation.
```

Your prompt:
```
[Write your prompt here]
```

**Exercise 3: Test Fixture**
```
Create pytest fixtures for:
- WebDriver (Chrome, Firefox)
- Test user creation/cleanup
- Screenshot on failure
- Test execution time logging
```

Your prompt:
```
[Write your prompt here]
```

### Best Practices for Code Generation

✅ **DO:**
- Specify exact framework and version
- Request error handling
- Ask for comments and docstrings
- Request type hints
- Specify coding standards (PEP 8)
- Ask for complete, runnable code
- Request example usage

❌ **DON'T:**
- Generate code without reviewing
- Use generated code in production without testing
- Skip error handling
- Ignore security considerations
- Copy code without understanding

### Code Review Checklist

After AI generates code:
- [ ] Does it run without errors?
- [ ] Are all imports available?
- [ ] Is error handling present?
- [ ] Are there security issues?
- [ ] Is it maintainable?
- [ ] Does it follow best practices?
- [ ] Are there unit tests?
- [ ] Is it well-documented?

### Key Takeaways

✅ AI can generate complete test automation code
✅ Be specific about framework, language, version
✅ Request best practices and design patterns
✅ Always review and test generated code
✅ Use AI for boilerplate, customize for your needs
✅ Generate utilities and frameworks, not just tests
✅ Include error handling and logging
✅ Request documentation and examples

### Next Steps
- Move to [Module 5: Prompt Engineering for Developers](../module-5-developers/README.md) (or continue with Module 4 when Lesson 2 is available)
