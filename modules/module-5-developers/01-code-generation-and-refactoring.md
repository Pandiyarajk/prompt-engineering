# Module 5: Prompt Engineering for Developers

## Lesson 1: Code Generation and Refactoring

---

## 🎯 Lesson Overview

**Time Required:** 90 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Modules 1-2 completed, programming experience

In this lesson, you'll learn:
- How to generate production-quality functions and classes
- Techniques for effective code refactoring
- Best practices for reviewing generated code
- How to ensure code quality and maintainability

---

## 📚 Introduction

AI can significantly accelerate development by generating code, refactoring existing code, and suggesting improvements. However, the quality of generated code depends heavily on how well you craft your prompts.

### The Code Generation Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│  AI-ASSISTED CODE GENERATION WORKFLOW                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│    DEFINE           CRAFT           GENERATE                     │
│  ┌─────────┐     ┌─────────┐     ┌─────────┐                   │
│  │ REQUIRE-│ →   │ DETAILED│ →   │  CODE   │                   │
│  │  MENTS  │     │ PROMPT  │     │ OUTPUT  │                   │
│  └─────────┘     └─────────┘     └─────────┘                   │
│       │                               │                         │
│       │                               ▼                         │
│       │         ┌─────────┐     ┌─────────┐                    │
│       │         │ ITERATE │ ←   │ REVIEW  │                    │
│       │         │ REFINE  │     │  TEST   │                    │
│       │         └────┬────┘     └─────────┘                    │
│       │              │                                          │
│       ▼              ▼                                          │
│  ┌─────────────────────────────┐                               │
│  │      PRODUCTION CODE        │                               │
│  │    (tested & documented)    │                               │
│  └─────────────────────────────┘                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📝 The Code Generation Template

This template works for generating any type of code:

```
Act as a Senior [Language] Developer with expertise in [domain].

Task: Create [component type] for [purpose]

Requirements:
- [Functional requirement 1]
- [Functional requirement 2]
- [etc.]

Technical Specifications:
- Language: [Language + version]
- Framework: [Framework if applicable]
- Design patterns: [Patterns to use]
- Coding standards: [Style guide]

Include:
- Complete working code
- Error handling
- Input validation
- Documentation (docstrings/comments)
- Type hints/annotations
- Unit tests
- Example usage

Optimize for:
- [Performance/Readability/Maintainability]

Follow [specific standards/guidelines].
```

### Understanding Each Section

| Section | Purpose | Example |
|---------|---------|---------|
| **Role** | Sets expertise level | "Senior Python Developer" |
| **Task** | What to create | "utility function for email validation" |
| **Requirements** | Functional needs | "Check format, verify domain" |
| **Tech Specs** | Technical constraints | "Python 3.11, type hints" |
| **Include** | Code elements | "Error handling, tests, docs" |
| **Optimize for** | Quality attributes | "Performance, readability" |

---

## 🔧 Function Generation

### Example 1: Utility Function with Full Documentation

**Prompt:**

```
Act as a Senior Python Developer.

Create a utility function to validate and parse phone numbers.

Requirements:
- Accept phone numbers in various formats (e.g., +1-234-567-8900, (234) 567-8900)
- Support international formats
- Extract country code, area code, and local number
- Return structured result with validation status
- Handle edge cases (too short, too long, invalid characters)

Technical Specs:
- Python 3.11+
- Use regex for pattern matching
- Type hints required
- No external dependencies (stdlib only)

Include:
- Main parsing function
- Helper functions if needed
- Comprehensive docstring with examples
- Custom exception class
- Unit tests using pytest
- Example usage

Follow PEP 8 and Google docstring style.
```

**Expected Output:**

```python
"""
Phone Number Parser and Validator.

This module provides utilities for parsing and validating phone numbers
in various international formats.
"""
import re
from dataclasses import dataclass
from typing import Optional
from enum import Enum


class PhoneNumberError(Exception):
    """Base exception for phone number parsing errors."""
    pass


class InvalidFormatError(PhoneNumberError):
    """Raised when phone number format is invalid."""
    pass


class InvalidLengthError(PhoneNumberError):
    """Raised when phone number length is invalid."""
    pass


class PhoneNumberType(Enum):
    """Type of phone number."""
    MOBILE = "mobile"
    LANDLINE = "landline"
    TOLL_FREE = "toll_free"
    UNKNOWN = "unknown"


@dataclass
class ParsedPhoneNumber:
    """
    Structured representation of a parsed phone number.
    
    Attributes:
        original: The original input string.
        country_code: International country code (e.g., "1" for US).
        area_code: Area/region code.
        local_number: Local subscriber number.
        formatted: Standardized formatted number.
        is_valid: Whether the number is valid.
        phone_type: Type of phone number.
    """
    original: str
    country_code: Optional[str]
    area_code: Optional[str]
    local_number: str
    formatted: str
    is_valid: bool
    phone_type: PhoneNumberType = PhoneNumberType.UNKNOWN


# Common country codes and their expected lengths
COUNTRY_CODES = {
    "1": {"name": "US/Canada", "lengths": [10]},
    "44": {"name": "UK", "lengths": [10, 11]},
    "49": {"name": "Germany", "lengths": [10, 11]},
    "33": {"name": "France", "lengths": [9]},
    "81": {"name": "Japan", "lengths": [10]},
}


def clean_phone_number(phone: str) -> str:
    """
    Remove all non-digit characters except leading +.
    
    Args:
        phone: Raw phone number string.
        
    Returns:
        Cleaned string containing only digits (and optional leading +).
        
    Examples:
        >>> clean_phone_number("+1 (234) 567-8900")
        '+12345678900'
        >>> clean_phone_number("234.567.8900")
        '2345678900'
    """
    if not phone:
        return ""
    
    # Preserve leading + for international format
    has_plus = phone.strip().startswith('+')
    
    # Remove all non-digits
    cleaned = re.sub(r'[^\d]', '', phone)
    
    return f"+{cleaned}" if has_plus else cleaned


def extract_country_code(cleaned_number: str) -> tuple[Optional[str], str]:
    """
    Extract country code from cleaned phone number.
    
    Args:
        cleaned_number: Phone number with only digits (and optional +).
        
    Returns:
        Tuple of (country_code, remaining_number).
        
    Examples:
        >>> extract_country_code("+12345678900")
        ('1', '2345678900')
        >>> extract_country_code("2345678900")
        (None, '2345678900')
    """
    if not cleaned_number:
        return None, ""
    
    # Remove + prefix if present
    number = cleaned_number.lstrip('+')
    
    if cleaned_number.startswith('+'):
        # Try matching known country codes (longest first)
        for code in sorted(COUNTRY_CODES.keys(), key=len, reverse=True):
            if number.startswith(code):
                return code, number[len(code):]
        
        # Default: assume 1-3 digit country code
        if len(number) > 10:
            return number[:1], number[1:]
    
    return None, number


def parse_phone_number(phone: str) -> ParsedPhoneNumber:
    """
    Parse and validate a phone number string.
    
    Accepts phone numbers in various formats and returns a structured
    ParsedPhoneNumber object with extracted components.
    
    Args:
        phone: Phone number string in any common format.
        
    Returns:
        ParsedPhoneNumber object with parsed components.
        
    Raises:
        InvalidFormatError: If the input contains invalid characters.
        InvalidLengthError: If the phone number length is invalid.
        
    Examples:
        >>> result = parse_phone_number("+1 (234) 567-8900")
        >>> result.country_code
        '1'
        >>> result.formatted
        '+1 (234) 567-8900'
        >>> result.is_valid
        True
        
        >>> result = parse_phone_number("234-567-8900")
        >>> result.area_code
        '234'
        >>> result.local_number
        '5678900'
    
    Note:
        - US/Canada numbers are assumed if no country code is provided
        - Invalid numbers still return a ParsedPhoneNumber with is_valid=False
    """
    if not phone or not isinstance(phone, str):
        raise InvalidFormatError("Phone number must be a non-empty string")
    
    # Check for invalid characters
    if re.search(r'[a-zA-Z]', phone):
        raise InvalidFormatError(f"Phone number contains invalid characters: {phone}")
    
    # Clean the input
    cleaned = clean_phone_number(phone)
    
    if not cleaned or cleaned == '+':
        raise InvalidFormatError("Phone number contains no digits")
    
    # Extract country code
    country_code, national_number = extract_country_code(cleaned)
    
    # Validate length
    min_length = 7
    max_length = 15
    
    if len(national_number) < min_length:
        raise InvalidLengthError(
            f"Phone number too short: {len(national_number)} digits "
            f"(minimum {min_length})"
        )
    
    if len(national_number) > max_length:
        raise InvalidLengthError(
            f"Phone number too long: {len(national_number)} digits "
            f"(maximum {max_length})"
        )
    
    # Extract area code (assume 3 digits for US/Canada format)
    area_code = None
    local_number = national_number
    
    if len(national_number) >= 10:
        area_code = national_number[:3]
        local_number = national_number[3:]
    
    # Format the number
    if country_code and area_code:
        formatted = f"+{country_code} ({area_code}) {local_number[:3]}-{local_number[3:]}"
    elif area_code:
        formatted = f"({area_code}) {local_number[:3]}-{local_number[3:]}"
    else:
        formatted = local_number
    
    # Determine validity
    is_valid = True
    if country_code and country_code in COUNTRY_CODES:
        expected_lengths = COUNTRY_CODES[country_code]["lengths"]
        is_valid = len(national_number) in expected_lengths
    
    return ParsedPhoneNumber(
        original=phone,
        country_code=country_code,
        area_code=area_code,
        local_number=local_number,
        formatted=formatted,
        is_valid=is_valid,
        phone_type=PhoneNumberType.UNKNOWN
    )


def is_valid_phone_number(phone: str) -> bool:
    """
    Quick validation check for phone numbers.
    
    Args:
        phone: Phone number string.
        
    Returns:
        True if valid, False otherwise.
        
    Examples:
        >>> is_valid_phone_number("+1 (234) 567-8900")
        True
        >>> is_valid_phone_number("123")
        False
    """
    try:
        result = parse_phone_number(phone)
        return result.is_valid
    except PhoneNumberError:
        return False


# ==================== UNIT TESTS ====================

import pytest


class TestCleanPhoneNumber:
    """Tests for clean_phone_number function."""
    
    def test_removes_spaces(self):
        assert clean_phone_number("234 567 8900") == "2345678900"
    
    def test_removes_dashes(self):
        assert clean_phone_number("234-567-8900") == "2345678900"
    
    def test_removes_parentheses(self):
        assert clean_phone_number("(234) 567-8900") == "2345678900"
    
    def test_preserves_plus(self):
        assert clean_phone_number("+1 234 567 8900") == "+12345678900"
    
    def test_empty_string(self):
        assert clean_phone_number("") == ""
    
    def test_only_plus(self):
        assert clean_phone_number("+") == "+"


class TestParsePhoneNumber:
    """Tests for parse_phone_number function."""
    
    def test_us_format_with_country_code(self):
        result = parse_phone_number("+1 (234) 567-8900")
        assert result.country_code == "1"
        assert result.area_code == "234"
        assert result.is_valid is True
    
    def test_us_format_without_country_code(self):
        result = parse_phone_number("(234) 567-8900")
        assert result.country_code is None
        assert result.area_code == "234"
    
    def test_international_format(self):
        result = parse_phone_number("+44 20 7946 0958")
        assert result.country_code == "44"
    
    def test_invalid_characters(self):
        with pytest.raises(InvalidFormatError):
            parse_phone_number("1-800-CALL-NOW")
    
    def test_too_short(self):
        with pytest.raises(InvalidLengthError):
            parse_phone_number("123")
    
    def test_too_long(self):
        with pytest.raises(InvalidLengthError):
            parse_phone_number("1234567890123456789")
    
    def test_empty_string(self):
        with pytest.raises(InvalidFormatError):
            parse_phone_number("")


class TestIsValidPhoneNumber:
    """Tests for is_valid_phone_number function."""
    
    def test_valid_number(self):
        assert is_valid_phone_number("+1 (234) 567-8900") is True
    
    def test_invalid_number(self):
        assert is_valid_phone_number("123") is False
    
    def test_invalid_characters(self):
        assert is_valid_phone_number("abc") is False


# ==================== USAGE EXAMPLE ====================

if __name__ == "__main__":
    # Example usage
    test_numbers = [
        "+1 (234) 567-8900",
        "234.567.8900",
        "+44 20 7946 0958",
        "1-800-555-0123",
        "invalid",
    ]
    
    for phone in test_numbers:
        try:
            result = parse_phone_number(phone)
            print(f"\nInput: {phone}")
            print(f"  Valid: {result.is_valid}")
            print(f"  Country: {result.country_code}")
            print(f"  Area: {result.area_code}")
            print(f"  Formatted: {result.formatted}")
        except PhoneNumberError as e:
            print(f"\nInput: {phone}")
            print(f"  Error: {e}")
```

---

### Example 2: API Function with Error Handling

**Prompt:**

```
Act as a Backend Developer expert in Python FastAPI.

Create an API endpoint for user authentication (login).

Requirements:
- Endpoint: POST /api/auth/login
- Accept: email and password
- Validate input data
- Verify credentials against database
- Generate JWT access token and refresh token
- Rate limit failed attempts (5 per minute)
- Log authentication attempts
- Return tokens on success, error details on failure

Technical Specs:
- Python 3.11+
- FastAPI framework
- Pydantic for validation
- SQLAlchemy for database
- python-jose for JWT
- bcrypt for password hashing
- HTTP status codes: 200, 400, 401, 429

Include:
- Pydantic request/response models
- Endpoint function with proper typing
- JWT token generation utility
- Password verification
- Rate limiting decorator
- Comprehensive error handling
- OpenAPI documentation
- Example request/response

Follow REST API best practices and OAuth2 password flow.
```

**Expected Output:**

```python
"""
Authentication API endpoint for user login.
"""
from datetime import datetime, timedelta
from typing import Optional
import logging

from fastapi import APIRouter, HTTPException, Depends, status, Request
from fastapi.security import OAuth2PasswordRequestForm
from pydantic import BaseModel, EmailStr, Field
from sqlalchemy.orm import Session
from jose import jwt, JWTError
import bcrypt

from app.database import get_db
from app.models.user import User
from app.core.config import settings
from app.core.rate_limit import rate_limit

# Configure logging
logger = logging.getLogger(__name__)

router = APIRouter(prefix="/api/auth", tags=["Authentication"])


# ==================== MODELS ====================

class LoginRequest(BaseModel):
    """Request model for user login."""
    email: EmailStr = Field(..., description="User's email address")
    password: str = Field(..., min_length=8, description="User's password")
    
    class Config:
        json_schema_extra = {
            "example": {
                "email": "user@example.com",
                "password": "SecurePassword123!"
            }
        }


class TokenResponse(BaseModel):
    """Response model for successful authentication."""
    access_token: str = Field(..., description="JWT access token")
    refresh_token: str = Field(..., description="JWT refresh token")
    token_type: str = Field(default="bearer", description="Token type")
    expires_in: int = Field(..., description="Token expiration in seconds")
    
    class Config:
        json_schema_extra = {
            "example": {
                "access_token": "eyJhbGciOiJIUzI1NiIs...",
                "refresh_token": "eyJhbGciOiJIUzI1NiIs...",
                "token_type": "bearer",
                "expires_in": 3600
            }
        }


class ErrorResponse(BaseModel):
    """Response model for errors."""
    detail: str = Field(..., description="Error message")
    error_code: str = Field(..., description="Error code for programmatic handling")
    
    class Config:
        json_schema_extra = {
            "example": {
                "detail": "Invalid email or password",
                "error_code": "INVALID_CREDENTIALS"
            }
        }


# ==================== UTILITIES ====================

def verify_password(plain_password: str, hashed_password: str) -> bool:
    """
    Verify a plain password against a hashed password.
    
    Args:
        plain_password: The password to verify.
        hashed_password: The stored hash to compare against.
        
    Returns:
        True if password matches, False otherwise.
    """
    return bcrypt.checkpw(
        plain_password.encode('utf-8'),
        hashed_password.encode('utf-8')
    )


def create_access_token(
    data: dict,
    expires_delta: Optional[timedelta] = None
) -> str:
    """
    Create a JWT access token.
    
    Args:
        data: Data to encode in the token.
        expires_delta: Optional custom expiration time.
        
    Returns:
        Encoded JWT token string.
    """
    to_encode = data.copy()
    
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=settings.ACCESS_TOKEN_EXPIRE_MINUTES)
    
    to_encode.update({
        "exp": expire,
        "type": "access"
    })
    
    return jwt.encode(
        to_encode,
        settings.SECRET_KEY,
        algorithm=settings.ALGORITHM
    )


def create_refresh_token(user_id: str) -> str:
    """
    Create a JWT refresh token.
    
    Args:
        user_id: User's unique identifier.
        
    Returns:
        Encoded JWT refresh token string.
    """
    expire = datetime.utcnow() + timedelta(days=settings.REFRESH_TOKEN_EXPIRE_DAYS)
    
    to_encode = {
        "sub": user_id,
        "exp": expire,
        "type": "refresh"
    }
    
    return jwt.encode(
        to_encode,
        settings.SECRET_KEY,
        algorithm=settings.ALGORITHM
    )


def get_user_by_email(db: Session, email: str) -> Optional[User]:
    """
    Retrieve user by email address.
    
    Args:
        db: Database session.
        email: User's email address.
        
    Returns:
        User object if found, None otherwise.
    """
    return db.query(User).filter(User.email == email.lower()).first()


# ==================== ENDPOINT ====================

@router.post(
    "/login",
    response_model=TokenResponse,
    responses={
        400: {"model": ErrorResponse, "description": "Invalid request"},
        401: {"model": ErrorResponse, "description": "Invalid credentials"},
        429: {"model": ErrorResponse, "description": "Too many failed attempts"},
    },
    summary="User Login",
    description="Authenticate user with email and password, returns JWT tokens."
)
@rate_limit(max_requests=5, window_seconds=60, key_func=lambda r: r.client.host)
async def login(
    request: Request,
    credentials: LoginRequest,
    db: Session = Depends(get_db)
) -> TokenResponse:
    """
    Authenticate user and return JWT tokens.
    
    This endpoint validates user credentials and returns access and refresh
    tokens for authenticated API access.
    
    - **email**: User's registered email address
    - **password**: User's password (minimum 8 characters)
    
    Returns JWT access token (1 hour) and refresh token (7 days).
    
    Raises:
        HTTPException 400: If request validation fails
        HTTPException 401: If credentials are invalid
        HTTPException 429: If rate limit exceeded (5 attempts/minute)
    """
    # Log attempt (without password!)
    logger.info(f"Login attempt for email: {credentials.email}")
    
    # Find user
    user = get_user_by_email(db, credentials.email)
    
    if not user:
        logger.warning(f"Login failed: User not found - {credentials.email}")
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid email or password",
            headers={"WWW-Authenticate": "Bearer"},
        )
    
    # Verify password
    if not verify_password(credentials.password, user.hashed_password):
        logger.warning(f"Login failed: Invalid password - {credentials.email}")
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid email or password",
            headers={"WWW-Authenticate": "Bearer"},
        )
    
    # Check if user is active
    if not user.is_active:
        logger.warning(f"Login failed: Inactive account - {credentials.email}")
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Account is inactive. Please contact support.",
            headers={"WWW-Authenticate": "Bearer"},
        )
    
    # Generate tokens
    access_token = create_access_token(
        data={"sub": str(user.id), "email": user.email}
    )
    refresh_token = create_refresh_token(str(user.id))
    
    # Update last login
    user.last_login = datetime.utcnow()
    db.commit()
    
    logger.info(f"Login successful: {credentials.email}")
    
    return TokenResponse(
        access_token=access_token,
        refresh_token=refresh_token,
        token_type="bearer",
        expires_in=settings.ACCESS_TOKEN_EXPIRE_MINUTES * 60
    )


# ==================== USAGE EXAMPLE ====================

"""
Example curl request:

curl -X POST "http://localhost:8000/api/auth/login" \
     -H "Content-Type: application/json" \
     -d '{"email": "user@example.com", "password": "SecurePassword123!"}'

Example successful response:
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "bearer",
    "expires_in": 3600
}

Example error response:
{
    "detail": "Invalid email or password"
}
"""
```

---

## 🏗️ Class Generation

### Example: Service Class with SOLID Principles

**Prompt:**

```
Act as a Senior Software Engineer specializing in clean architecture.

Create a UserService class for user management operations.

Methods Needed:
- create_user(data) → User
- get_user(user_id) → User
- get_user_by_email(email) → User
- update_user(user_id, data) → User
- delete_user(user_id) → bool
- list_users(page, limit, filters) → PaginatedResult[User]
- change_password(user_id, old_password, new_password) → bool
- verify_email(user_id, token) → bool

Requirements:
- Dependency injection for repository
- Input validation
- Business logic separation from data access
- Custom exceptions for error handling
- Comprehensive logging
- Transaction management for multi-step operations

Technical Specs:
- Python 3.11+
- Full type hints
- Async methods
- Follow SOLID principles
- Google-style docstrings

Include:
- UserService class
- Custom exception classes
- Interface/Protocol for repository
- Pydantic models for input/output
- Example usage
- Unit tests with mocked repository

Optimize for testability and maintainability.
```

**Expected Output:**

```python
"""
User Service - Business logic for user management.

This module implements the service layer for user operations,
following clean architecture principles with dependency injection.
"""
from abc import ABC, abstractmethod
from dataclasses import dataclass
from datetime import datetime
from typing import Optional, List, Protocol, Generic, TypeVar
from enum import Enum
import logging
import uuid
import bcrypt

from pydantic import BaseModel, EmailStr, Field, field_validator

# Configure logging
logger = logging.getLogger(__name__)

T = TypeVar('T')


# ==================== EXCEPTIONS ====================

class UserServiceError(Exception):
    """Base exception for user service errors."""
    pass


class UserNotFoundError(UserServiceError):
    """Raised when user is not found."""
    def __init__(self, identifier: str):
        self.identifier = identifier
        super().__init__(f"User not found: {identifier}")


class DuplicateEmailError(UserServiceError):
    """Raised when email already exists."""
    def __init__(self, email: str):
        self.email = email
        super().__init__(f"Email already registered: {email}")


class InvalidCredentialsError(UserServiceError):
    """Raised when credentials are invalid."""
    pass


class ValidationError(UserServiceError):
    """Raised when input validation fails."""
    def __init__(self, field: str, message: str):
        self.field = field
        super().__init__(f"Validation error for {field}: {message}")


class InvalidTokenError(UserServiceError):
    """Raised when verification token is invalid."""
    pass


# ==================== MODELS ====================

class UserStatus(Enum):
    """User account status."""
    ACTIVE = "active"
    INACTIVE = "inactive"
    PENDING = "pending"
    SUSPENDED = "suspended"


@dataclass
class User:
    """Domain model for User."""
    id: str
    email: str
    hashed_password: str
    first_name: str
    last_name: str
    status: UserStatus
    email_verified: bool
    created_at: datetime
    updated_at: datetime
    
    @property
    def full_name(self) -> str:
        """Get user's full name."""
        return f"{self.first_name} {self.last_name}"


class CreateUserDTO(BaseModel):
    """Data transfer object for creating a user."""
    email: EmailStr
    password: str = Field(..., min_length=8)
    first_name: str = Field(..., min_length=1, max_length=50)
    last_name: str = Field(..., min_length=1, max_length=50)
    
    @field_validator('password')
    @classmethod
    def validate_password(cls, v: str) -> str:
        """Ensure password meets security requirements."""
        if not any(c.isupper() for c in v):
            raise ValueError('Password must contain uppercase letter')
        if not any(c.islower() for c in v):
            raise ValueError('Password must contain lowercase letter')
        if not any(c.isdigit() for c in v):
            raise ValueError('Password must contain digit')
        return v


class UpdateUserDTO(BaseModel):
    """Data transfer object for updating a user."""
    first_name: Optional[str] = Field(None, min_length=1, max_length=50)
    last_name: Optional[str] = Field(None, min_length=1, max_length=50)
    status: Optional[UserStatus] = None


class UserFilters(BaseModel):
    """Filters for listing users."""
    status: Optional[UserStatus] = None
    email_verified: Optional[bool] = None
    search: Optional[str] = None


@dataclass
class PaginatedResult(Generic[T]):
    """Generic paginated result."""
    items: List[T]
    total: int
    page: int
    limit: int
    total_pages: int
    has_next: bool
    has_prev: bool


# ==================== REPOSITORY PROTOCOL ====================

class UserRepositoryProtocol(Protocol):
    """Protocol defining repository interface."""
    
    async def add(self, user: User) -> User:
        """Add a new user."""
        ...
    
    async def get_by_id(self, user_id: str) -> Optional[User]:
        """Get user by ID."""
        ...
    
    async def get_by_email(self, email: str) -> Optional[User]:
        """Get user by email."""
        ...
    
    async def update(self, user: User) -> User:
        """Update existing user."""
        ...
    
    async def delete(self, user_id: str) -> bool:
        """Delete user by ID."""
        ...
    
    async def list(
        self,
        page: int,
        limit: int,
        filters: Optional[UserFilters]
    ) -> tuple[List[User], int]:
        """List users with pagination and filters."""
        ...


# ==================== SERVICE ====================

class UserService:
    """
    Service class for user management operations.
    
    This service encapsulates business logic for user operations,
    using dependency injection for the repository layer.
    
    Attributes:
        repository: User repository for data access.
        
    Example:
        >>> repo = InMemoryUserRepository()
        >>> service = UserService(repo)
        >>> user = await service.create_user(CreateUserDTO(
        ...     email="user@example.com",
        ...     password="SecurePass123!",
        ...     first_name="John",
        ...     last_name="Doe"
        ... ))
        >>> print(user.email)
        user@example.com
    """
    
    def __init__(self, repository: UserRepositoryProtocol):
        """
        Initialize UserService with repository.
        
        Args:
            repository: Implementation of UserRepositoryProtocol.
        """
        self._repository = repository
        logger.info("UserService initialized")
    
    async def create_user(self, data: CreateUserDTO) -> User:
        """
        Create a new user account.
        
        Args:
            data: User creation data.
            
        Returns:
            Created User object.
            
        Raises:
            DuplicateEmailError: If email already exists.
            ValidationError: If input validation fails.
        """
        logger.info(f"Creating user with email: {data.email}")
        
        # Check for duplicate email
        existing = await self._repository.get_by_email(data.email.lower())
        if existing:
            logger.warning(f"Duplicate email attempt: {data.email}")
            raise DuplicateEmailError(data.email)
        
        # Hash password
        hashed_password = self._hash_password(data.password)
        
        # Create user entity
        now = datetime.utcnow()
        user = User(
            id=str(uuid.uuid4()),
            email=data.email.lower(),
            hashed_password=hashed_password,
            first_name=data.first_name,
            last_name=data.last_name,
            status=UserStatus.PENDING,
            email_verified=False,
            created_at=now,
            updated_at=now
        )
        
        # Save to repository
        created_user = await self._repository.add(user)
        logger.info(f"User created: {created_user.id}")
        
        return created_user
    
    async def get_user(self, user_id: str) -> User:
        """
        Retrieve user by ID.
        
        Args:
            user_id: User's unique identifier.
            
        Returns:
            User object.
            
        Raises:
            UserNotFoundError: If user doesn't exist.
        """
        user = await self._repository.get_by_id(user_id)
        
        if not user:
            logger.warning(f"User not found: {user_id}")
            raise UserNotFoundError(user_id)
        
        return user
    
    async def get_user_by_email(self, email: str) -> User:
        """
        Retrieve user by email address.
        
        Args:
            email: User's email address.
            
        Returns:
            User object.
            
        Raises:
            UserNotFoundError: If user doesn't exist.
        """
        user = await self._repository.get_by_email(email.lower())
        
        if not user:
            logger.warning(f"User not found by email: {email}")
            raise UserNotFoundError(email)
        
        return user
    
    async def update_user(self, user_id: str, data: UpdateUserDTO) -> User:
        """
        Update user information.
        
        Args:
            user_id: User's unique identifier.
            data: Update data (partial updates supported).
            
        Returns:
            Updated User object.
            
        Raises:
            UserNotFoundError: If user doesn't exist.
        """
        user = await self.get_user(user_id)
        
        # Apply updates
        update_dict = data.model_dump(exclude_unset=True)
        for field, value in update_dict.items():
            setattr(user, field, value)
        
        user.updated_at = datetime.utcnow()
        
        updated_user = await self._repository.update(user)
        logger.info(f"User updated: {user_id}")
        
        return updated_user
    
    async def delete_user(self, user_id: str) -> bool:
        """
        Delete user account.
        
        Args:
            user_id: User's unique identifier.
            
        Returns:
            True if deleted successfully.
            
        Raises:
            UserNotFoundError: If user doesn't exist.
        """
        # Verify user exists
        await self.get_user(user_id)
        
        result = await self._repository.delete(user_id)
        logger.info(f"User deleted: {user_id}")
        
        return result
    
    async def list_users(
        self,
        page: int = 1,
        limit: int = 20,
        filters: Optional[UserFilters] = None
    ) -> PaginatedResult[User]:
        """
        List users with pagination and optional filters.
        
        Args:
            page: Page number (1-indexed).
            limit: Items per page (max 100).
            filters: Optional filter criteria.
            
        Returns:
            Paginated list of users.
            
        Raises:
            ValidationError: If page/limit are invalid.
        """
        # Validate pagination
        if page < 1:
            raise ValidationError("page", "Must be >= 1")
        if limit < 1 or limit > 100:
            raise ValidationError("limit", "Must be between 1 and 100")
        
        users, total = await self._repository.list(page, limit, filters)
        
        total_pages = (total + limit - 1) // limit
        
        return PaginatedResult(
            items=users,
            total=total,
            page=page,
            limit=limit,
            total_pages=total_pages,
            has_next=page < total_pages,
            has_prev=page > 1
        )
    
    async def change_password(
        self,
        user_id: str,
        old_password: str,
        new_password: str
    ) -> bool:
        """
        Change user's password.
        
        Args:
            user_id: User's unique identifier.
            old_password: Current password for verification.
            new_password: New password to set.
            
        Returns:
            True if password changed successfully.
            
        Raises:
            UserNotFoundError: If user doesn't exist.
            InvalidCredentialsError: If old password is incorrect.
            ValidationError: If new password doesn't meet requirements.
        """
        user = await self.get_user(user_id)
        
        # Verify old password
        if not self._verify_password(old_password, user.hashed_password):
            logger.warning(f"Password change failed - invalid old password: {user_id}")
            raise InvalidCredentialsError("Current password is incorrect")
        
        # Validate new password (reuse DTO validation)
        try:
            CreateUserDTO.model_validate({
                "email": user.email,
                "password": new_password,
                "first_name": user.first_name,
                "last_name": user.last_name
            })
        except Exception as e:
            raise ValidationError("new_password", str(e))
        
        # Hash and update
        user.hashed_password = self._hash_password(new_password)
        user.updated_at = datetime.utcnow()
        
        await self._repository.update(user)
        logger.info(f"Password changed: {user_id}")
        
        return True
    
    async def verify_email(self, user_id: str, token: str) -> bool:
        """
        Verify user's email address.
        
        Args:
            user_id: User's unique identifier.
            token: Email verification token.
            
        Returns:
            True if verification successful.
            
        Raises:
            UserNotFoundError: If user doesn't exist.
            InvalidTokenError: If token is invalid.
        """
        user = await self.get_user(user_id)
        
        # In real implementation, validate token against stored token
        # For example: compare with token in cache/database
        if not self._validate_verification_token(user_id, token):
            raise InvalidTokenError("Invalid or expired verification token")
        
        user.email_verified = True
        user.status = UserStatus.ACTIVE
        user.updated_at = datetime.utcnow()
        
        await self._repository.update(user)
        logger.info(f"Email verified: {user_id}")
        
        return True
    
    # ==================== PRIVATE METHODS ====================
    
    def _hash_password(self, password: str) -> str:
        """Hash password using bcrypt."""
        salt = bcrypt.gensalt()
        return bcrypt.hashpw(password.encode('utf-8'), salt).decode('utf-8')
    
    def _verify_password(self, plain_password: str, hashed_password: str) -> bool:
        """Verify password against hash."""
        return bcrypt.checkpw(
            plain_password.encode('utf-8'),
            hashed_password.encode('utf-8')
        )
    
    def _validate_verification_token(self, user_id: str, token: str) -> bool:
        """Validate email verification token."""
        # Placeholder - implement actual token validation
        return len(token) >= 32


# ==================== UNIT TESTS ====================

import pytest
from unittest.mock import AsyncMock, MagicMock


class TestUserService:
    """Unit tests for UserService."""
    
    @pytest.fixture
    def mock_repository(self):
        """Create mock repository."""
        repo = MagicMock(spec=UserRepositoryProtocol)
        repo.add = AsyncMock()
        repo.get_by_id = AsyncMock()
        repo.get_by_email = AsyncMock()
        repo.update = AsyncMock()
        repo.delete = AsyncMock()
        repo.list = AsyncMock()
        return repo
    
    @pytest.fixture
    def service(self, mock_repository):
        """Create service with mock repository."""
        return UserService(mock_repository)
    
    @pytest.fixture
    def sample_user(self):
        """Create sample user."""
        return User(
            id="user-123",
            email="test@example.com",
            hashed_password="hashed",
            first_name="John",
            last_name="Doe",
            status=UserStatus.ACTIVE,
            email_verified=True,
            created_at=datetime.utcnow(),
            updated_at=datetime.utcnow()
        )
    
    @pytest.mark.asyncio
    async def test_create_user_success(self, service, mock_repository):
        """Test successful user creation."""
        mock_repository.get_by_email.return_value = None
        mock_repository.add.return_value = MagicMock()
        
        data = CreateUserDTO(
            email="test@example.com",
            password="SecurePass123!",
            first_name="John",
            last_name="Doe"
        )
        
        await service.create_user(data)
        
        mock_repository.add.assert_called_once()
    
    @pytest.mark.asyncio
    async def test_create_user_duplicate_email(self, service, mock_repository, sample_user):
        """Test duplicate email prevention."""
        mock_repository.get_by_email.return_value = sample_user
        
        data = CreateUserDTO(
            email="test@example.com",
            password="SecurePass123!",
            first_name="John",
            last_name="Doe"
        )
        
        with pytest.raises(DuplicateEmailError):
            await service.create_user(data)
    
    @pytest.mark.asyncio
    async def test_get_user_not_found(self, service, mock_repository):
        """Test user not found error."""
        mock_repository.get_by_id.return_value = None
        
        with pytest.raises(UserNotFoundError):
            await service.get_user("nonexistent-id")
```

---

## 🔄 Code Refactoring

### Refactoring Template

```
Act as a Senior Developer expert in code refactoring.

Refactor the following code to improve [aspect].

Current Code:
```[language]
[Paste code here]
```

Issues Identified:
- [Issue 1]
- [Issue 2]

Refactoring Goals:
- Improve readability
- Reduce complexity
- Follow [specific principles]
- Add proper error handling
- Add documentation
- Add type hints

Provide:
1. Refactored code
2. Explanation of each change
3. Before/after comparison
4. Any trade-offs or considerations
```

### Example: Simplifying Complex Function

**Prompt:**

```
Act as a Senior Python Developer specializing in clean code.

Refactor this complex function to improve readability and maintainability.

Current Code:
```python
def process_order(order_data):
    if order_data:
        if 'items' in order_data and len(order_data['items']) > 0:
            total = 0
            for item in order_data['items']:
                if 'price' in item and 'quantity' in item:
                    total += item['price'] * item['quantity']
            if total > 100:
                discount = total * 0.1
                total = total - discount
            if 'shipping' in order_data:
                total += order_data['shipping']
            return {'total': total, 'status': 'success'}
        else:
            return {'error': 'No items in order'}
    else:
        return {'error': 'Invalid order data'}
```

Refactoring Goals:
- Extract helper functions
- Use early returns (guard clauses)
- Add type hints
- Add proper error handling
- Add docstrings
- Reduce nesting depth
- Make it testable

Provide:
- Refactored code
- Explanation of changes
- Unit tests
```

**Expected Refactored Code:**

```python
"""
Order processing module with clean, maintainable code.
"""
from dataclasses import dataclass
from typing import List, Optional
from decimal import Decimal


@dataclass
class OrderItem:
    """Represents an item in an order."""
    price: Decimal
    quantity: int
    
    @property
    def subtotal(self) -> Decimal:
        """Calculate item subtotal."""
        return self.price * self.quantity


@dataclass
class OrderResult:
    """Result of order processing."""
    total: Decimal
    discount: Decimal
    shipping: Decimal
    subtotal: Decimal
    status: str = "success"
    
    @classmethod
    def success(cls, subtotal: Decimal, discount: Decimal, shipping: Decimal) -> 'OrderResult':
        """Create successful order result."""
        return cls(
            total=subtotal - discount + shipping,
            discount=discount,
            shipping=shipping,
            subtotal=subtotal
        )


class OrderError(Exception):
    """Base exception for order processing errors."""
    pass


class EmptyOrderError(OrderError):
    """Raised when order has no items."""
    pass


class InvalidOrderError(OrderError):
    """Raised when order data is invalid."""
    pass


class InvalidItemError(OrderError):
    """Raised when an item is invalid."""
    pass


# Configuration
DISCOUNT_THRESHOLD = Decimal("100.00")
DISCOUNT_RATE = Decimal("0.10")


def validate_order_data(order_data: Optional[dict]) -> None:
    """
    Validate order data structure.
    
    Args:
        order_data: Raw order dictionary.
        
    Raises:
        InvalidOrderError: If order data is None or not a dict.
        EmptyOrderError: If order has no items.
    """
    if not order_data:
        raise InvalidOrderError("Order data is required")
    
    if not isinstance(order_data, dict):
        raise InvalidOrderError("Order data must be a dictionary")
    
    items = order_data.get('items', [])
    if not items:
        raise EmptyOrderError("Order must contain at least one item")


def parse_order_items(items: List[dict]) -> List[OrderItem]:
    """
    Parse and validate order items.
    
    Args:
        items: List of item dictionaries.
        
    Returns:
        List of validated OrderItem objects.
        
    Raises:
        InvalidItemError: If an item is missing required fields.
    """
    parsed_items = []
    
    for idx, item in enumerate(items):
        if 'price' not in item or 'quantity' not in item:
            raise InvalidItemError(f"Item {idx} missing price or quantity")
        
        parsed_items.append(OrderItem(
            price=Decimal(str(item['price'])),
            quantity=int(item['quantity'])
        ))
    
    return parsed_items


def calculate_subtotal(items: List[OrderItem]) -> Decimal:
    """Calculate order subtotal from items."""
    return sum(item.subtotal for item in items)


def calculate_discount(subtotal: Decimal) -> Decimal:
    """
    Calculate discount based on subtotal.
    
    10% discount applied for orders over $100.
    """
    if subtotal > DISCOUNT_THRESHOLD:
        return subtotal * DISCOUNT_RATE
    return Decimal("0.00")


def get_shipping_cost(order_data: dict) -> Decimal:
    """Extract shipping cost from order data."""
    return Decimal(str(order_data.get('shipping', 0)))


def process_order(order_data: Optional[dict]) -> OrderResult:
    """
    Process an order and calculate totals.
    
    Validates order data, calculates subtotal, applies discounts,
    and adds shipping costs.
    
    Args:
        order_data: Dictionary containing order information.
            Expected structure:
            {
                "items": [{"price": float, "quantity": int}, ...],
                "shipping": float (optional)
            }
    
    Returns:
        OrderResult with calculated totals.
        
    Raises:
        InvalidOrderError: If order data is invalid.
        EmptyOrderError: If order has no items.
        InvalidItemError: If an item is malformed.
        
    Examples:
        >>> result = process_order({
        ...     "items": [{"price": 50.00, "quantity": 3}],
        ...     "shipping": 5.99
        ... })
        >>> result.total
        Decimal('140.99')
        >>> result.discount
        Decimal('15.00')
    """
    # Validate input
    validate_order_data(order_data)
    
    # Parse items
    items = parse_order_items(order_data['items'])
    
    # Calculate totals
    subtotal = calculate_subtotal(items)
    discount = calculate_discount(subtotal)
    shipping = get_shipping_cost(order_data)
    
    return OrderResult.success(subtotal, discount, shipping)


# ==================== UNIT TESTS ====================

import pytest


class TestProcessOrder:
    """Tests for process_order function."""
    
    def test_valid_order_without_discount(self):
        """Test order below discount threshold."""
        order = {"items": [{"price": 25.00, "quantity": 2}]}
        result = process_order(order)
        
        assert result.subtotal == Decimal("50.00")
        assert result.discount == Decimal("0.00")
        assert result.total == Decimal("50.00")
    
    def test_valid_order_with_discount(self):
        """Test order above discount threshold."""
        order = {"items": [{"price": 50.00, "quantity": 3}]}
        result = process_order(order)
        
        assert result.subtotal == Decimal("150.00")
        assert result.discount == Decimal("15.00")
        assert result.total == Decimal("135.00")
    
    def test_order_with_shipping(self):
        """Test order with shipping cost."""
        order = {
            "items": [{"price": 50.00, "quantity": 3}],
            "shipping": 5.99
        }
        result = process_order(order)
        
        assert result.shipping == Decimal("5.99")
        assert result.total == Decimal("140.99")
    
    def test_none_order_raises_error(self):
        """Test None order data."""
        with pytest.raises(InvalidOrderError):
            process_order(None)
    
    def test_empty_items_raises_error(self):
        """Test empty items list."""
        with pytest.raises(EmptyOrderError):
            process_order({"items": []})
    
    def test_missing_item_fields_raises_error(self):
        """Test item missing required fields."""
        with pytest.raises(InvalidItemError):
            process_order({"items": [{"price": 10.00}]})
    
    def test_multiple_items(self):
        """Test order with multiple items."""
        order = {
            "items": [
                {"price": 10.00, "quantity": 2},
                {"price": 25.00, "quantity": 4}
            ]
        }
        result = process_order(order)
        
        assert result.subtotal == Decimal("120.00")
        assert result.discount == Decimal("12.00")
```

---

## 💻 Practical Exercises

### Exercise 5.1: Generate a Validation Function

**Task:** Create a function that validates credit card numbers using the Luhn algorithm.

**Requirements:**
- Validate card number format (13-19 digits)
- Implement Luhn algorithm
- Identify card type (Visa, Mastercard, Amex, Discover)
- Check expiry date
- Return detailed validation result

**Write your prompt:**
```
[Your prompt here]
```

---

### Exercise 5.2: Refactor This Code

**Task:** Refactor the following calculator code to be more maintainable:

```python
def calc(x, y, op):
    if op == '+':
        return x + y
    elif op == '-':
        return x - y
    elif op == '*':
        return x * y
    elif op == '/':
        return x / y if y != 0 else None
    elif op == '%':
        return x % y if y != 0 else None
    elif op == '**':
        return x ** y
    else:
        return None
```

**Goals:**
- Apply Strategy Pattern
- Add error handling
- Add type hints
- Make it extensible
- Add unit tests

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
│  ✅ AI accelerates development by 50-70%                        │
│                                                                  │
│  ✅ Be specific about requirements and tech stack               │
│                                                                  │
│  ✅ Always request type hints and documentation                 │
│                                                                  │
│  ✅ Include error handling in your prompts                      │
│                                                                  │
│  ✅ Request unit tests with your code                           │
│                                                                  │
│  ✅ Review and test ALL generated code                          │
│                                                                  │
│  ✅ Refactoring improves existing code quality                  │
│                                                                  │
│  ✅ Use early returns to reduce nesting                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ Lesson 1 Checklist

Before moving on:
- [ ] Can write effective prompts for function generation
- [ ] Can generate service classes with SOLID principles
- [ ] Can apply refactoring techniques
- [ ] Understand code review process for generated code
- [ ] Completed Exercises 5.1-5.2

---

## 🔗 Next Steps

Continue to:
- [Lesson 2: Design Patterns and Architecture](./02-design-patterns-and-architecture.md)
- [Lesson 3: Full-Stack Development](./03-full-stack-development.md)

---

**Great progress! Continue to learn about design patterns! 🚀**
