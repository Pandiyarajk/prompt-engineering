# Module 4: Prompt Engineering for Automation Testing

## Lesson 3: CI/CD and Advanced Patterns

---

## 🎯 Lesson Overview

**Time Required:** 45 minutes  
**Difficulty:** Advanced  
**Prerequisites:** Lessons 1 and 2 completed

In this lesson, you'll learn:
- How to generate CI/CD pipeline configurations
- Implementing Behavior-Driven Development (BDD)
- Building visual regression testing
- Creating parallel test execution
- Generating Docker configurations for testing

---

## ⚙️ CI/CD Pipeline Generation

### GitHub Actions Configuration

**Prompt:**

```
Act as a DevOps Engineer specializing in test automation CI/CD.

Create a comprehensive GitHub Actions workflow for Selenium tests.

Requirements:
- Trigger on: push to main, pull requests
- Run on: Ubuntu latest
- Python version: 3.11
- Browsers: Chrome and Firefox (matrix strategy)
- Headless mode
- Generate pytest HTML report
- Upload test artifacts (reports, screenshots)
- Cache pip dependencies
- Parallel test execution with pytest-xdist
- Slack notification on failure
- Test coverage report

Include:
1. .github/workflows/test.yml - Main workflow
2. .github/workflows/nightly.yml - Nightly full regression
3. Makefile for local execution

Add detailed comments explaining each step.
```

**Expected Output - GitHub Actions Workflow:**

```yaml
# .github/workflows/test.yml
# Automated test execution on push and pull requests

name: Selenium Tests

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  workflow_dispatch:  # Allow manual trigger

# Environment variables available to all jobs
env:
  PYTHON_VERSION: '3.11'
  PYTEST_ARGS: '-v --tb=short'

jobs:
  test:
    runs-on: ubuntu-latest
    
    # Matrix strategy for cross-browser testing
    strategy:
      fail-fast: false  # Continue with other browsers if one fails
      matrix:
        browser: [chrome, firefox]
    
    steps:
      # ==================== SETUP ====================
      
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Set up Python ${{ env.PYTHON_VERSION }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ env.PYTHON_VERSION }}
      
      # Cache pip packages to speed up builds
      - name: Cache pip packages
        uses: actions/cache@v3
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('requirements.txt') }}
          restore-keys: |
            ${{ runner.os }}-pip-
      
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pytest-xdist pytest-html pytest-cov
      
      # ==================== BROWSER SETUP ====================
      
      - name: Setup Chrome
        if: matrix.browser == 'chrome'
        uses: browser-actions/setup-chrome@v1
        with:
          chrome-version: stable
      
      - name: Setup Firefox
        if: matrix.browser == 'firefox'
        uses: browser-actions/setup-firefox@v1
        with:
          firefox-version: latest
      
      # ==================== TEST EXECUTION ====================
      
      - name: Run tests
        run: |
          pytest tests/ \
            ${{ env.PYTEST_ARGS }} \
            --browser=${{ matrix.browser }} \
            --headless \
            -n auto \
            --html=reports/report_${{ matrix.browser }}.html \
            --self-contained-html \
            --cov=. \
            --cov-report=xml \
            --cov-report=html:reports/coverage
        continue-on-error: true
        id: pytest
      
      # ==================== ARTIFACTS ====================
      
      - name: Upload test report
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: test-report-${{ matrix.browser }}
          path: reports/
          retention-days: 30
      
      - name: Upload screenshots
        uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: failure-screenshots-${{ matrix.browser }}
          path: screenshots/
          retention-days: 7
      
      # ==================== COVERAGE ====================
      
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage.xml
          flags: ${{ matrix.browser }}
          name: codecov-${{ matrix.browser }}
      
      # ==================== NOTIFICATIONS ====================
      
      - name: Notify Slack on failure
        if: failure()
        uses: 8398a7/action-slack@v3
        with:
          status: failure
          text: ':x: Tests failed for ${{ matrix.browser }}!'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
      
      # Fail the job if tests failed
      - name: Check test results
        if: steps.pytest.outcome == 'failure'
        run: exit 1

  # ==================== MERGE REPORTS ====================
  
  merge-reports:
    needs: test
    runs-on: ubuntu-latest
    if: always()
    
    steps:
      - name: Download all reports
        uses: actions/download-artifact@v4
        with:
          pattern: test-report-*
          path: all-reports/
          merge-multiple: true
      
      - name: Upload merged reports
        uses: actions/upload-artifact@v4
        with:
          name: all-test-reports
          path: all-reports/
          retention-days: 30
```

**Nightly Regression Workflow:**

```yaml
# .github/workflows/nightly.yml
# Full regression test suite - runs every night

name: Nightly Regression

on:
  schedule:
    - cron: '0 2 * * *'  # Run at 2 AM UTC every day
  workflow_dispatch:

env:
  PYTHON_VERSION: '3.11'

jobs:
  full-regression:
    runs-on: ubuntu-latest
    
    strategy:
      fail-fast: false
      matrix:
        browser: [chrome, firefox, edge]
        os: [ubuntu-latest, windows-latest, macos-latest]
        exclude:
          - browser: edge
            os: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ env.PYTHON_VERSION }}
      
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install pytest-xdist pytest-html allure-pytest
      
      - name: Run full regression
        run: |
          pytest tests/ \
            -v \
            --browser=${{ matrix.browser }} \
            --headless \
            -n 4 \
            --alluredir=allure-results \
            -m "regression"
        timeout-minutes: 60
      
      - name: Generate Allure Report
        uses: simple-elf/allure-report-action@v1.7
        if: always()
        with:
          allure_results: allure-results
          allure_history: allure-history
      
      - name: Deploy Allure Report
        uses: peaceiris/actions-gh-pages@v3
        if: always()
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          publish_dir: allure-history
```

---

### Jenkins Pipeline

**Prompt:**

```
Create a Jenkins Pipeline (Jenkinsfile) for the same test automation.

Requirements:
- Declarative pipeline syntax
- Parameters for browser and environment
- Stage for each phase (setup, test, report)
- Parallel browser testing
- Email notification
- Artifact archival
- Cleanup workspace
```

**Expected Output:**

```groovy
// Jenkinsfile
pipeline {
    agent any
    
    parameters {
        choice(
            name: 'BROWSER',
            choices: ['chrome', 'firefox', 'all'],
            description: 'Browser for testing'
        )
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'staging', 'prod'],
            description: 'Test environment'
        )
        booleanParam(
            name: 'HEADLESS',
            defaultValue: true,
            description: 'Run in headless mode'
        )
    }
    
    environment {
        PYTHON_VERSION = '3.11'
        PYTEST_ARGS = '-v --tb=short'
    }
    
    stages {
        stage('Setup') {
            steps {
                script {
                    echo "Setting up Python environment..."
                    sh '''
                        python3 -m venv venv
                        . venv/bin/activate
                        pip install -r requirements.txt
                        pip install pytest-html pytest-xdist
                    '''
                }
            }
        }
        
        stage('Test') {
            parallel {
                stage('Chrome Tests') {
                    when {
                        expression { 
                            params.BROWSER == 'chrome' || params.BROWSER == 'all' 
                        }
                    }
                    steps {
                        script {
                            runTests('chrome')
                        }
                    }
                }
                stage('Firefox Tests') {
                    when {
                        expression { 
                            params.BROWSER == 'firefox' || params.BROWSER == 'all' 
                        }
                    }
                    steps {
                        script {
                            runTests('firefox')
                        }
                    }
                }
            }
        }
        
        stage('Publish Reports') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: '*.html',
                    reportName: 'Test Report'
                ])
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'reports/**/*', fingerprint: true
            archiveArtifacts artifacts: 'screenshots/**/*', allowEmptyArchive: true
            cleanWs()
        }
        failure {
            emailext(
                subject: "Build Failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                body: "Check console output: ${env.BUILD_URL}",
                to: 'qa-team@example.com'
            )
        }
    }
}

def runTests(String browser) {
    def headlessArg = params.HEADLESS ? '--headless' : ''
    
    sh """
        . venv/bin/activate
        pytest tests/ \
            ${PYTEST_ARGS} \
            --browser=${browser} \
            ${headlessArg} \
            --env=${params.ENVIRONMENT} \
            --html=reports/report_${browser}.html \
            --self-contained-html \
            -n auto
    """
}
```

---

## 🥒 Behavior-Driven Development (BDD)

### Generate BDD Framework

**Prompt:**

```
Act as a BDD expert using Python behave framework.

Create a complete BDD test automation framework for e-commerce checkout.

Feature to test: Complete checkout flow
- Add items to cart
- Apply discount code
- Enter shipping address
- Select payment method
- Place order
- Verify confirmation

Requirements:
- Use behave framework
- Selenium for web automation
- Page Object Model for pages
- Data tables for test data
- Scenario outlines for data-driven tests
- Background for common setup
- Tags for test organization
- Allure reporting

Include:
1. features/checkout.feature - Gherkin scenarios
2. features/steps/checkout_steps.py - Step definitions
3. features/environment.py - Hooks
4. pages/ - Page objects
5. behave.ini - Configuration
6. requirements.txt
```

**Expected Output - Feature File:**

```gherkin
# features/checkout.feature
# Checkout flow test scenarios

@checkout @smoke
Feature: E-commerce Checkout
  As a customer
  I want to complete the checkout process
  So that I can purchase products

  Background:
    Given I am logged in as a registered user
    And I have items in my cart

  # ==================== HAPPY PATH ====================

  @positive @p1
  Scenario: Complete checkout with credit card
    When I proceed to checkout
    And I enter shipping address:
      | field    | value              |
      | street   | 123 Main Street    |
      | city     | New York           |
      | state    | NY                 |
      | zip      | 10001              |
      | country  | United States      |
    And I select "Standard Shipping" as shipping method
    And I enter payment details:
      | field       | value             |
      | card_number | 4111111111111111  |
      | expiry      | 12/25             |
      | cvv         | 123               |
      | name        | John Doe          |
    And I place the order
    Then I should see order confirmation
    And I should receive confirmation email

  @positive @discount
  Scenario: Checkout with discount code
    When I proceed to checkout
    And I apply discount code "SAVE20"
    Then I should see 20% discount applied
    When I complete checkout with valid details
    Then I should see order confirmation with discounted total

  # ==================== DATA-DRIVEN TESTS ====================

  @positive @shipping
  Scenario Outline: Checkout with different shipping methods
    When I proceed to checkout
    And I enter a valid shipping address
    And I select "<shipping_method>" as shipping method
    And I complete payment
    Then I should see order confirmation
    And the order total should include <shipping_cost> shipping

    Examples:
      | shipping_method    | shipping_cost |
      | Standard Shipping  | $5.99         |
      | Express Shipping   | $14.99        |
      | Overnight Shipping | $29.99        |

  @negative @validation
  Scenario Outline: Checkout fails with invalid payment
    When I proceed to checkout
    And I enter a valid shipping address
    And I enter invalid payment details:
      | field       | value          |
      | card_number | <card_number>  |
      | expiry      | <expiry>       |
      | cvv         | <cvv>          |
    And I try to place the order
    Then I should see payment error "<error_message>"

    Examples:
      | card_number      | expiry | cvv | error_message              |
      | 4000000000000002 | 12/25  | 123 | Card was declined          |
      | 1234567890123456 | 12/25  | 123 | Invalid card number        |
      | 4111111111111111 | 01/20  | 123 | Card has expired           |
      | 4111111111111111 | 12/25  | 12  | Invalid security code      |

  # ==================== EDGE CASES ====================

  @edge @cart
  Scenario: Checkout with maximum cart items
    Given I have 50 items in my cart
    When I proceed to checkout
    And I complete checkout with valid details
    Then I should see order confirmation for 50 items

  @edge @session
  Scenario: Checkout with session timeout
    When I proceed to checkout
    And I wait for session to almost expire
    And I refresh the page
    Then I should still see my cart items
    And I should be able to complete checkout
```

**Expected Output - Step Definitions:**

```python
# features/steps/checkout_steps.py
"""Step definitions for checkout feature."""

from behave import given, when, then, step
from selenium.webdriver.common.by import By
from pages.login_page import LoginPage
from pages.cart_page import CartPage
from pages.checkout_page import CheckoutPage
from pages.confirmation_page import ConfirmationPage


# ==================== GIVEN STEPS ====================

@given('I am logged in as a registered user')
def step_login(context):
    """Login with test user."""
    login_page = LoginPage(context.driver)
    login_page.navigate()
    login_page.login(
        context.config.userdata.get('test_email'),
        context.config.userdata.get('test_password')
    )
    assert login_page.is_logged_in(), "Login failed"


@given('I have items in my cart')
def step_add_items_to_cart(context):
    """Ensure cart has items."""
    cart_page = CartPage(context.driver)
    
    # If cart is empty, add a test product
    if cart_page.is_cart_empty():
        # Navigate to a product and add it
        context.driver.get(context.config.userdata.get('test_product_url'))
        context.driver.find_element(By.ID, "add-to-cart").click()
    
    context.cart_page = cart_page


@given('I have {count:d} items in my cart')
def step_add_specific_items(context, count):
    """Add specific number of items to cart."""
    cart_page = CartPage(context.driver)
    
    # Clear cart first
    cart_page.clear_cart()
    
    # Add items
    for i in range(count):
        context.driver.get(f"{context.config.userdata.get('base_url')}/products/{i+1}")
        context.driver.find_element(By.ID, "add-to-cart").click()
    
    assert cart_page.get_item_count() == count


# ==================== WHEN STEPS ====================

@when('I proceed to checkout')
def step_proceed_to_checkout(context):
    """Navigate to checkout page."""
    context.cart_page = CartPage(context.driver)
    context.cart_page.proceed_to_checkout()
    context.checkout_page = CheckoutPage(context.driver)


@when('I enter shipping address')
def step_enter_shipping_address_table(context):
    """Enter shipping address from data table."""
    address = {}
    for row in context.table:
        address[row['field']] = row['value']
    
    context.checkout_page.enter_shipping_address(
        street=address.get('street'),
        city=address.get('city'),
        state=address.get('state'),
        zip_code=address.get('zip'),
        country=address.get('country')
    )


@when('I enter a valid shipping address')
def step_enter_valid_address(context):
    """Enter a valid test shipping address."""
    context.checkout_page.enter_shipping_address(
        street="123 Test Street",
        city="Test City",
        state="TS",
        zip_code="12345",
        country="United States"
    )


@when('I select "{shipping_method}" as shipping method')
def step_select_shipping(context, shipping_method):
    """Select shipping method."""
    context.checkout_page.select_shipping_method(shipping_method)


@when('I enter payment details')
def step_enter_payment_table(context):
    """Enter payment details from data table."""
    payment = {}
    for row in context.table:
        payment[row['field']] = row['value']
    
    context.checkout_page.enter_payment_details(
        card_number=payment.get('card_number'),
        expiry=payment.get('expiry'),
        cvv=payment.get('cvv'),
        name=payment.get('name')
    )


@when('I enter invalid payment details')
def step_enter_invalid_payment(context):
    """Enter invalid payment details from table."""
    payment = {}
    for row in context.table:
        payment[row['field']] = row['value']
    
    context.checkout_page.enter_payment_details(
        card_number=payment.get('card_number'),
        expiry=payment.get('expiry'),
        cvv=payment.get('cvv'),
        name="Test User"
    )


@when('I apply discount code "{code}"')
def step_apply_discount(context, code):
    """Apply discount code."""
    context.checkout_page.apply_discount_code(code)


@when('I place the order')
def step_place_order(context):
    """Place the order."""
    context.checkout_page.place_order()


@when('I try to place the order')
def step_try_place_order(context):
    """Attempt to place order (may fail)."""
    context.checkout_page.place_order()


@when('I complete checkout with valid details')
def step_complete_checkout(context):
    """Complete checkout with valid test data."""
    context.execute_steps('''
        When I enter a valid shipping address
        And I select "Standard Shipping" as shipping method
        And I enter payment details:
            | field       | value             |
            | card_number | 4111111111111111  |
            | expiry      | 12/25             |
            | cvv         | 123               |
            | name        | Test User         |
        And I place the order
    ''')


@when('I complete payment')
def step_complete_payment(context):
    """Enter valid payment and place order."""
    context.checkout_page.enter_payment_details(
        card_number="4111111111111111",
        expiry="12/25",
        cvv="123",
        name="Test User"
    )
    context.checkout_page.place_order()


# ==================== THEN STEPS ====================

@then('I should see order confirmation')
def step_verify_confirmation(context):
    """Verify order confirmation page."""
    confirmation = ConfirmationPage(context.driver)
    assert confirmation.is_displayed(), "Confirmation page not displayed"
    context.order_number = confirmation.get_order_number()


@then('I should see order confirmation for {count:d} items')
def step_verify_confirmation_items(context, count):
    """Verify confirmation shows correct item count."""
    confirmation = ConfirmationPage(context.driver)
    assert confirmation.is_displayed()
    assert confirmation.get_item_count() == count


@then('I should receive confirmation email')
def step_verify_email(context):
    """Verify confirmation email (mock/check email service)."""
    # This would typically check an email testing service
    # For now, we verify the "email sent" message on the page
    confirmation = ConfirmationPage(context.driver)
    assert confirmation.is_email_confirmation_shown()


@then('I should see {discount:d}% discount applied')
def step_verify_discount(context, discount):
    """Verify discount is applied."""
    actual_discount = context.checkout_page.get_discount_percentage()
    assert actual_discount == discount, \
        f"Expected {discount}% discount, got {actual_discount}%"


@then('I should see order confirmation with discounted total')
def step_verify_discounted_confirmation(context):
    """Verify confirmation shows discounted total."""
    confirmation = ConfirmationPage(context.driver)
    assert confirmation.is_displayed()
    assert confirmation.get_discount_amount() > 0


@then('the order total should include {shipping_cost} shipping')
def step_verify_shipping_cost(context, shipping_cost):
    """Verify shipping cost in order."""
    confirmation = ConfirmationPage(context.driver)
    # Remove $ and convert to float
    expected = float(shipping_cost.replace('$', ''))
    actual = confirmation.get_shipping_cost()
    assert actual == expected, \
        f"Expected shipping {expected}, got {actual}"


@then('I should see payment error "{error_message}"')
def step_verify_payment_error(context, error_message):
    """Verify payment error message."""
    assert context.checkout_page.is_error_displayed(), \
        "Error message not displayed"
    actual_message = context.checkout_page.get_error_message()
    assert error_message in actual_message, \
        f"Expected '{error_message}' in '{actual_message}'"


@then('I should still see my cart items')
def step_verify_cart_persisted(context):
    """Verify cart items are still present."""
    cart = CartPage(context.driver)
    assert not cart.is_cart_empty(), "Cart is empty after refresh"


@then('I should be able to complete checkout')
def step_can_complete_checkout(context):
    """Verify checkout can be completed."""
    context.execute_steps('''
        When I complete checkout with valid details
        Then I should see order confirmation
    ''')
```

---

## 🖼️ Visual Regression Testing

### Generate Visual Testing Framework

**Prompt:**

```
Act as a Test Automation Engineer specializing in visual regression testing.

Create visual testing automation using Selenium and image comparison.

Requirements:
- Capture screenshots of key pages
- Compare with baseline images
- Detect visual differences with configurable threshold
- Generate diff images highlighting changes
- Support multiple viewport sizes
- Ignore dynamic content (timestamps, ads)
- Generate HTML report with visual comparison

Technologies:
- Python 3.9+
- Selenium
- Pillow (PIL) for image comparison
- pytest

Include:
1. VisualTest class with all methods
2. Screenshot capture with viewport support
3. Image comparison with threshold
4. Diff image generation
5. Baseline management (create/update)
6. HTML report generation
7. Configuration for regions to ignore
```

**Expected Output:**

```python
"""
Visual Regression Testing Framework.
"""
import os
import json
from pathlib import Path
from typing import Tuple, List, Optional, Dict
from dataclasses import dataclass
from datetime import datetime
from PIL import Image, ImageChops, ImageDraw
from selenium.webdriver.remote.webdriver import WebDriver
import logging

logger = logging.getLogger(__name__)


@dataclass
class ComparisonResult:
    """Result of visual comparison."""
    matched: bool
    diff_percentage: float
    baseline_path: str
    actual_path: str
    diff_path: Optional[str] = None
    message: str = ""


class VisualTester:
    """Visual regression testing utilities."""
    
    def __init__(
        self,
        driver: WebDriver,
        baseline_dir: str = "visual_baselines",
        output_dir: str = "visual_reports",
        threshold: float = 0.1  # 0.1% difference threshold
    ):
        """
        Initialize visual tester.
        
        Args:
            driver: Selenium WebDriver instance
            baseline_dir: Directory for baseline images
            output_dir: Directory for test outputs
            threshold: Maximum allowed difference percentage
        """
        self.driver = driver
        self.baseline_dir = Path(baseline_dir)
        self.output_dir = Path(output_dir)
        self.threshold = threshold
        
        # Create directories
        self.baseline_dir.mkdir(parents=True, exist_ok=True)
        self.output_dir.mkdir(parents=True, exist_ok=True)
        
        self.results: List[ComparisonResult] = []
    
    def capture_screenshot(
        self,
        name: str,
        viewport: Tuple[int, int] = None,
        full_page: bool = False
    ) -> Path:
        """
        Capture screenshot.
        
        Args:
            name: Screenshot name
            viewport: Optional viewport size (width, height)
            full_page: Capture full page (scroll)
            
        Returns:
            Path to saved screenshot
        """
        # Set viewport if specified
        if viewport:
            self.driver.set_window_size(viewport[0], viewport[1])
        
        # Create output path
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        filename = f"{name}_{timestamp}.png"
        filepath = self.output_dir / "actual" / filename
        filepath.parent.mkdir(parents=True, exist_ok=True)
        
        if full_page:
            self._capture_full_page(filepath)
        else:
            self.driver.save_screenshot(str(filepath))
        
        logger.info(f"Screenshot saved: {filepath}")
        return filepath
    
    def _capture_full_page(self, filepath: Path) -> None:
        """Capture full page screenshot by scrolling."""
        # Get page dimensions
        total_height = self.driver.execute_script(
            "return document.body.scrollHeight"
        )
        viewport_height = self.driver.execute_script(
            "return window.innerHeight"
        )
        viewport_width = self.driver.execute_script(
            "return window.innerWidth"
        )
        
        # Capture and stitch screenshots
        screenshots = []
        offset = 0
        
        while offset < total_height:
            self.driver.execute_script(f"window.scrollTo(0, {offset});")
            
            # Wait for scroll
            import time
            time.sleep(0.2)
            
            # Capture viewport
            screenshot = self.driver.get_screenshot_as_png()
            screenshots.append(Image.open(io.BytesIO(screenshot)))
            
            offset += viewport_height
        
        # Stitch images
        full_image = Image.new('RGB', (viewport_width, total_height))
        y_offset = 0
        
        for img in screenshots[:-1]:
            full_image.paste(img, (0, y_offset))
            y_offset += viewport_height
        
        # Handle last screenshot (may be partial)
        if screenshots:
            last_height = total_height - (len(screenshots) - 1) * viewport_height
            last_img = screenshots[-1].crop((0, viewport_height - last_height, viewport_width, viewport_height))
            full_image.paste(last_img, (0, y_offset))
        
        full_image.save(str(filepath))
    
    def compare_with_baseline(
        self,
        name: str,
        actual_path: Path = None,
        ignore_regions: List[Tuple[int, int, int, int]] = None,
        update_baseline: bool = False
    ) -> ComparisonResult:
        """
        Compare screenshot with baseline.
        
        Args:
            name: Baseline name
            actual_path: Path to actual screenshot (or capture new)
            ignore_regions: List of (x, y, width, height) regions to ignore
            update_baseline: If True, update baseline with actual
            
        Returns:
            ComparisonResult with match status
        """
        baseline_path = self.baseline_dir / f"{name}.png"
        
        # Capture if not provided
        if actual_path is None:
            actual_path = self.capture_screenshot(name)
        
        # Create baseline if doesn't exist
        if not baseline_path.exists():
            if update_baseline:
                self._copy_as_baseline(actual_path, baseline_path)
                return ComparisonResult(
                    matched=True,
                    diff_percentage=0.0,
                    baseline_path=str(baseline_path),
                    actual_path=str(actual_path),
                    message="Baseline created"
                )
            else:
                return ComparisonResult(
                    matched=False,
                    diff_percentage=100.0,
                    baseline_path=str(baseline_path),
                    actual_path=str(actual_path),
                    message="No baseline exists"
                )
        
        # Load images
        baseline_img = Image.open(baseline_path)
        actual_img = Image.open(actual_path)
        
        # Resize if dimensions differ
        if baseline_img.size != actual_img.size:
            actual_img = actual_img.resize(baseline_img.size)
        
        # Apply ignore regions
        if ignore_regions:
            baseline_img = self._mask_regions(baseline_img, ignore_regions)
            actual_img = self._mask_regions(actual_img, ignore_regions)
        
        # Compare images
        diff_percentage, diff_img = self._compare_images(baseline_img, actual_img)
        
        # Save diff if differences found
        diff_path = None
        if diff_percentage > 0:
            diff_path = self.output_dir / "diffs" / f"{name}_diff.png"
            diff_path.parent.mkdir(parents=True, exist_ok=True)
            diff_img.save(str(diff_path))
        
        matched = diff_percentage <= self.threshold
        
        result = ComparisonResult(
            matched=matched,
            diff_percentage=diff_percentage,
            baseline_path=str(baseline_path),
            actual_path=str(actual_path),
            diff_path=str(diff_path) if diff_path else None,
            message=f"Difference: {diff_percentage:.2f}%"
        )
        
        self.results.append(result)
        
        if update_baseline and not matched:
            self._copy_as_baseline(actual_path, baseline_path)
            result.message += " (baseline updated)"
        
        return result
    
    def _compare_images(
        self,
        baseline: Image.Image,
        actual: Image.Image
    ) -> Tuple[float, Image.Image]:
        """
        Compare two images and generate diff.
        
        Returns:
            Tuple of (difference percentage, diff image)
        """
        # Calculate difference
        diff = ImageChops.difference(baseline, actual)
        
        # Convert to grayscale for comparison
        diff_gray = diff.convert('L')
        
        # Count different pixels
        pixels = list(diff_gray.getdata())
        diff_pixels = sum(1 for p in pixels if p > 10)  # Threshold for noise
        total_pixels = len(pixels)
        
        diff_percentage = (diff_pixels / total_pixels) * 100
        
        # Create highlighted diff image
        diff_highlighted = actual.copy()
        draw = ImageDraw.Draw(diff_highlighted)
        
        # Highlight different regions in red
        diff_rgb = diff.convert('RGB')
        for x in range(diff.width):
            for y in range(diff.height):
                r, g, b = diff_rgb.getpixel((x, y))
                if r > 10 or g > 10 or b > 10:
                    draw.point((x, y), fill=(255, 0, 0))
        
        return diff_percentage, diff_highlighted
    
    def _mask_regions(
        self,
        image: Image.Image,
        regions: List[Tuple[int, int, int, int]]
    ) -> Image.Image:
        """Mask out regions to ignore."""
        masked = image.copy()
        draw = ImageDraw.Draw(masked)
        
        for x, y, width, height in regions:
            draw.rectangle(
                [x, y, x + width, y + height],
                fill=(128, 128, 128)  # Gray mask
            )
        
        return masked
    
    def _copy_as_baseline(self, source: Path, dest: Path) -> None:
        """Copy file as new baseline."""
        import shutil
        dest.parent.mkdir(parents=True, exist_ok=True)
        shutil.copy(source, dest)
        logger.info(f"Baseline updated: {dest}")
    
    def generate_report(self) -> Path:
        """
        Generate HTML report of visual comparisons.
        
        Returns:
            Path to generated report
        """
        report_path = self.output_dir / "visual_report.html"
        
        html = """
        <!DOCTYPE html>
        <html>
        <head>
            <title>Visual Regression Report</title>
            <style>
                body { font-family: Arial, sans-serif; margin: 20px; }
                .result { margin: 20px 0; padding: 15px; border: 1px solid #ddd; }
                .passed { border-left: 5px solid #28a745; }
                .failed { border-left: 5px solid #dc3545; }
                .images { display: flex; gap: 20px; flex-wrap: wrap; }
                .image-container { text-align: center; }
                .image-container img { max-width: 400px; border: 1px solid #ddd; }
                h1 { color: #333; }
                .stats { background: #f5f5f5; padding: 10px; margin-bottom: 20px; }
            </style>
        </head>
        <body>
            <h1>Visual Regression Report</h1>
            <div class="stats">
                <strong>Total: {total}</strong> | 
                <span style="color: green">Passed: {passed}</span> | 
                <span style="color: red">Failed: {failed}</span>
            </div>
        """.format(
            total=len(self.results),
            passed=sum(1 for r in self.results if r.matched),
            failed=sum(1 for r in self.results if not r.matched)
        )
        
        for result in self.results:
            status_class = "passed" if result.matched else "failed"
            status_text = "PASSED" if result.matched else "FAILED"
            
            html += f"""
            <div class="result {status_class}">
                <h3>{status_text} - {result.message}</h3>
                <p>Difference: {result.diff_percentage:.2f}%</p>
                <div class="images">
                    <div class="image-container">
                        <h4>Baseline</h4>
                        <img src="{result.baseline_path}" alt="Baseline">
                    </div>
                    <div class="image-container">
                        <h4>Actual</h4>
                        <img src="{result.actual_path}" alt="Actual">
                    </div>
            """
            
            if result.diff_path:
                html += f"""
                    <div class="image-container">
                        <h4>Difference</h4>
                        <img src="{result.diff_path}" alt="Diff">
                    </div>
                """
            
            html += """
                </div>
            </div>
            """
        
        html += "</body></html>"
        
        with open(report_path, 'w') as f:
            f.write(html)
        
        logger.info(f"Visual report generated: {report_path}")
        return report_path
```

---

## 💻 Practical Exercises

### Exercise 4.6: Create CI/CD Pipeline

**Task:** Generate a GitLab CI configuration for your test automation with:
- Multi-stage pipeline
- Parallel browser testing
- Artifact management
- Deployment of test reports

**Write your prompt:**
```
[Your prompt here]
```

### Exercise 4.7: Create BDD Feature

**Task:** Create a BDD feature file and step definitions for user registration with email verification.

**Write your prompt:**
```
[Your prompt here]
```

---

## 📝 Key Takeaways

```
┌─────────────────────────────────────────────────────────────────┐
│  LESSON 3 KEY TAKEAWAYS                                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✅ CI/CD pipelines automate test execution                     │
│                                                                  │
│  ✅ Matrix strategies enable cross-browser testing              │
│                                                                  │
│  ✅ BDD bridges gap between business and technical              │
│                                                                  │
│  ✅ Visual testing catches UI regressions                       │
│                                                                  │
│  ✅ AI can generate complete pipeline configurations            │
│                                                                  │
│  ✅ Parallel execution speeds up test suites                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ Module 4 Complete! 🎉

Congratulations! You've mastered AI-powered test automation!

**You can now:**
- ✅ Generate complete test automation code
- ✅ Build Page Object Model frameworks
- ✅ Create API test automation
- ✅ Generate mobile tests with Appium
- ✅ Build test fixtures and utilities
- ✅ Create CI/CD pipeline configurations
- ✅ Implement BDD with behave
- ✅ Build visual regression testing

---

## 🔗 Next Steps

| Your Interest | Next Module |
|---------------|-------------|
| **Application code** | [Module 5: Developers](../module-5-developers/README.md) |
| **Advanced prompting** | [Module 6: Advanced Techniques](../module-6-advanced-techniques/README.md) |
| **Hands-on project** | [Project 1: Test Suite Generator](../../projects/project-1-test-suite-generator/) |

---

**Outstanding work completing Module 4! 🚀**
