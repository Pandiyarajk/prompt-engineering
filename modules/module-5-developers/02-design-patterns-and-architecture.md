# Module 5: Prompt Engineering for Developers

## Lesson 2: Design Patterns and Architecture

---

## 🎯 Lesson Overview

**Time Required:** 60 minutes  
**Difficulty:** Advanced  
**Prerequisites:** Lesson 1 completed

In this lesson, you'll learn:
- How to generate design pattern implementations
- When to use each pattern
- Architectural patterns for clean code
- Best practices for maintainable systems

---

## 📚 Introduction to Design Patterns

Design patterns are proven solutions to common software design problems. AI can help you implement them correctly, but you need to understand when to apply each pattern.

### Pattern Categories

```
┌─────────────────────────────────────────────────────────────────┐
│  DESIGN PATTERN CATEGORIES                                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  CREATIONAL PATTERNS                                             │
│  └─ How objects are created                                     │
│     • Factory, Abstract Factory, Builder, Singleton, Prototype │
│                                                                  │
│  STRUCTURAL PATTERNS                                             │
│  └─ How objects are composed                                    │
│     • Adapter, Bridge, Composite, Decorator, Facade, Proxy     │
│                                                                  │
│  BEHAVIORAL PATTERNS                                             │
│  └─ How objects interact                                        │
│     • Strategy, Observer, Command, State, Template Method      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Strategy Pattern

### When to Use
- Multiple algorithms that can be used interchangeably
- Need to switch behavior at runtime
- Want to avoid large if-else chains

### Generation Prompt

```
Act as a Software Architect expert in design patterns.

Implement the Strategy Pattern for a payment processing system.

Context:
- Support multiple payment methods (CreditCard, PayPal, BankTransfer, Crypto)
- Each payment method has different processing logic
- Need to easily add new payment methods
- Process payments and handle responses uniformly

Requirements:
- Abstract strategy interface
- Concrete strategy implementations for each payment type
- Context class that uses strategies
- Easy to add new payment types without modifying existing code
- Proper error handling
- Logging for each transaction

Technical Specs:
- Python 3.11+
- Type hints and Protocol
- Dataclasses for data structures
- Follow SOLID principles

Include:
- Strategy interface (Protocol)
- All concrete strategies
- Context class (PaymentProcessor)
- Data models
- Factory for creating strategies
- Unit tests
- Usage examples

Show how to add a new payment method to demonstrate extensibility.
```

**Expected Output:**

```python
"""
Payment Processing System using Strategy Pattern.

This module demonstrates the Strategy Pattern for handling
multiple payment methods in a flexible, extensible way.
"""
from abc import ABC, abstractmethod
from dataclasses import dataclass
from datetime import datetime
from enum import Enum
from typing import Protocol, Dict, Type, Optional
import logging
import uuid

logger = logging.getLogger(__name__)


# ==================== DATA MODELS ====================

class PaymentStatus(Enum):
    """Status of a payment transaction."""
    PENDING = "pending"
    PROCESSING = "processing"
    COMPLETED = "completed"
    FAILED = "failed"
    REFUNDED = "refunded"


@dataclass
class PaymentRequest:
    """Request to process a payment."""
    amount: float
    currency: str
    customer_id: str
    description: str
    metadata: Optional[Dict] = None


@dataclass
class PaymentResult:
    """Result of a payment transaction."""
    transaction_id: str
    status: PaymentStatus
    amount: float
    currency: str
    payment_method: str
    message: str
    timestamp: datetime
    
    @classmethod
    def success(cls, amount: float, currency: str, method: str, message: str = "Payment successful") -> 'PaymentResult':
        return cls(
            transaction_id=str(uuid.uuid4()),
            status=PaymentStatus.COMPLETED,
            amount=amount,
            currency=currency,
            payment_method=method,
            message=message,
            timestamp=datetime.utcnow()
        )
    
    @classmethod
    def failure(cls, amount: float, currency: str, method: str, message: str) -> 'PaymentResult':
        return cls(
            transaction_id=str(uuid.uuid4()),
            status=PaymentStatus.FAILED,
            amount=amount,
            currency=currency,
            payment_method=method,
            message=message,
            timestamp=datetime.utcnow()
        )


# ==================== STRATEGY INTERFACE ====================

class PaymentStrategy(Protocol):
    """
    Protocol defining the payment strategy interface.
    
    All payment methods must implement this interface.
    """
    
    def process(self, request: PaymentRequest) -> PaymentResult:
        """Process a payment request."""
        ...
    
    def validate(self, request: PaymentRequest) -> bool:
        """Validate payment request before processing."""
        ...
    
    @property
    def name(self) -> str:
        """Get the payment method name."""
        ...


# ==================== CONCRETE STRATEGIES ====================

class CreditCardStrategy:
    """Credit card payment strategy."""
    
    def __init__(self, card_number: str, expiry: str, cvv: str):
        self.card_number = card_number
        self.expiry = expiry
        self.cvv = cvv
    
    @property
    def name(self) -> str:
        return "credit_card"
    
    def validate(self, request: PaymentRequest) -> bool:
        """Validate credit card payment."""
        if request.amount <= 0:
            return False
        if len(self.card_number) < 13:
            return False
        return True
    
    def process(self, request: PaymentRequest) -> PaymentResult:
        """Process credit card payment."""
        logger.info(f"Processing credit card payment: {request.amount} {request.currency}")
        
        if not self.validate(request):
            return PaymentResult.failure(
                request.amount, request.currency, self.name,
                "Validation failed"
            )
        
        # Simulate payment gateway call
        # In real implementation, call actual payment gateway
        last_four = self.card_number[-4:]
        
        return PaymentResult.success(
            request.amount, request.currency, self.name,
            f"Payment completed with card ending in {last_four}"
        )


class PayPalStrategy:
    """PayPal payment strategy."""
    
    def __init__(self, email: str, auth_token: str):
        self.email = email
        self.auth_token = auth_token
    
    @property
    def name(self) -> str:
        return "paypal"
    
    def validate(self, request: PaymentRequest) -> bool:
        """Validate PayPal payment."""
        if request.amount <= 0:
            return False
        if "@" not in self.email:
            return False
        return True
    
    def process(self, request: PaymentRequest) -> PaymentResult:
        """Process PayPal payment."""
        logger.info(f"Processing PayPal payment: {request.amount} {request.currency}")
        
        if not self.validate(request):
            return PaymentResult.failure(
                request.amount, request.currency, self.name,
                "Invalid PayPal account"
            )
        
        return PaymentResult.success(
            request.amount, request.currency, self.name,
            f"Payment completed via PayPal ({self.email})"
        )


class BankTransferStrategy:
    """Bank transfer payment strategy."""
    
    def __init__(self, account_number: str, routing_number: str, account_name: str):
        self.account_number = account_number
        self.routing_number = routing_number
        self.account_name = account_name
    
    @property
    def name(self) -> str:
        return "bank_transfer"
    
    def validate(self, request: PaymentRequest) -> bool:
        """Validate bank transfer."""
        return request.amount > 0 and len(self.account_number) >= 8
    
    def process(self, request: PaymentRequest) -> PaymentResult:
        """Process bank transfer."""
        logger.info(f"Processing bank transfer: {request.amount} {request.currency}")
        
        if not self.validate(request):
            return PaymentResult.failure(
                request.amount, request.currency, self.name,
                "Invalid bank account details"
            )
        
        return PaymentResult.success(
            request.amount, request.currency, self.name,
            f"Bank transfer initiated to {self.account_name}"
        )


class CryptoStrategy:
    """Cryptocurrency payment strategy."""
    
    def __init__(self, wallet_address: str, crypto_type: str = "BTC"):
        self.wallet_address = wallet_address
        self.crypto_type = crypto_type
    
    @property
    def name(self) -> str:
        return f"crypto_{self.crypto_type.lower()}"
    
    def validate(self, request: PaymentRequest) -> bool:
        """Validate crypto payment."""
        return request.amount > 0 and len(self.wallet_address) >= 26
    
    def process(self, request: PaymentRequest) -> PaymentResult:
        """Process crypto payment."""
        logger.info(f"Processing {self.crypto_type} payment: {request.amount} {request.currency}")
        
        if not self.validate(request):
            return PaymentResult.failure(
                request.amount, request.currency, self.name,
                "Invalid wallet address"
            )
        
        return PaymentResult.success(
            request.amount, request.currency, self.name,
            f"Crypto payment sent to {self.wallet_address[:10]}..."
        )


# ==================== CONTEXT (PROCESSOR) ====================

class PaymentProcessor:
    """
    Context class that uses payment strategies.
    
    This class provides a unified interface for processing payments
    using different payment methods (strategies).
    """
    
    def __init__(self, strategy: PaymentStrategy):
        """
        Initialize with a payment strategy.
        
        Args:
            strategy: Payment strategy to use.
        """
        self._strategy = strategy
        logger.info(f"PaymentProcessor initialized with {strategy.name}")
    
    @property
    def strategy(self) -> PaymentStrategy:
        """Get current strategy."""
        return self._strategy
    
    @strategy.setter
    def strategy(self, strategy: PaymentStrategy) -> None:
        """Change payment strategy at runtime."""
        self._strategy = strategy
        logger.info(f"Strategy changed to {strategy.name}")
    
    def process_payment(self, request: PaymentRequest) -> PaymentResult:
        """
        Process payment using current strategy.
        
        Args:
            request: Payment request details.
            
        Returns:
            PaymentResult with transaction details.
        """
        logger.info(f"Processing payment of {request.amount} {request.currency}")
        return self._strategy.process(request)


# ==================== ADDING NEW PAYMENT METHOD ====================

# To add a new payment method, simply create a new strategy class:

class ApplePayStrategy:
    """Apple Pay payment strategy - demonstrates extensibility."""
    
    def __init__(self, device_token: str):
        self.device_token = device_token
    
    @property
    def name(self) -> str:
        return "apple_pay"
    
    def validate(self, request: PaymentRequest) -> bool:
        return request.amount > 0 and len(self.device_token) >= 20
    
    def process(self, request: PaymentRequest) -> PaymentResult:
        logger.info(f"Processing Apple Pay payment: {request.amount} {request.currency}")
        
        if not self.validate(request):
            return PaymentResult.failure(
                request.amount, request.currency, self.name,
                "Apple Pay validation failed"
            )
        
        return PaymentResult.success(
            request.amount, request.currency, self.name,
            "Payment completed via Apple Pay"
        )

# Note: No modification to PaymentProcessor or other strategies needed!


# ==================== USAGE EXAMPLE ====================

if __name__ == "__main__":
    # Create payment request
    request = PaymentRequest(
        amount=99.99,
        currency="USD",
        customer_id="cust_123",
        description="Order #12345"
    )
    
    # Use credit card
    cc_strategy = CreditCardStrategy("4111111111111111", "12/25", "123")
    processor = PaymentProcessor(cc_strategy)
    result = processor.process_payment(request)
    print(f"Credit Card: {result.status.value} - {result.message}")
    
    # Switch to PayPal at runtime
    paypal_strategy = PayPalStrategy("user@example.com", "auth_token_123")
    processor.strategy = paypal_strategy
    result = processor.process_payment(request)
    print(f"PayPal: {result.status.value} - {result.message}")
    
    # Use the new Apple Pay
    apple_strategy = ApplePayStrategy("device_token_abc123xyz")
    processor.strategy = apple_strategy
    result = processor.process_payment(request)
    print(f"Apple Pay: {result.status.value} - {result.message}")
```

---

## 📦 Repository Pattern

### When to Use
- Need to abstract data access logic
- Want to swap data sources easily (SQL, NoSQL, files)
- Testing with mock data sources
- Centralizing query logic

### Generation Prompt

```
Act as a Software Architect expert in clean architecture.

Implement the Repository Pattern for a Task management system.

Requirements:
- Abstract repository interface for any entity
- Generic CRUD operations
- Concrete implementation for PostgreSQL with SQLAlchemy
- Support for filtering and pagination
- Transaction management
- Connection pooling
- Unit of Work pattern for transaction handling

Operations:
- add(entity) → entity
- get(id) → entity | None
- get_all(filters, pagination) → List[entity]
- update(entity) → entity
- delete(id) → bool
- find_by_criteria(criteria) → List[entity]

Technical Specs:
- Python 3.11+
- SQLAlchemy 2.0 (async)
- Type hints with generics
- Proper session management

Include:
- Abstract Repository class
- TaskRepository implementation
- Task SQLAlchemy model
- Unit of Work class
- Usage examples
- Unit tests with mock
```

---

## 🏭 Factory Pattern

### When to Use
- Creating objects without specifying exact class
- Object creation logic is complex
- Need flexibility in instantiation

### Generation Prompt

```
Act as a Software Architect.

Implement the Factory Pattern for creating notification services.

Context:
- Support Email, SMS, Push, and Slack notifications
- Each has different configuration requirements
- Want to create the right service based on configuration
- Easily add new notification types

Requirements:
- Abstract Notifier interface
- Concrete implementations for each type
- Factory class with registration mechanism
- Configuration-based creation
- Error handling for unknown types

Include:
- Notifier interface
- All concrete implementations
- NotifierFactory class
- Configuration models
- Usage examples
- Tests
```

---

## 👁️ Observer Pattern

### When to Use
- One-to-many dependency between objects
- When changes in one object need to notify others
- Event-driven architectures
- Decoupling event producers from consumers

### Generation Prompt

```
Act as a Software Architect.

Implement the Observer Pattern for an event notification system.

Context:
- Multiple components need to react to events
- Events include: UserCreated, OrderPlaced, PaymentReceived
- Observers should be able to subscribe/unsubscribe dynamically
- Support async observers
- Type-safe event handling

Requirements:
- Event base class with type safety
- Observable/Subject interface
- Observer interface
- Event dispatcher with topic-based subscription
- Support for priority ordering
- Async event handling

Include:
- Event classes
- Observer interface
- EventDispatcher class
- Example observers
- Usage example showing subscription/unsubscription
- Tests
```

**Expected Pattern:**

```python
"""
Observer Pattern for Event-Driven Architecture.
"""
from abc import ABC, abstractmethod
from dataclasses import dataclass, field
from datetime import datetime
from typing import Dict, List, Type, Callable, Any, Optional
from collections import defaultdict
import asyncio
import logging

logger = logging.getLogger(__name__)


# ==================== EVENTS ====================

@dataclass
class Event:
    """Base class for all events."""
    timestamp: datetime = field(default_factory=datetime.utcnow)
    event_id: str = field(default_factory=lambda: str(uuid.uuid4()))
    
    @property
    def event_type(self) -> str:
        return self.__class__.__name__


@dataclass
class UserCreatedEvent(Event):
    """Event fired when a new user is created."""
    user_id: str = ""
    email: str = ""
    name: str = ""


@dataclass
class OrderPlacedEvent(Event):
    """Event fired when an order is placed."""
    order_id: str = ""
    user_id: str = ""
    total: float = 0.0
    items: List[str] = field(default_factory=list)


@dataclass
class PaymentReceivedEvent(Event):
    """Event fired when payment is received."""
    payment_id: str = ""
    order_id: str = ""
    amount: float = 0.0
    method: str = ""


# ==================== OBSERVER INTERFACE ====================

class Observer(ABC):
    """Abstract observer interface."""
    
    @property
    @abstractmethod
    def priority(self) -> int:
        """Observer priority (lower = higher priority)."""
        pass
    
    @abstractmethod
    async def handle(self, event: Event) -> None:
        """Handle the event."""
        pass


# ==================== CONCRETE OBSERVERS ====================

class EmailNotificationObserver(Observer):
    """Sends email notifications for events."""
    
    @property
    def priority(self) -> int:
        return 10
    
    async def handle(self, event: Event) -> None:
        if isinstance(event, UserCreatedEvent):
            logger.info(f"Sending welcome email to {event.email}")
            # await send_email(event.email, "Welcome!", ...)
        elif isinstance(event, OrderPlacedEvent):
            logger.info(f"Sending order confirmation for {event.order_id}")


class AnalyticsObserver(Observer):
    """Tracks events for analytics."""
    
    @property
    def priority(self) -> int:
        return 50  # Lower priority
    
    async def handle(self, event: Event) -> None:
        logger.info(f"Tracking event: {event.event_type}")
        # await analytics.track(event)


class InventoryObserver(Observer):
    """Updates inventory on order events."""
    
    @property
    def priority(self) -> int:
        return 5  # High priority
    
    async def handle(self, event: Event) -> None:
        if isinstance(event, OrderPlacedEvent):
            logger.info(f"Updating inventory for order {event.order_id}")
            # await update_inventory(event.items)


# ==================== EVENT DISPATCHER ====================

class EventDispatcher:
    """
    Central event dispatcher implementing Observer pattern.
    
    Manages subscriptions and dispatches events to observers.
    """
    
    def __init__(self):
        self._observers: Dict[Type[Event], List[Observer]] = defaultdict(list)
        self._global_observers: List[Observer] = []
    
    def subscribe(
        self, 
        event_type: Type[Event], 
        observer: Observer
    ) -> None:
        """
        Subscribe observer to specific event type.
        
        Args:
            event_type: Type of event to listen for.
            observer: Observer to notify.
        """
        self._observers[event_type].append(observer)
        self._observers[event_type].sort(key=lambda o: o.priority)
        logger.info(f"Observer subscribed to {event_type.__name__}")
    
    def subscribe_all(self, observer: Observer) -> None:
        """Subscribe observer to all events."""
        self._global_observers.append(observer)
        self._global_observers.sort(key=lambda o: o.priority)
    
    def unsubscribe(
        self, 
        event_type: Type[Event], 
        observer: Observer
    ) -> None:
        """Unsubscribe observer from event type."""
        if observer in self._observers[event_type]:
            self._observers[event_type].remove(observer)
            logger.info(f"Observer unsubscribed from {event_type.__name__}")
    
    async def dispatch(self, event: Event) -> None:
        """
        Dispatch event to all subscribed observers.
        
        Args:
            event: Event to dispatch.
        """
        event_type = type(event)
        observers = self._observers.get(event_type, []) + self._global_observers
        
        logger.info(f"Dispatching {event.event_type} to {len(observers)} observers")
        
        # Execute observers concurrently
        tasks = [observer.handle(event) for observer in observers]
        await asyncio.gather(*tasks, return_exceptions=True)


# ==================== USAGE EXAMPLE ====================

async def main():
    # Create dispatcher
    dispatcher = EventDispatcher()
    
    # Create observers
    email_observer = EmailNotificationObserver()
    analytics_observer = AnalyticsObserver()
    inventory_observer = InventoryObserver()
    
    # Subscribe to events
    dispatcher.subscribe(UserCreatedEvent, email_observer)
    dispatcher.subscribe(OrderPlacedEvent, email_observer)
    dispatcher.subscribe(OrderPlacedEvent, inventory_observer)
    dispatcher.subscribe_all(analytics_observer)  # Track all events
    
    # Dispatch events
    await dispatcher.dispatch(UserCreatedEvent(
        user_id="user_123",
        email="user@example.com",
        name="John Doe"
    ))
    
    await dispatcher.dispatch(OrderPlacedEvent(
        order_id="order_456",
        user_id="user_123",
        total=99.99,
        items=["item_1", "item_2"]
    ))
    
    # Unsubscribe
    dispatcher.unsubscribe(OrderPlacedEvent, inventory_observer)


if __name__ == "__main__":
    asyncio.run(main())
```

---

## 💻 Practical Exercises

### Exercise 5.3: Implement Decorator Pattern

**Task:** Implement the Decorator Pattern for a logging/caching system.

**Context:**
- Base interface for data fetchers
- Decorators for: Logging, Caching, Retry logic, Rate limiting
- Stack multiple decorators

**Write your prompt:**
```
[Your prompt here]
```

---

### Exercise 5.4: Implement Builder Pattern

**Task:** Implement the Builder Pattern for creating complex query objects.

**Context:**
- Build SQL-like queries programmatically
- Support SELECT, WHERE, ORDER BY, LIMIT, JOIN
- Fluent interface

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
│  ✅ Strategy Pattern: Interchangeable algorithms                │
│                                                                  │
│  ✅ Repository Pattern: Abstract data access                    │
│                                                                  │
│  ✅ Factory Pattern: Flexible object creation                   │
│                                                                  │
│  ✅ Observer Pattern: Event-driven decoupling                   │
│                                                                  │
│  ✅ Choose patterns based on the problem, not preference        │
│                                                                  │
│  ✅ AI generates implementations, you validate appropriateness  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ Lesson 2 Checklist

Before moving on:
- [ ] Understand when to use Strategy Pattern
- [ ] Can implement Repository Pattern
- [ ] Understand Factory Pattern usage
- [ ] Can apply Observer Pattern
- [ ] Completed Exercises 5.3-5.4

---

## 🔗 Next Steps

Continue to:
- [Lesson 3: Full-Stack Development](./03-full-stack-development.md)

---

**Excellent work! You're mastering design patterns! 🚀**
