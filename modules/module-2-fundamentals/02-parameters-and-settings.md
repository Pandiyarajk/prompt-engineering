# Module 2: Fundamentals of Prompt Engineering

## Lesson 2: Parameters and Temperature Settings

---

## 🎯 Lesson Overview

**Time Required:** 45-60 minutes  
**Difficulty:** Beginner to Intermediate  
**Prerequisites:** Lesson 1 (Basic Prompt Structure) completed

In this lesson, you'll learn:
- What AI parameters are and why they matter
- How temperature affects AI responses
- When to use high vs low settings
- Optimal configurations for different tasks
- How to use parameters even in the ChatGPT web interface

---

## 🎚️ What Are AI Parameters?

When using AI models (especially via API), you can adjust settings that control how the AI generates responses. Think of them like dials on a mixing board - each one affects the output in specific ways.

```
┌─────────────────────────────────────────────────────────────────┐
│  AI PARAMETERS                                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  🌡️ TEMPERATURE ──────────────────────── Randomness/Creativity   │
│      0.0 ════════════════════════════ 2.0                       │
│      Focused                        Random                      │
│                                                                  │
│  📏 MAX TOKENS ───────────────────────── Response Length         │
│      100 ════════════════════════════ 4000                      │
│      Short                          Long                        │
│                                                                  │
│  🎯 TOP P ────────────────────────────── Token Selection         │
│      0.1 ════════════════════════════ 1.0                       │
│      Narrow                         Wide                        │
│                                                                  │
│  🔄 FREQUENCY PENALTY ────────────────── Reduce Repetition       │
│      -2.0 ═══════════════════════════ 2.0                       │
│      Repeat                         Avoid                       │
│                                                                  │
│  ✨ PRESENCE PENALTY ─────────────────── Encourage Novelty       │
│      -2.0 ═══════════════════════════ 2.0                       │
│      Stay on topic                  Explore                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🌡️ Temperature: The Most Important Parameter

### What Temperature Does

Temperature controls **randomness** in AI responses. It determines how "creative" or "focused" the AI will be.

```
LOW TEMPERATURE (0.0 - 0.3)          HIGH TEMPERATURE (0.7 - 1.5)
┌────────────────────────┐           ┌────────────────────────┐
│ • Consistent           │           │ • Creative             │
│ • Predictable          │           │ • Varied               │
│ • Focused              │           │ • Unpredictable        │
│ • Deterministic        │           │ • Diverse              │
│ • Same output each run │           │ • Different each run   │
└────────────────────────┘           └────────────────────────┘

Best for:                            Best for:
• Code generation                    • Brainstorming
• Test cases                         • Creative writing
• Documentation                      • Edge case ideas
• Factual answers                    • Alternative approaches
```

### Visual Example

```
Prompt: "Give me 3 names for a testing utility function"

Temperature 0.1 (Run 3 times):
  Run 1: validate_input, check_value, verify_data
  Run 2: validate_input, check_value, verify_data
  Run 3: validate_input, check_value, verify_data
  ─────────────────────────────────────────────────
  Result: Same answers each time (consistent)

Temperature 1.0 (Run 3 times):
  Run 1: verify_input, data_checker, assertion_helper
  Run 2: input_validator, test_evaluator, value_inspector
  Run 3: quality_checker, param_analyzer, data_validator
  ─────────────────────────────────────────────────
  Result: Different answers each time (creative)
```

### Temperature Guidelines

| Temperature | Range | Use For | Example Tasks |
|-------------|-------|---------|---------------|
| **Very Low** | 0.0-0.2 | Precise, consistent output | Code generation, test scripts, documentation |
| **Low** | 0.2-0.4 | Reliable with slight variation | Test cases, bug analysis, structured output |
| **Medium** | 0.4-0.7 | Balanced creativity/focus | General questions, explanations, summaries |
| **High** | 0.7-1.0 | Creative, diverse output | Brainstorming, edge cases, alternatives |
| **Very High** | 1.0-1.5 | Maximum creativity | Exploration, unusual ideas, experimentation |

### When to Use Each Temperature

#### Use LOW Temperature (0.1-0.3) For:

✅ **Code Generation**
```python
# API call example with low temperature
response = client.chat.completions.create(
    model="gpt-4",
    messages=[...],
    temperature=0.2  # Consistent, working code
)
```
- Test automation scripts
- Unit tests
- API clients
- Utility functions

✅ **Documentation**
- README files
- API documentation
- Test plans
- Technical specifications

✅ **Structured Output**
- Test case tables
- Bug reports
- JSON/XML generation
- Database queries

#### Use HIGH Temperature (0.7-1.0) For:

✅ **Brainstorming**
```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[...],
    temperature=0.9  # Creative, varied ideas
)
```
- Edge case identification
- Security vulnerability ideas
- Test scenario exploration
- Alternative solutions

✅ **Creative Tasks**
- Naming suggestions
- Multiple approaches
- Unusual test scenarios
- "What could go wrong?" analysis

---

## 📏 Max Tokens: Response Length

### What Max Tokens Does

Max tokens limits the **length** of the AI response. Roughly:
- 1 token ≈ 4 characters
- 1 token ≈ 0.75 words
- 100 tokens ≈ 75 words

### Token Guidelines

| Setting | Tokens | Use For |
|---------|--------|---------|
| **Short** | 50-200 | Quick answers, single items |
| **Medium** | 200-500 | Explanations, short code |
| **Long** | 500-1000 | Detailed responses, test suites |
| **Very Long** | 1000-4000 | Complete code files, comprehensive docs |

### Examples

```python
# Short response (quick test case summary)
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "List 5 login test scenarios"}],
    max_tokens=200
)

# Long response (complete test suite)
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Create complete Selenium test file"}],
    max_tokens=2000
)
```

### ⚠️ Important Notes

- **Token limit includes both input AND output**
- If response is cut off, increase max_tokens
- Longer responses = higher API costs
- ChatGPT web interface handles this automatically

---

## 🎯 Top P (Nucleus Sampling)

### What Top P Does

Top P is an alternative to temperature for controlling randomness. It controls what percentage of possible words the AI considers.

```
Top P = 0.1
  • Only considers top 10% most likely words
  • Very focused output
  • Similar to low temperature

Top P = 1.0
  • Considers all possible words
  • Full range of output
  • Default setting
```

### ⚠️ Important Rule

**Use EITHER temperature OR top_p, not both!**

```python
# CORRECT - Use temperature
response = client.chat.completions.create(
    model="gpt-4",
    messages=[...],
    temperature=0.3
)

# CORRECT - Use top_p (with temperature=1)
response = client.chat.completions.create(
    model="gpt-4",
    messages=[...],
    temperature=1,
    top_p=0.1
)

# INCORRECT - Don't use both
response = client.chat.completions.create(
    model="gpt-4",
    messages=[...],
    temperature=0.3,
    top_p=0.5  # Don't do this!
)
```

### When to Use Top P vs Temperature

| Situation | Use | Setting |
|-----------|-----|---------|
| Need consistent code | Temperature | 0.2 |
| Need creative ideas | Temperature | 0.8 |
| Need very focused output | Top P | 0.1 |
| General usage | Temperature | 0.7 (default) |

---

## 🔄 Frequency and Presence Penalties

### Frequency Penalty

Controls how much the AI avoids repeating the same words.

```
Frequency Penalty = 0: Normal repetition allowed
Frequency Penalty = 0.5: Some repetition discouraged
Frequency Penalty = 1.0: Strong avoidance of repetition
```

**Use case:** Generating diverse test cases

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Generate 20 different test scenarios"}],
    frequency_penalty=0.7  # Avoid repetitive scenarios
)
```

### Presence Penalty

Encourages the AI to talk about new topics.

```
Presence Penalty = 0: Stay on current topics
Presence Penalty = 0.5: Introduce some new topics
Presence Penalty = 1.0: Actively explore new topics
```

**Use case:** Exploring different testing angles

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "What are different ways to test this?"}],
    presence_penalty=0.6  # Explore diverse approaches
)
```

---

## 📊 Optimal Settings for QA Tasks

### Quick Reference Table

| Task | Temp | Max Tokens | Freq Penalty | Pres Penalty |
|------|------|------------|--------------|--------------|
| Code generation | 0.2 | 2000 | 0.2 | 0 |
| Test case creation | 0.3 | 1500 | 0.3 | 0 |
| Bug analysis | 0.3 | 1000 | 0.2 | 0 |
| Edge case brainstorming | 0.9 | 800 | 0.7 | 0.6 |
| Test strategy | 0.5 | 1200 | 0.4 | 0.3 |
| Code review | 0.3 | 1500 | 0.3 | 0.2 |
| Documentation | 0.4 | 1000 | 0.3 | 0 |
| Brainstorming | 1.0 | 1000 | 0.8 | 0.7 |

### Configuration Presets

#### Preset 1: Precise Code Generation

```python
config_code = {
    "temperature": 0.2,
    "max_tokens": 2000,
    "top_p": 1,
    "frequency_penalty": 0.2,
    "presence_penalty": 0
}
# Use for: Test scripts, automation code, utilities
```

#### Preset 2: Detailed Test Cases

```python
config_test_cases = {
    "temperature": 0.3,
    "max_tokens": 1500,
    "frequency_penalty": 0.3,
    "presence_penalty": 0
}
# Use for: Test case generation, test plans
```

#### Preset 3: Creative Brainstorming

```python
config_creative = {
    "temperature": 0.9,
    "max_tokens": 1000,
    "frequency_penalty": 0.8,
    "presence_penalty": 0.6
}
# Use for: Edge cases, security ideas, exploration
```

#### Preset 4: Balanced Analysis

```python
config_analysis = {
    "temperature": 0.5,
    "max_tokens": 1200,
    "frequency_penalty": 0.4,
    "presence_penalty": 0.3
}
# Use for: Code review, bug analysis, strategy
```

---

## 💬 Using Parameters in ChatGPT Web Interface

If you're using the ChatGPT web interface (not API), you can't directly set parameters. However, you can **influence** the behavior through your prompt:

### For More Consistent/Focused Output:

```
Be precise and deterministic in your answer.
Give me the exact same format every time.
Be very specific and consistent.
Provide only one option, the most standard approach.
```

### For More Creative/Varied Output:

```
Think creatively and suggest unusual approaches.
Give me diverse and varied ideas.
Brainstorm multiple different solutions.
What are some unconventional ways to...?
Consider edge cases that others might miss.
```

### Example Prompts

**For Code Generation (Low Temp Effect):**
```
Act as a Senior Python Developer.
Write a function to validate email addresses.

Be precise and provide the standard, production-ready implementation.
Follow conventional patterns. No creative alternatives.
```

**For Brainstorming (High Temp Effect):**
```
Act as a Creative QA Engineer.
What are unusual ways users might break a search feature?

Think outside the box. Consider unconventional scenarios.
Give me ideas that most testers wouldn't think of.
```

---

## 💻 Practical Examples

### Example 1: Generate Test Cases (Low Temperature)

```python
import os
from openai import OpenAI

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

def generate_test_cases(feature_description: str) -> str:
    """Generate consistent test cases for a feature."""
    
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[
            {
                "role": "system",
                "content": "You are a meticulous Senior QA Engineer who creates comprehensive, structured test cases."
            },
            {
                "role": "user",
                "content": f"""
                Generate test cases for: {feature_description}
                
                Include positive, negative, and edge cases.
                Format as a markdown table.
                """
            }
        ],
        temperature=0.2,      # Low for consistency
        max_tokens=1500,
        frequency_penalty=0.3
    )
    
    return response.choices[0].message.content

# Usage
feature = "User login with email and password"
test_cases = generate_test_cases(feature)
print(test_cases)
```

### Example 2: Brainstorm Edge Cases (High Temperature)

```python
def brainstorm_edge_cases(feature_description: str) -> str:
    """Generate creative edge cases for testing."""
    
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[
            {
                "role": "system",
                "content": "You are a creative security tester who thinks of unusual ways things can break."
            },
            {
                "role": "user",
                "content": f"""
                Think of unusual edge cases that could break: {feature_description}
                
                Be creative. Consider scenarios that normal testing might miss.
                Include security, performance, and usability edge cases.
                """
            }
        ],
        temperature=0.9,       # High for creativity
        max_tokens=1000,
        frequency_penalty=0.8,  # Avoid repetitive ideas
        presence_penalty=0.6   # Explore different angles
    )
    
    return response.choices[0].message.content

# Usage
feature = "File upload with drag and drop"
edge_cases = brainstorm_edge_cases(feature)
print(edge_cases)
```

### Example 3: Compare Temperatures

```python
def compare_temperatures(prompt: str):
    """Run the same prompt with different temperatures to see the difference."""
    
    temperatures = [0.2, 0.7, 1.2]
    
    for temp in temperatures:
        print(f"\n{'='*50}")
        print(f"Temperature: {temp}")
        print('='*50)
        
        response = client.chat.completions.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}],
            temperature=temp,
            max_tokens=300
        )
        
        print(response.choices[0].message.content)

# Usage
compare_temperatures("Name 5 test scenarios for a shopping cart feature")
```

---

## 💻 Exercise: Temperature Experimentation

### Exercise 2.2: Test Temperature Effects

**Step 1:** Use this prompt with different temperatures

```
Suggest 5 creative names for a test automation framework
```

**Step 2:** Try temperatures: 0.2, 0.5, 1.0

**Step 3:** Document your observations:

| Temperature | Observations |
|-------------|--------------|
| 0.2 | Names are: _______, similarity level: _______ |
| 0.5 | Names are: _______, similarity level: _______ |
| 1.0 | Names are: _______, similarity level: _______ |

**Step 4:** Run each temperature 3 times. Are results consistent?

### Exercise 2.3: Find Optimal Settings

**Task:** For each scenario, determine the optimal temperature:

| Scenario | Your Recommended Temp | Why? |
|----------|----------------------|------|
| Generate pytest code for login tests | ___ | ___ |
| Find security vulnerabilities | ___ | ___ |
| Create API documentation | ___ | ___ |
| Brainstorm mobile app test ideas | ___ | ___ |
| Write SQL queries for test data | ___ | ___ |

---

## 🔧 Troubleshooting Common Issues

### Problem: Response is too similar every time
```python
# Solution: Increase temperature
temperature = 0.8  # Instead of 0.2
frequency_penalty = 0.6  # Add some variation
```

### Problem: Code is inconsistent or broken
```python
# Solution: Decrease temperature
temperature = 0.1  # More consistent
top_p = 0.1  # Very focused
```

### Problem: Response is cut off
```python
# Solution: Increase max_tokens
max_tokens = 3000  # Instead of 1000
```

### Problem: Too much repetition
```python
# Solution: Increase penalties
frequency_penalty = 0.7
presence_penalty = 0.5
```

### Problem: Response is too random/weird
```python
# Solution: Lower temperature
temperature = 0.3  # Instead of 1.0+
```

---

## 📝 Key Takeaways

1. **Temperature** is the most important parameter - controls creativity vs consistency
2. **Low temperature (0.1-0.3)** for code, test cases, documentation
3. **High temperature (0.7-1.0)** for brainstorming, edge cases, creativity
4. **Max tokens** controls response length - increase if cut off
5. **Frequency/presence penalties** reduce repetition
6. **Use either temperature OR top_p, not both**
7. In web interface, use **prompt wording** to influence behavior

---

## ✅ Lesson 2 Checklist

Before moving on, ensure you can:

- [ ] Explain what temperature does
- [ ] Choose appropriate temperature for different tasks
- [ ] Understand max tokens and its purpose
- [ ] Know when to use frequency/presence penalties
- [ ] Use prompts to influence ChatGPT web interface
- [ ] Complete all exercises

---

## 🔗 Next Steps

You've mastered parameters! 🎉

**Continue to:** [Lesson 3: Best Practices and Optimization](./03-best-practices.md)

---

## 📖 Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────┐
│  PARAMETERS QUICK REFERENCE                                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  🌡️ TEMPERATURE                                                  │
│     0.0-0.3: Code, documentation, consistent output             │
│     0.4-0.7: Balanced (default)                                 │
│     0.7-1.0: Brainstorming, creative ideas                      │
│                                                                  │
│  📏 MAX TOKENS                                                   │
│     Short answer: 200-500                                       │
│     Medium: 500-1000                                            │
│     Long/code: 1000-2000                                        │
│                                                                  │
│  🔄 FREQUENCY PENALTY (0 to 2)                                   │
│     Higher = less repetition                                    │
│                                                                  │
│  ✨ PRESENCE PENALTY (0 to 2)                                    │
│     Higher = more topic variety                                 │
│                                                                  │
│  ⚠️ REMEMBER: Temperature OR top_p, not both!                   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

**Great progress! Let's learn best practices next! 🚀**
