# Module 6: Advanced Techniques

## Lesson 1: Chain-of-Thought Prompting

---

## 🎯 Lesson Overview

**Time Required:** 45 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Module 2 completed

Chain-of-Thought (CoT) prompting is one of the most powerful techniques for improving AI reasoning. By asking AI to show its work step-by-step, you get more accurate, transparent, and reliable results.

---

## 📚 What is Chain-of-Thought Prompting?

Chain-of-Thought prompting involves asking the AI to break down complex problems into smaller steps and **show its reasoning process**. Instead of jumping directly to an answer, the AI explains how it arrived at its conclusion.

### Why Does This Work?

```
┌─────────────────────────────────────────────────────────────────┐
│  HOW CHAIN-OF-THOUGHT IMPROVES RESULTS                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  WITHOUT CoT:                                                    │
│  ┌─────────────────────────────────────────┐                   │
│  │  Question  ──────────►  Answer           │                   │
│  │                        (often wrong)     │                   │
│  └─────────────────────────────────────────┘                   │
│                                                                  │
│  AI jumps to conclusion, may miss important factors             │
│                                                                  │
│                                                                  │
│  WITH CoT:                                                       │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Question → Step 1 → Step 2 → Step 3 → ... → Answer     │   │
│  │              ↓         ↓         ↓          (accurate)  │   │
│  │          [Check]   [Check]   [Check]                    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Each step can be verified, errors caught early                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔑 Key Benefits

| Benefit | Description |
|---------|-------------|
| **Better Accuracy** | Breaking problems down leads to fewer errors |
| **Transparency** | You can see HOW the AI reached its conclusion |
| **Debuggable** | Easy to identify where reasoning went wrong |
| **Educational** | Learn problem-solving approaches |
| **Verifiable** | Each step can be checked |

---

## 📝 Two Types of Chain-of-Thought

### 1. Zero-Shot CoT (The Magic Phrase)

Simply add **"Let's think step by step"** to your prompt:

```
What are all the edge cases we should test for a file upload feature?

Let's think step by step.
```

This simple addition can improve results by 30-50% on complex tasks!

### 2. Structured CoT (Explicit Steps)

Provide a specific framework for the AI to follow:

```
Analyze this bug report step by step:

Step 1: Identify the root cause
Step 2: Determine affected components
Step 3: Suggest test cases to reproduce
Step 4: Propose fixes
Step 5: Recommend regression tests

Bug Report: [description]
```

---

## 💡 Practical Examples

### Example 1: Test Case Generation

**❌ Without Chain-of-Thought:**
```
Generate test cases for a login form.
```

**Result:** May miss edge cases, inconsistent coverage.

**✅ With Chain-of-Thought:**
```
Generate test cases for a login form. Think through the process step by step:

1. First, identify all input fields and their constraints
   (username, password, remember me, etc.)

2. For each field, list valid and invalid input types
   (correct format, empty, too long, special characters)

3. Consider boundary conditions
   (minimum/maximum lengths, edge values)

4. Think about user journey scenarios
   (first-time user, returning user, locked account)

5. Consider security aspects
   (SQL injection, XSS, brute force)

6. Think about integration points
   (session management, 2FA, SSO)

Now provide test cases organized by these categories.
```

**Result:** Comprehensive, well-organized test coverage.

---

### Example 2: Debugging Code

**❌ Without Chain-of-Thought:**
```
What's wrong with this code?

def calculate_discount(price, discount_percent):
    return price - (price * discount_percent)
```

**✅ With Chain-of-Thought:**
```
Analyze this code for bugs step by step:

def calculate_discount(price, discount_percent):
    return price - (price * discount_percent)

Step 1: Understand what the function should do
Step 2: Check the math/logic
Step 3: Consider edge cases (what inputs could break it?)
Step 4: Check for type issues
Step 5: Consider security/performance
Step 6: List all bugs found with fixes
```

**AI Response:**
```
Step 1: Function should calculate discounted price given original 
        price and discount percentage.

Step 2: Math check - if discount_percent is 10 (meaning 10%),
        the formula gives: price - (price * 10) = negative!
        The formula assumes decimal (0.10) not percentage (10).

Step 3: Edge cases:
        - price = 0: Works (returns 0)
        - price < 0: Logic error (negative prices?)
        - discount_percent = 0: Works
        - discount_percent = 100: Returns 0 (correct)
        - discount_percent > 100: Returns negative (bug!)
        - discount_percent < 0: Increases price (bug or feature?)

Step 4: Type issues:
        - No validation that inputs are numbers
        - Could crash with strings

Step 5: No security/performance issues for simple function

Step 6: Bugs found:
        1. Ambiguous discount_percent (decimal vs percentage)
        2. No input validation
        3. No bounds checking

Fixed version:
def calculate_discount(price: float, discount_percent: float) -> float:
    if not isinstance(price, (int, float)) or price < 0:
        raise ValueError("Price must be non-negative number")
    if not 0 <= discount_percent <= 100:
        raise ValueError("Discount must be between 0 and 100")
    return price * (1 - discount_percent / 100)
```

---

### Example 3: Root Cause Analysis

```
A user reports: "The checkout page freezes when I click 'Place Order'"

Analyze this issue step by step:

Step 1: Gather more information
- What browser/device?
- Does it happen every time?
- When did it start?
- Any error messages?

Step 2: Form hypotheses
Based on common causes of page freezing:
- JavaScript error stopping execution
- Infinite loop
- Large synchronous operation blocking UI
- Memory leak
- Network request timing out

Step 3: For each hypothesis, identify how to test it
...

Step 4: Prioritize investigation order (most likely first)
...

Step 5: Suggest quick wins that might resolve the issue
...
```

---

## 🎯 When to Use Chain-of-Thought

| Use CoT For | Don't Need CoT For |
|-------------|-------------------|
| Complex reasoning | Simple questions |
| Multi-step problems | Direct facts |
| Debugging | Basic lookups |
| Analysis tasks | Simple formatting |
| Decision making | List generation |
| Code review | Translation |

---

## 📐 Chain-of-Thought Templates

### Template 1: General Problem Solving

```
[Problem description]

Let's solve this step by step:

1. First, let's understand the problem
2. Identify the key components
3. Consider possible approaches
4. Evaluate each approach
5. Choose the best solution
6. Implement and verify

Show your reasoning for each step.
```

### Template 2: Code Analysis

```
Analyze this code step by step:

[Code block]

1. What is this code supposed to do?
2. Walk through the logic line by line
3. What are the inputs and outputs?
4. What could go wrong? (edge cases, errors)
5. Are there any bugs or issues?
6. How could it be improved?

Provide detailed analysis.
```

### Template 3: Test Case Design

```
Design test cases for [feature] step by step:

1. List all functional requirements
2. Identify input fields and their constraints
3. Define equivalence classes for each input
4. Identify boundary values
5. Consider error conditions
6. Think about integration points
7. Consider security scenarios

Generate test cases based on this analysis.
```

### Template 4: Bug Investigation

```
Investigate this bug step by step:

Bug: [description]

1. Clarify the expected vs actual behavior
2. Identify possible root causes
3. For each cause, how would we confirm it?
4. What's the most likely cause?
5. What's the fix?
6. What regression tests do we need?
```

---

## 💻 Practical Exercises

### Exercise 6.1: Apply Chain-of-Thought

**Task 1:** Transform this basic prompt into a CoT prompt:
```
Basic: "What security tests should we run on a REST API?"
```

**Your CoT Prompt:**
```
[Write your improved prompt here]
```

---

**Task 2:** Use CoT to debug this function:
```python
def is_palindrome(s):
    return s == s[::-1]
```

Hint: What edge cases might this miss?

---

**Task 3:** Apply CoT to generate a test plan:
```
Feature: Password reset functionality

Requirements:
- User clicks "Forgot Password"
- Enters email
- Receives reset link (expires in 24h)
- Creates new password (8+ chars)

Use Chain-of-Thought to create a comprehensive test plan.
```

---

## ⚠️ Common Mistakes

| Mistake | How to Fix |
|---------|------------|
| Too few steps | Add more granular steps |
| Steps too vague | Make each step specific |
| Skipping verification | Add "verify your reasoning" step |
| Not using for complex tasks | Apply to all multi-step problems |

---

## 📝 Key Takeaways

```
┌─────────────────────────────────────────────────────────────────┐
│  CHAIN-OF-THOUGHT KEY TAKEAWAYS                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✅ Add "Let's think step by step" for quick improvement        │
│                                                                  │
│  ✅ Use structured steps for complex problems                   │
│                                                                  │
│  ✅ Each step should be verifiable                              │
│                                                                  │
│  ✅ Great for debugging, analysis, and problem-solving          │
│                                                                  │
│  ✅ Ask AI to verify its reasoning at the end                   │
│                                                                  │
│  ✅ Can improve accuracy by 30-50% on complex tasks            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔗 Next Lesson

Continue to [Lesson 2: Few-Shot Learning](./02-few-shot-learning.md) to learn how to teach AI through examples!

---

**You're mastering advanced techniques! 🚀**
