# Module 4: Prompt Engineering for Automation Testing

## Lesson 2: Framework Building

---

## 🎯 Lesson Overview

**Time Required:** 60 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Lesson 1 completed

In this lesson, you'll learn:
- How to generate complete test automation frameworks
- Creating Page Object Model structures
- Building test fixtures and utilities
- Generating test data with realistic data
- Creating configuration management systems

---

## 🏗️ Building a Complete Framework

### Framework Architecture

A well-designed test automation framework follows this structure:

```
automation-framework/
│
├── 📁 pages/                    # Page Object classes
│   ├── __init__.py
│   ├── base_page.py            # Common page methods
│   ├── login_page.py
│   ├── home_page.py
│   └── ...
│
├── 📁 api/                      # API client classes
│   ├── __init__.py
│   ├── base_client.py          # Base API client
│   └── user_api.py
│
├── 📁 tests/                    # Test files
│   ├── __init__.py
│   ├── test_login.py
│   ├── test_api.py
│   └── ...
│
├── 📁 utilities/                # Helper functions
│   ├── __init__.py
│   ├── data_generator.py       # Test data generation
│   ├── wait_helpers.py         # Custom waits
│   ├── screenshot_helper.py    # Screenshot utilities
│   └── logger.py               # Logging configuration
│
├── 📁 config/                   # Configuration
│   ├── __init__.py
│   ├── config.py               # Config loader
│   └── environments/
│       ├── dev.json
│       ├── staging.json
│       └── prod.json
│
├── 📁 test_data/                # Test data files
│   ├── users.json
│   └── products.csv
│
├── 📁 reports/                  # Generated reports
├── 📁 screenshots/              # Failure screenshots
│
├── conftest.py                  # Pytest fixtures
├── pytest.ini                   # Pytest config
├── requirements.txt             # Dependencies
├── .env.example                 # Environment template
└── README.md                    # Documentation
```

---

## 📄 Page Object Model Generation

### Generate Complete POM Structure

**Prompt:**

```
Act as a Test Automation Architect expert in Page Object Model design.

Create a complete Page Object Model structure for an e-commerce website.

Pages to create:
1. Base Page - Common methods for all pages
2. Home Page - Hero banner, product categories, search
3. Product Listing Page - Filters, sorting, product cards
4. Product Detail Page - Images, description, add to cart
5. Cart Page - Cart items, quantity, totals
6. Checkout Page - Shipping, payment, order summary

For each page object:
- Define locators as class variables (use tuple format)
- Create methods for ALL user actions
- Include wait strategies
- Add detailed docstrings
- Follow single responsibility principle
- Use method chaining where appropriate

Technology:
- Python 3.9+
- Selenium WebDriver
- pytest

Also include:
1. Locator strategy (CSS selectors preferred)
2. Example test using the page objects
3. conftest.py with driver fixture

Follow these naming conventions:
- Locators: UPPER_CASE
- Methods: snake_case
- Classes: PascalCase
```

**Expected Output - Base Page:**

```python
"""
Base Page class with common functionality for all page objects.
"""
import logging
from typing import Tuple, List, Optional
from selenium.webdriver.remote.webdriver import WebDriver
from selenium.webdriver.remote.webelement import WebElement
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.select import Select
from selenium.webdriver.common.action_chains import ActionChains
from selenium.common.exceptions import TimeoutException, StaleElementReferenceException

logger = logging.getLogger(__name__)


class BasePage:
    """Base class for all page objects."""
    
    def __init__(self, driver: WebDriver, timeout: int = 10):
        """
        Initialize base page.
        
        Args:
            driver: Selenium WebDriver instance
            timeout: Default wait timeout in seconds
        """
        self.driver = driver
        self.timeout = timeout
        self.wait = WebDriverWait(
            driver, 
            timeout,
            ignored_exceptions=[StaleElementReferenceException]
        )
        logger.info(f"Initialized {self.__class__.__name__}")
    
    # ==================== ELEMENT FINDING ====================
    
    def find_element(self, locator: Tuple[str, str]) -> WebElement:
        """Find element with explicit wait."""
        try:
            element = self.wait.until(
                EC.presence_of_element_located(locator)
            )
            logger.debug(f"Found element: {locator}")
            return element
        except TimeoutException:
            logger.error(f"Element not found: {locator}")
            self._capture_debug_screenshot(f"element_not_found")
            raise
    
    def find_elements(self, locator: Tuple[str, str]) -> List[WebElement]:
        """Find all matching elements."""
        self.wait.until(EC.presence_of_element_located(locator))
        return self.driver.find_elements(*locator)
    
    def find_visible_element(self, locator: Tuple[str, str]) -> WebElement:
        """Find element and wait for it to be visible."""
        return self.wait.until(
            EC.visibility_of_element_located(locator)
        )
    
    def find_clickable_element(self, locator: Tuple[str, str]) -> WebElement:
        """Find element and wait for it to be clickable."""
        return self.wait.until(
            EC.element_to_be_clickable(locator)
        )
    
    # ==================== ACTIONS ====================
    
    def click(self, locator: Tuple[str, str]) -> 'BasePage':
        """Click an element."""
        element = self.find_clickable_element(locator)
        element.click()
        logger.info(f"Clicked: {locator}")
        return self
    
    def enter_text(self, locator: Tuple[str, str], text: str, clear: bool = True) -> 'BasePage':
        """Enter text into an input field."""
        element = self.find_element(locator)
        if clear:
            element.clear()
        element.send_keys(text)
        logger.info(f"Entered text into: {locator}")
        return self
    
    def get_text(self, locator: Tuple[str, str]) -> str:
        """Get text from an element."""
        return self.find_element(locator).text
    
    def get_attribute(self, locator: Tuple[str, str], attribute: str) -> str:
        """Get attribute value from an element."""
        return self.find_element(locator).get_attribute(attribute)
    
    def select_by_text(self, locator: Tuple[str, str], text: str) -> 'BasePage':
        """Select dropdown option by visible text."""
        select = Select(self.find_element(locator))
        select.select_by_visible_text(text)
        logger.info(f"Selected '{text}' from: {locator}")
        return self
    
    def select_by_value(self, locator: Tuple[str, str], value: str) -> 'BasePage':
        """Select dropdown option by value."""
        select = Select(self.find_element(locator))
        select.select_by_value(value)
        return self
    
    def hover(self, locator: Tuple[str, str]) -> 'BasePage':
        """Hover over an element."""
        element = self.find_element(locator)
        ActionChains(self.driver).move_to_element(element).perform()
        logger.info(f"Hovered over: {locator}")
        return self
    
    def scroll_to_element(self, locator: Tuple[str, str]) -> 'BasePage':
        """Scroll element into view."""
        element = self.find_element(locator)
        self.driver.execute_script(
            "arguments[0].scrollIntoView({behavior: 'smooth', block: 'center'});",
            element
        )
        return self
    
    # ==================== VERIFICATION ====================
    
    def is_visible(self, locator: Tuple[str, str], timeout: int = None) -> bool:
        """Check if element is visible."""
        try:
            wait = WebDriverWait(self.driver, timeout or self.timeout)
            wait.until(EC.visibility_of_element_located(locator))
            return True
        except TimeoutException:
            return False
    
    def is_present(self, locator: Tuple[str, str], timeout: int = None) -> bool:
        """Check if element is present in DOM."""
        try:
            wait = WebDriverWait(self.driver, timeout or self.timeout)
            wait.until(EC.presence_of_element_located(locator))
            return True
        except TimeoutException:
            return False
    
    def is_clickable(self, locator: Tuple[str, str], timeout: int = None) -> bool:
        """Check if element is clickable."""
        try:
            wait = WebDriverWait(self.driver, timeout or self.timeout)
            wait.until(EC.element_to_be_clickable(locator))
            return True
        except TimeoutException:
            return False
    
    def wait_for_element_to_disappear(self, locator: Tuple[str, str]) -> bool:
        """Wait for element to disappear."""
        return self.wait.until(EC.invisibility_of_element_located(locator))
    
    # ==================== NAVIGATION ====================
    
    def get_current_url(self) -> str:
        """Get current page URL."""
        return self.driver.current_url
    
    def get_page_title(self) -> str:
        """Get page title."""
        return self.driver.title
    
    def wait_for_url_contains(self, text: str) -> bool:
        """Wait for URL to contain specific text."""
        return self.wait.until(EC.url_contains(text))
    
    def refresh(self) -> 'BasePage':
        """Refresh the page."""
        self.driver.refresh()
        return self
    
    # ==================== UTILITIES ====================
    
    def _capture_debug_screenshot(self, name: str) -> None:
        """Capture screenshot for debugging."""
        import os
        os.makedirs("screenshots", exist_ok=True)
        filename = f"screenshots/debug_{name}.png"
        self.driver.save_screenshot(filename)
        logger.info(f"Debug screenshot saved: {filename}")
```

**Expected Output - Product Page:**

```python
"""
Product Detail Page object.
"""
from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webdriver import WebDriver
from pages.base_page import BasePage, logger


class ProductDetailPage(BasePage):
    """Page object for product detail page."""
    
    # ==================== LOCATORS ====================
    
    # Product Information
    PRODUCT_TITLE = (By.CSS_SELECTOR, "h1.product-title")
    PRODUCT_PRICE = (By.CSS_SELECTOR, ".product-price .current-price")
    ORIGINAL_PRICE = (By.CSS_SELECTOR, ".product-price .original-price")
    DISCOUNT_BADGE = (By.CSS_SELECTOR, ".discount-badge")
    PRODUCT_DESCRIPTION = (By.CSS_SELECTOR, ".product-description")
    PRODUCT_SKU = (By.CSS_SELECTOR, ".product-sku")
    
    # Images
    MAIN_IMAGE = (By.CSS_SELECTOR, ".product-image-main img")
    THUMBNAIL_IMAGES = (By.CSS_SELECTOR, ".product-thumbnails img")
    
    # Variants
    SIZE_OPTIONS = (By.CSS_SELECTOR, ".size-selector button")
    COLOR_OPTIONS = (By.CSS_SELECTOR, ".color-selector button")
    SELECTED_SIZE = (By.CSS_SELECTOR, ".size-selector button.selected")
    SELECTED_COLOR = (By.CSS_SELECTOR, ".color-selector button.selected")
    
    # Quantity
    QUANTITY_INPUT = (By.CSS_SELECTOR, "input.quantity-input")
    QUANTITY_INCREASE = (By.CSS_SELECTOR, "button.quantity-plus")
    QUANTITY_DECREASE = (By.CSS_SELECTOR, "button.quantity-minus")
    
    # Actions
    ADD_TO_CART_BUTTON = (By.CSS_SELECTOR, "button.add-to-cart")
    BUY_NOW_BUTTON = (By.CSS_SELECTOR, "button.buy-now")
    ADD_TO_WISHLIST = (By.CSS_SELECTOR, "button.add-wishlist")
    
    # Feedback
    SUCCESS_MESSAGE = (By.CSS_SELECTOR, ".cart-success-message")
    ERROR_MESSAGE = (By.CSS_SELECTOR, ".error-message")
    OUT_OF_STOCK_MESSAGE = (By.CSS_SELECTOR, ".out-of-stock")
    
    # Reviews
    RATING_STARS = (By.CSS_SELECTOR, ".product-rating .stars")
    REVIEW_COUNT = (By.CSS_SELECTOR, ".review-count")
    
    def __init__(self, driver: WebDriver):
        """Initialize product detail page."""
        super().__init__(driver)
    
    # ==================== PRODUCT INFO ====================
    
    def get_product_title(self) -> str:
        """Get product title."""
        return self.get_text(self.PRODUCT_TITLE)
    
    def get_current_price(self) -> float:
        """Get current product price as float."""
        price_text = self.get_text(self.PRODUCT_PRICE)
        # Remove currency symbol and convert to float
        return float(price_text.replace("$", "").replace(",", "").strip())
    
    def get_original_price(self) -> float:
        """Get original price (before discount)."""
        if not self.is_visible(self.ORIGINAL_PRICE, timeout=2):
            return self.get_current_price()
        price_text = self.get_text(self.ORIGINAL_PRICE)
        return float(price_text.replace("$", "").replace(",", "").strip())
    
    def get_discount_percentage(self) -> int:
        """Get discount percentage if available."""
        if not self.is_visible(self.DISCOUNT_BADGE, timeout=2):
            return 0
        badge_text = self.get_text(self.DISCOUNT_BADGE)
        # Extract number from text like "-20%"
        return int(''.join(filter(str.isdigit, badge_text)))
    
    def get_description(self) -> str:
        """Get product description."""
        return self.get_text(self.PRODUCT_DESCRIPTION)
    
    def is_in_stock(self) -> bool:
        """Check if product is in stock."""
        return not self.is_visible(self.OUT_OF_STOCK_MESSAGE, timeout=2)
    
    # ==================== VARIANT SELECTION ====================
    
    def select_size(self, size: str) -> 'ProductDetailPage':
        """
        Select product size.
        
        Args:
            size: Size to select (e.g., "S", "M", "L", "XL")
        """
        size_locator = (By.XPATH, f"//button[contains(@class, 'size-selector')]//span[text()='{size}']/parent::button")
        self.click(size_locator)
        logger.info(f"Selected size: {size}")
        return self
    
    def select_color(self, color: str) -> 'ProductDetailPage':
        """
        Select product color.
        
        Args:
            color: Color name or code
        """
        color_locator = (By.XPATH, f"//button[@data-color='{color}']")
        self.click(color_locator)
        logger.info(f"Selected color: {color}")
        return self
    
    def get_available_sizes(self) -> list:
        """Get list of available sizes."""
        elements = self.find_elements(self.SIZE_OPTIONS)
        return [el.text for el in elements if "disabled" not in el.get_attribute("class")]
    
    def get_available_colors(self) -> list:
        """Get list of available colors."""
        elements = self.find_elements(self.COLOR_OPTIONS)
        return [el.get_attribute("data-color") for el in elements]
    
    # ==================== QUANTITY ====================
    
    def set_quantity(self, quantity: int) -> 'ProductDetailPage':
        """Set product quantity."""
        self.enter_text(self.QUANTITY_INPUT, str(quantity))
        return self
    
    def increase_quantity(self, times: int = 1) -> 'ProductDetailPage':
        """Increase quantity by clicking plus button."""
        for _ in range(times):
            self.click(self.QUANTITY_INCREASE)
        return self
    
    def decrease_quantity(self, times: int = 1) -> 'ProductDetailPage':
        """Decrease quantity by clicking minus button."""
        for _ in range(times):
            self.click(self.QUANTITY_DECREASE)
        return self
    
    def get_quantity(self) -> int:
        """Get current quantity value."""
        value = self.get_attribute(self.QUANTITY_INPUT, "value")
        return int(value)
    
    # ==================== CART ACTIONS ====================
    
    def add_to_cart(self) -> 'ProductDetailPage':
        """Click add to cart button."""
        self.scroll_to_element(self.ADD_TO_CART_BUTTON)
        self.click(self.ADD_TO_CART_BUTTON)
        logger.info("Added product to cart")
        return self
    
    def buy_now(self) -> None:
        """Click buy now button (redirects to checkout)."""
        self.click(self.BUY_NOW_BUTTON)
        logger.info("Clicked Buy Now")
    
    def add_to_wishlist(self) -> 'ProductDetailPage':
        """Add product to wishlist."""
        self.click(self.ADD_TO_WISHLIST)
        logger.info("Added to wishlist")
        return self
    
    # ==================== VERIFICATION ====================
    
    def is_add_to_cart_successful(self) -> bool:
        """Check if add to cart was successful."""
        return self.is_visible(self.SUCCESS_MESSAGE, timeout=5)
    
    def get_success_message(self) -> str:
        """Get success message text."""
        return self.get_text(self.SUCCESS_MESSAGE)
    
    def is_error_displayed(self) -> bool:
        """Check if error message is displayed."""
        return self.is_visible(self.ERROR_MESSAGE, timeout=3)
    
    def get_error_message(self) -> str:
        """Get error message text."""
        return self.get_text(self.ERROR_MESSAGE)
    
    # ==================== COMPLEX ACTIONS ====================
    
    def add_product_with_options(
        self, 
        size: str = None, 
        color: str = None, 
        quantity: int = 1
    ) -> 'ProductDetailPage':
        """
        Add product to cart with specified options.
        
        Args:
            size: Size to select (optional)
            color: Color to select (optional)
            quantity: Quantity to add
            
        Returns:
            Self for method chaining
        """
        if size:
            self.select_size(size)
        if color:
            self.select_color(color)
        if quantity > 1:
            self.set_quantity(quantity)
        
        self.add_to_cart()
        return self
```

---

## 🧪 Test Fixtures Generation

### Generate Comprehensive Fixtures

**Prompt:**

```
Act as a pytest expert specializing in test fixtures.

Create comprehensive pytest fixtures for web automation testing.

Required Fixtures:

1. WebDriver Setup/Teardown:
   - Support Chrome, Firefox, Edge
   - Headless option via command line
   - Configurable window size
   - Screenshot on failure
   - Automatic driver cleanup

2. Test Data Fixtures:
   - Valid user credentials
   - Invalid user data
   - Product test data
   - Dynamic data using Faker

3. Page Object Fixtures:
   - Login page
   - Home page
   - Product page
   - Cart page

4. API Client Fixture:
   - Configured with environment settings
   - Authentication handled

5. Database Fixture:
   - Test database connection
   - Data cleanup after tests

6. Environment Configuration:
   - Load from JSON files
   - Support multiple environments

Include:
- conftest.py with all fixtures
- Fixture scope management (function, class, module, session)
- Fixture parametrization for browser matrix
- Command line options (--browser, --headless, --env)
- Detailed docstrings

Technologies:
- Python 3.9+
- pytest
- selenium
- Faker
```

**Expected Output - conftest.py:**

```python
"""
Pytest configuration and fixtures for test automation.
"""
import os
import json
import pytest
import logging
from datetime import datetime
from typing import Generator
from pathlib import Path

from selenium import webdriver
from selenium.webdriver.chrome.options import Options as ChromeOptions
from selenium.webdriver.firefox.options import Options as FirefoxOptions
from selenium.webdriver.edge.options import Options as EdgeOptions
from selenium.webdriver.chrome.service import Service as ChromeService
from selenium.webdriver.firefox.service import Service as FirefoxService
from webdriver_manager.chrome import ChromeDriverManager
from webdriver_manager.firefox import GeckoDriverManager
from faker import Faker

# Import page objects
from pages.login_page import LoginPage
from pages.home_page import HomePage
from pages.product_page import ProductDetailPage
from pages.cart_page import CartPage
from api.user_api import UserAPI

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

# Initialize Faker
fake = Faker()


# ==================== COMMAND LINE OPTIONS ====================

def pytest_addoption(parser):
    """Add custom command line options."""
    parser.addoption(
        "--browser",
        action="store",
        default="chrome",
        choices=["chrome", "firefox", "edge"],
        help="Browser to run tests: chrome, firefox, or edge"
    )
    parser.addoption(
        "--headless",
        action="store_true",
        default=False,
        help="Run browser in headless mode"
    )
    parser.addoption(
        "--env",
        action="store",
        default="dev",
        choices=["dev", "staging", "prod"],
        help="Environment to run tests against"
    )
    parser.addoption(
        "--window-size",
        action="store",
        default="1920,1080",
        help="Browser window size (width,height)"
    )


# ==================== CONFIGURATION FIXTURES ====================

@pytest.fixture(scope="session")
def config(request) -> dict:
    """
    Load environment configuration.
    
    Returns:
        Configuration dictionary for the selected environment
    """
    env = request.config.getoption("--env")
    config_path = Path(__file__).parent / "config" / "environments" / f"{env}.json"
    
    if not config_path.exists():
        pytest.fail(f"Configuration file not found: {config_path}")
    
    with open(config_path) as f:
        config_data = json.load(f)
    
    logger.info(f"Loaded configuration for environment: {env}")
    return config_data


@pytest.fixture(scope="session")
def base_url(config) -> str:
    """Get base URL from configuration."""
    return config["base_url"]


# ==================== WEBDRIVER FIXTURES ====================

@pytest.fixture(scope="function")
def driver(request, config) -> Generator[webdriver.Remote, None, None]:
    """
    Create and configure WebDriver instance.
    
    Yields:
        Configured WebDriver instance
    """
    browser = request.config.getoption("--browser")
    headless = request.config.getoption("--headless")
    window_size = request.config.getoption("--window-size")
    
    driver = None
    
    try:
        if browser == "chrome":
            options = ChromeOptions()
            if headless:
                options.add_argument("--headless=new")
            options.add_argument("--no-sandbox")
            options.add_argument("--disable-dev-shm-usage")
            options.add_argument(f"--window-size={window_size}")
            options.add_argument("--disable-gpu")
            
            service = ChromeService(ChromeDriverManager().install())
            driver = webdriver.Chrome(service=service, options=options)
            
        elif browser == "firefox":
            options = FirefoxOptions()
            if headless:
                options.add_argument("--headless")
            
            service = FirefoxService(GeckoDriverManager().install())
            driver = webdriver.Firefox(service=service, options=options)
            
        elif browser == "edge":
            options = EdgeOptions()
            if headless:
                options.add_argument("--headless")
            driver = webdriver.Edge(options=options)
        
        if not headless:
            driver.maximize_window()
        
        driver.implicitly_wait(config.get("implicit_wait", 5))
        
        logger.info(f"WebDriver started: {browser} (headless={headless})")
        
        yield driver
        
    finally:
        if driver:
            driver.quit()
            logger.info("WebDriver closed")


@pytest.fixture(params=["chrome", "firefox"])
def multi_browser_driver(request, config) -> Generator[webdriver.Remote, None, None]:
    """
    Parametrized fixture for cross-browser testing.
    
    Yields:
        WebDriver instance for each browser
    """
    browser = request.param
    headless = request.config.getoption("--headless")
    
    if browser == "chrome":
        options = ChromeOptions()
        if headless:
            options.add_argument("--headless=new")
        service = ChromeService(ChromeDriverManager().install())
        driver = webdriver.Chrome(service=service, options=options)
    else:
        options = FirefoxOptions()
        if headless:
            options.add_argument("--headless")
        service = FirefoxService(GeckoDriverManager().install())
        driver = webdriver.Firefox(service=service, options=options)
    
    driver.maximize_window()
    yield driver
    driver.quit()


# ==================== PAGE OBJECT FIXTURES ====================

@pytest.fixture
def login_page(driver, base_url) -> LoginPage:
    """Get login page object."""
    page = LoginPage(driver)
    driver.get(f"{base_url}/login")
    return page


@pytest.fixture
def home_page(driver, base_url) -> HomePage:
    """Get home page object."""
    page = HomePage(driver)
    driver.get(base_url)
    return page


@pytest.fixture
def product_page(driver) -> ProductDetailPage:
    """Get product page object (requires navigation to product)."""
    return ProductDetailPage(driver)


@pytest.fixture
def cart_page(driver) -> CartPage:
    """Get cart page object."""
    return CartPage(driver)


# ==================== TEST DATA FIXTURES ====================

@pytest.fixture
def valid_user() -> dict:
    """Generate valid user test data."""
    return {
        "email": fake.email(),
        "password": "Test@123456",
        "first_name": fake.first_name(),
        "last_name": fake.last_name(),
        "phone": fake.phone_number()
    }


@pytest.fixture
def invalid_users() -> list:
    """Generate various invalid user data for negative testing."""
    return [
        {"email": "invalid-email", "password": "Test@123456", "reason": "invalid_email"},
        {"email": fake.email(), "password": "short", "reason": "password_too_short"},
        {"email": "", "password": "Test@123456", "reason": "empty_email"},
        {"email": fake.email(), "password": "", "reason": "empty_password"},
    ]


@pytest.fixture
def product_data() -> dict:
    """Generate product test data."""
    return {
        "name": fake.catch_phrase(),
        "price": round(fake.pyfloat(min_value=10, max_value=1000), 2),
        "description": fake.paragraph(),
        "category": fake.random_element(["Electronics", "Clothing", "Books", "Home"]),
        "sku": fake.uuid4()[:8].upper()
    }


@pytest.fixture
def bulk_products() -> list:
    """Generate multiple products for testing."""
    return [
        {
            "name": fake.catch_phrase(),
            "price": round(fake.pyfloat(min_value=10, max_value=1000), 2),
            "sku": fake.uuid4()[:8].upper()
        }
        for _ in range(10)
    ]


# ==================== API FIXTURES ====================

@pytest.fixture
def api_client(config) -> UserAPI:
    """Get configured API client."""
    return UserAPI(
        base_url=config["api_base_url"],
        token=config.get("api_token")
    )


@pytest.fixture
def authenticated_api_client(api_client, valid_user) -> UserAPI:
    """Get API client with authenticated user."""
    # Create user and get token
    response = api_client.create_user(valid_user)
    if response.status_code == 201:
        token = response.json().get("token")
        api_client.set_token(token)
    return api_client


# ==================== SCREENSHOT FIXTURES ====================

@pytest.fixture(autouse=True)
def screenshot_on_failure(request, driver):
    """Automatically capture screenshot on test failure."""
    yield
    
    if hasattr(request.node, 'rep_call') and request.node.rep_call.failed:
        # Create screenshots directory
        screenshots_dir = Path("screenshots")
        screenshots_dir.mkdir(exist_ok=True)
        
        # Generate filename
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        test_name = request.node.name.replace("[", "_").replace("]", "")
        filename = screenshots_dir / f"FAILED_{test_name}_{timestamp}.png"
        
        # Capture screenshot
        driver.save_screenshot(str(filename))
        logger.info(f"Screenshot saved: {filename}")


@pytest.hookimpl(tryfirst=True, hookwrapper=True)
def pytest_runtest_makereport(item, call):
    """Make test result available in fixtures."""
    outcome = yield
    rep = outcome.get_result()
    setattr(item, f"rep_{rep.when}", rep)


# ==================== CLEANUP FIXTURES ====================

@pytest.fixture
def cleanup_users(api_client):
    """Track and cleanup created users after test."""
    created_user_ids = []
    
    yield created_user_ids
    
    # Cleanup after test
    for user_id in created_user_ids:
        try:
            api_client.delete_user(user_id)
            logger.info(f"Cleaned up user: {user_id}")
        except Exception as e:
            logger.warning(f"Failed to cleanup user {user_id}: {e}")
```

---

## 🔢 Test Data Generation

### Generate Realistic Test Data

**Prompt:**

```
Act as a Test Automation Engineer specializing in test data management.

Create a comprehensive test data generator utility for an e-commerce application.

Data Types Needed:
1. User data (name, email, phone, address, credentials)
2. Product data (name, price, description, SKU, category)
3. Order data (order ID, items, totals, status)
4. Payment data (card numbers, CVV, expiry - TEST ONLY)
5. Address data (street, city, state, zip, country)

Requirements:
- Use Faker library for realistic data
- Create data in multiple formats (dict, JSON, CSV)
- Support bulk generation
- Include validation methods
- Save to file capability
- Configurable data sets (US, UK, etc.)
- Thread-safe for parallel tests

Include:
1. DataGenerator class with all methods
2. Factory classes for each data type
3. Export utilities (JSON, CSV)
4. Example usage
5. Type hints and docstrings
```

**Expected Output:**

```python
"""
Test Data Generator for E-commerce Application.
"""
import json
import csv
import uuid
import random
import threading
from pathlib import Path
from typing import Dict, List, Optional, Any
from dataclasses import dataclass, asdict
from datetime import datetime, timedelta
from faker import Faker


class DataGenerator:
    """Thread-safe test data generator using Faker."""
    
    _instance = None
    _lock = threading.Lock()
    
    def __new__(cls, locale: str = "en_US"):
        """Singleton pattern for thread safety."""
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
                    cls._instance._initialized = False
        return cls._instance
    
    def __init__(self, locale: str = "en_US"):
        """
        Initialize data generator.
        
        Args:
            locale: Locale for data generation (en_US, en_GB, etc.)
        """
        if self._initialized:
            return
        
        self.fake = Faker(locale)
        self.locale = locale
        self._initialized = True
    
    # ==================== USER DATA ====================
    
    def generate_user(
        self, 
        valid: bool = True,
        with_address: bool = False
    ) -> Dict[str, Any]:
        """
        Generate user data.
        
        Args:
            valid: If True, generates valid data. If False, includes some invalid data.
            with_address: Include address in user data.
            
        Returns:
            Dictionary with user data
        """
        user = {
            "id": str(uuid.uuid4()),
            "email": self.fake.email() if valid else "invalid-email",
            "password": self._generate_password(valid),
            "first_name": self.fake.first_name(),
            "last_name": self.fake.last_name(),
            "phone": self.fake.phone_number(),
            "date_of_birth": self.fake.date_of_birth(minimum_age=18, maximum_age=80).isoformat(),
            "created_at": datetime.now().isoformat()
        }
        
        if with_address:
            user["address"] = self.generate_address()
        
        return user
    
    def generate_users(self, count: int, **kwargs) -> List[Dict[str, Any]]:
        """Generate multiple users."""
        return [self.generate_user(**kwargs) for _ in range(count)]
    
    def _generate_password(self, valid: bool = True) -> str:
        """Generate password."""
        if valid:
            # Password with uppercase, lowercase, number, special char
            return f"{self.fake.password(length=8, special_chars=True, digits=True, upper_case=True)}@1A"
        else:
            return "weak"  # Invalid password
    
    # ==================== PRODUCT DATA ====================
    
    def generate_product(self, category: str = None) -> Dict[str, Any]:
        """
        Generate product data.
        
        Args:
            category: Specific category or random if None
            
        Returns:
            Dictionary with product data
        """
        categories = ["Electronics", "Clothing", "Books", "Home & Garden", "Sports", "Toys"]
        
        return {
            "id": str(uuid.uuid4()),
            "sku": self.fake.uuid4()[:8].upper(),
            "name": self.fake.catch_phrase(),
            "description": self.fake.paragraph(nb_sentences=3),
            "category": category or random.choice(categories),
            "price": round(random.uniform(9.99, 999.99), 2),
            "original_price": round(random.uniform(999.99, 1499.99), 2),
            "quantity": random.randint(0, 100),
            "rating": round(random.uniform(1, 5), 1),
            "review_count": random.randint(0, 500),
            "images": [self.fake.image_url() for _ in range(3)],
            "created_at": self.fake.date_time_this_year().isoformat()
        }
    
    def generate_products(self, count: int, category: str = None) -> List[Dict[str, Any]]:
        """Generate multiple products."""
        return [self.generate_product(category) for _ in range(count)]
    
    # ==================== ORDER DATA ====================
    
    def generate_order(
        self, 
        user_id: str = None,
        item_count: int = None
    ) -> Dict[str, Any]:
        """
        Generate order data.
        
        Args:
            user_id: User ID or generate random
            item_count: Number of items or random 1-5
            
        Returns:
            Dictionary with order data
        """
        items = self._generate_order_items(item_count or random.randint(1, 5))
        subtotal = sum(item["total"] for item in items)
        tax = round(subtotal * 0.08, 2)
        shipping = 9.99 if subtotal < 50 else 0
        
        return {
            "id": str(uuid.uuid4()),
            "order_number": f"ORD-{self.fake.uuid4()[:8].upper()}",
            "user_id": user_id or str(uuid.uuid4()),
            "items": items,
            "subtotal": subtotal,
            "tax": tax,
            "shipping": shipping,
            "total": round(subtotal + tax + shipping, 2),
            "status": random.choice(["pending", "processing", "shipped", "delivered"]),
            "shipping_address": self.generate_address(),
            "billing_address": self.generate_address(),
            "created_at": self.fake.date_time_this_month().isoformat(),
            "updated_at": datetime.now().isoformat()
        }
    
    def _generate_order_items(self, count: int) -> List[Dict[str, Any]]:
        """Generate order line items."""
        items = []
        for _ in range(count):
            quantity = random.randint(1, 3)
            price = round(random.uniform(9.99, 199.99), 2)
            items.append({
                "product_id": str(uuid.uuid4()),
                "product_name": self.fake.catch_phrase(),
                "quantity": quantity,
                "price": price,
                "total": round(price * quantity, 2)
            })
        return items
    
    # ==================== ADDRESS DATA ====================
    
    def generate_address(self, country: str = "US") -> Dict[str, str]:
        """
        Generate address data.
        
        Args:
            country: Country code (US, UK, etc.)
            
        Returns:
            Dictionary with address data
        """
        return {
            "street": self.fake.street_address(),
            "city": self.fake.city(),
            "state": self.fake.state_abbr() if country == "US" else self.fake.state(),
            "zip_code": self.fake.zipcode() if country == "US" else self.fake.postcode(),
            "country": country
        }
    
    # ==================== PAYMENT DATA (TEST ONLY) ====================
    
    def generate_test_card(self, card_type: str = "visa") -> Dict[str, str]:
        """
        Generate TEST credit card data. NOT FOR PRODUCTION USE.
        
        Args:
            card_type: Card type (visa, mastercard, amex)
            
        Returns:
            Dictionary with test card data
        """
        # Test card numbers (standard test numbers)
        test_cards = {
            "visa": "4111111111111111",
            "mastercard": "5555555555554444",
            "amex": "378282246310005",
            "declined": "4000000000000002"
        }
        
        expiry_date = self.fake.date_between(start_date="+1m", end_date="+5y")
        
        return {
            "number": test_cards.get(card_type, test_cards["visa"]),
            "expiry_month": expiry_date.strftime("%m"),
            "expiry_year": expiry_date.strftime("%Y"),
            "cvv": str(random.randint(100, 999)),
            "holder_name": self.fake.name().upper()
        }
    
    # ==================== EXPORT UTILITIES ====================
    
    def export_to_json(
        self, 
        data: List[Dict], 
        filename: str,
        directory: str = "test_data"
    ) -> Path:
        """
        Export data to JSON file.
        
        Args:
            data: List of dictionaries to export
            filename: Output filename
            directory: Output directory
            
        Returns:
            Path to created file
        """
        output_dir = Path(directory)
        output_dir.mkdir(exist_ok=True)
        
        filepath = output_dir / f"{filename}.json"
        
        with open(filepath, 'w') as f:
            json.dump(data, f, indent=2, default=str)
        
        return filepath
    
    def export_to_csv(
        self, 
        data: List[Dict], 
        filename: str,
        directory: str = "test_data"
    ) -> Path:
        """
        Export data to CSV file.
        
        Args:
            data: List of dictionaries to export
            filename: Output filename
            directory: Output directory
            
        Returns:
            Path to created file
        """
        if not data:
            raise ValueError("No data to export")
        
        output_dir = Path(directory)
        output_dir.mkdir(exist_ok=True)
        
        filepath = output_dir / f"{filename}.csv"
        
        # Flatten nested dicts for CSV
        flat_data = [self._flatten_dict(item) for item in data]
        
        with open(filepath, 'w', newline='') as f:
            writer = csv.DictWriter(f, fieldnames=flat_data[0].keys())
            writer.writeheader()
            writer.writerows(flat_data)
        
        return filepath
    
    def _flatten_dict(self, d: Dict, parent_key: str = '', sep: str = '_') -> Dict:
        """Flatten nested dictionary."""
        items = []
        for k, v in d.items():
            new_key = f"{parent_key}{sep}{k}" if parent_key else k
            if isinstance(v, dict):
                items.extend(self._flatten_dict(v, new_key, sep).items())
            elif isinstance(v, list):
                items.append((new_key, json.dumps(v)))
            else:
                items.append((new_key, v))
        return dict(items)


# Example usage
if __name__ == "__main__":
    gen = DataGenerator()
    
    # Generate sample data
    users = gen.generate_users(5, with_address=True)
    products = gen.generate_products(10)
    orders = gen.generate_order()
    
    # Export
    gen.export_to_json(users, "users")
    gen.export_to_csv(products, "products")
    
    print("Test data generated successfully!")
```

---

## 💻 Practical Exercises

### Exercise 4.4: Create Page Objects

**Task:** Generate page objects for a blog application:
- Home page (article list, categories, search)
- Article page (title, content, comments, share)
- Login/Register page
- User profile page

**Write your prompt:**
```
[Your prompt here]
```

### Exercise 4.5: Create Test Data Generator

**Task:** Create a test data generator for:
- Blog posts
- Comments
- User profiles
- Tags/categories

**Write your prompt:**
```
[Your prompt here]
```

---

## 📝 Key Takeaways

```
┌─────────────────────────────────────────────────────────────────┐
│  LESSON 2 KEY TAKEAWAYS                                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✅ Page Object Model improves maintainability                  │
│                                                                  │
│  ✅ Fixtures enable reusable test setup                         │
│                                                                  │
│  ✅ Test data generators create realistic data                  │
│                                                                  │
│  ✅ Configuration management enables multi-environment          │
│                                                                  │
│  ✅ Thread-safe utilities enable parallel testing               │
│                                                                  │
│  ✅ AI can generate complete framework components               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ Lesson 2 Checklist

Before moving on:
- [ ] Can generate Page Object Model structures
- [ ] Can create comprehensive pytest fixtures
- [ ] Can build test data generators
- [ ] Understand fixture scopes and dependencies
- [ ] Completed Exercises 4.4-4.5

---

## 🔗 Next Steps

Continue to:
- [Lesson 3: CI/CD and Advanced Patterns](./03-cicd-and-advanced-patterns.md)

---

**Great work! You're building professional automation frameworks! 🚀**
