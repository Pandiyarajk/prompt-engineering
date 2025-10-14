# Module 2: Fundamentals of Prompt Engineering

## Lesson 2: Parameters and Temperature Settings

### Understanding AI Model Parameters

When using AI models via API, you can control several parameters that affect the output. Understanding these helps you get more consistent and appropriate results.

### Key Parameters

#### 1. **Temperature**

**What it controls:** Randomness and creativity of responses

**Range:** 0.0 to 2.0
- **0.0** - Deterministic, focused, consistent
- **0.7** - Balanced (default for most use cases)
- **1.5+** - Creative, varied, unpredictable

**For Testing & QA:**
```python
# Test case generation - use low temperature for consistency
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Generate login test cases"}],
    temperature=0.2  # More focused and consistent
)

# Brainstorming edge cases - use higher temperature
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "What unusual edge cases might break a login?"}],
    temperature=0.9  # More creative and diverse
)
```

**Recommended Temperature Settings:**

| Task | Temperature | Reason |
|------|-------------|--------|
| Code generation | 0.2 - 0.3 | Need consistent, working code |
| Test case creation | 0.3 - 0.5 | Want thoroughness but consistency |
| Bug analysis | 0.2 - 0.4 | Need focused, accurate diagnosis |
| Edge case brainstorming | 0.7 - 1.0 | Want creative, diverse ideas |
| Documentation | 0.3 - 0.5 | Need clear, consistent language |
| Code review | 0.2 - 0.4 | Want focused, specific feedback |

#### 2. **Max Tokens**

**What it controls:** Maximum length of the response

**Typical values:**
- Short answers: 100-500 tokens
- Medium responses: 500-1000 tokens
- Long content: 1000-4000 tokens

**Note:** ~4 characters = 1 token (approximate)

**Example:**
```python
# Quick test case summary
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Summarize key test scenarios for login"}],
    max_tokens=200  # Brief summary
)

# Detailed test automation code
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Create complete Selenium test suite"}],
    max_tokens=2000  # Comprehensive code
)
```

#### 3. **Top P (Nucleus Sampling)**

**What it controls:** Alternative to temperature for controlling randomness

**Range:** 0.0 to 1.0
- **0.1** - Very focused, considers only top 10% probable tokens
- **0.5** - Balanced
- **1.0** - Considers all possibilities

**Best Practice:** Use either temperature OR top_p, not both

```python
# Using top_p for code generation
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Generate pytest fixture"}],
    top_p=0.1,  # Very focused
    temperature=1  # Set to 1 when using top_p
)
```

#### 4. **Frequency Penalty**

**What it controls:** Reduces repetition of tokens

**Range:** -2.0 to 2.0
- **Negative values** - Encourage repetition
- **0** - No penalty
- **Positive values** - Discourage repetition

```python
# Generating diverse test cases
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Create 20 different login test scenarios"}],
    frequency_penalty=0.7  # Encourage variety
)
```

#### 5. **Presence Penalty**

**What it controls:** Encourages new topics

**Range:** -2.0 to 2.0
- **Negative values** - Stick to current topics
- **0** - Neutral
- **Positive values** - Explore new topics

```python
# Exploring different testing approaches
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Suggest testing strategies"}],
    presence_penalty=0.6  # Encourage diverse approaches
)
```

### Practical Examples for QA & Development

#### Example 1: Generating Test Cases (Consistent)

```python
import openai

def generate_test_cases(feature_description):
    """
    Generate consistent test cases
    Using low temperature for reproducibility
    """
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {
                "role": "system",
                "content": "You are a meticulous QA engineer who creates comprehensive test cases."
            },
            {
                "role": "user",
                "content": f"Generate test cases for: {feature_description}"
            }
        ],
        temperature=0.2,  # Low for consistency
        max_tokens=1500,
        top_p=1,
        frequency_penalty=0.3,  # Some variety in wording
        presence_penalty=0
    )
    return response.choices[0].message.content

# Usage
test_cases = generate_test_cases("user profile update feature")
print(test_cases)
```

#### Example 2: Brainstorming Edge Cases (Creative)

```python
def brainstorm_edge_cases(feature):
    """
    Brainstorm creative edge cases
    Using higher temperature for diversity
    """
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {
                "role": "system",
                "content": "You are a creative QA engineer who thinks of unusual edge cases."
            },
            {
                "role": "user",
                "content": f"What unusual edge cases could break this feature: {feature}"
            }
        ],
        temperature=0.9,  # High for creativity
        max_tokens=800,
        frequency_penalty=0.8,  # Avoid repetitive ideas
        presence_penalty=0.6  # Encourage exploring new angles
    )
    return response.choices[0].message.content

# Usage
edge_cases = brainstorm_edge_cases("file upload with drag and drop")
print(edge_cases)
```

#### Example 3: Code Generation (Precise)

```python
def generate_test_code(test_scenario, language="python"):
    """
    Generate precise, working code
    Using very low temperature
    """
    response = openai.ChatCompletion.create(
        model="gpt-4",  # GPT-4 for better code quality
        messages=[
            {
                "role": "system",
                "content": f"You are an expert test automation engineer writing {language} code."
            },
            {
                "role": "user",
                "content": f"Write test automation code for: {test_scenario}"
            }
        ],
        temperature=0.1,  # Very low for precise code
        max_tokens=2000,
        top_p=0.1  # Very focused
    )
    return response.choices[0].message.content

# Usage
code = generate_test_code("API test for user login endpoint using pytest and requests")
print(code)
```

### Parameter Combinations for Different Tasks

#### Configuration 1: Deterministic Test Documentation
```python
{
    "temperature": 0.2,
    "max_tokens": 1000,
    "top_p": 1,
    "frequency_penalty": 0,
    "presence_penalty": 0
}
# Use for: Test plans, bug reports, test case templates
```

#### Configuration 2: Creative Problem Solving
```python
{
    "temperature": 0.8,
    "max_tokens": 1000,
    "frequency_penalty": 0.7,
    "presence_penalty": 0.6
}
# Use for: Finding edge cases, security vulnerabilities, performance issues
```

#### Configuration 3: Code Generation
```python
{
    "temperature": 0.3,
    "max_tokens": 2000,
    "top_p": 0.2,
    "frequency_penalty": 0.2,
    "presence_penalty": 0
}
# Use for: Test scripts, automation code, API clients
```

#### Configuration 4: Diverse Test Scenarios
```python
{
    "temperature": 0.6,
    "max_tokens": 1500,
    "frequency_penalty": 0.8,
    "presence_penalty": 0.5
}
# Use for: Multiple test scenarios, different testing approaches
```

### ChatGPT Web Interface

**Note:** When using ChatGPT web interface (chat.openai.com), you cannot directly control these parameters, but you can influence them through prompts:

**For more consistent responses:**
```
"Provide a precise, deterministic answer..."
"Give me the exact same format every time..."
"Be very specific and consistent..."
```

**For more creative responses:**
```
"Think creatively and suggest unusual approaches..."
"Give me diverse and varied ideas..."
"Brainstorm multiple different solutions..."
```

### Testing Parameter Settings

**Create a test script to experiment:**

```python
import openai
import os

openai.api_key = os.getenv("OPENAI_API_KEY")

def test_temperature_variations():
    """Test how temperature affects output"""
    prompt = "Generate 3 test cases for a search feature"
    
    temperatures = [0.2, 0.7, 1.2]
    
    for temp in temperatures:
        print(f"\n{'='*50}")
        print(f"Temperature: {temp}")
        print('='*50)
        
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[{"role": "user", "content": prompt}],
            temperature=temp
        )
        
        print(response.choices[0].message.content)
        print()

# Run the test
test_temperature_variations()
```

### Exercise 2.2: Parameter Experimentation

**Task:** Test different parameter settings

1. **Test Case Generation:**
   - Run the same prompt 3 times with temperature=0.2
   - Run the same prompt 3 times with temperature=1.0
   - Compare the consistency and variety

2. **Code Generation:**
   - Generate Python code with temperature=0.1
   - Generate the same code with temperature=0.8
   - Compare code quality and variation

3. **Optimal Settings:**
   - Find the best temperature for generating API test cases
   - Find the best temperature for finding security vulnerabilities
   - Document your findings

### Best Practices

✅ **DO:**
- Use low temperature (0.2-0.4) for code generation
- Use low temperature for consistent test cases
- Use higher temperature (0.7-1.0) for brainstorming
- Experiment to find optimal settings for your use case
- Set appropriate max_tokens to avoid truncated responses

❌ **DON'T:**
- Use both temperature and top_p together
- Use very high temperature (>1.5) for production code
- Ignore token limits (responses may be cut off)
- Use same settings for all tasks

### Troubleshooting

**Problem:** Responses are too similar/boring
```python
# Solution: Increase temperature and frequency_penalty
temperature=0.8
frequency_penalty=0.6
```

**Problem:** Code is inconsistent or broken
```python
# Solution: Decrease temperature
temperature=0.2
top_p=0.1
```

**Problem:** Response is cut off
```python
# Solution: Increase max_tokens
max_tokens=3000
```

**Problem:** Too much repetition
```python
# Solution: Increase frequency_penalty
frequency_penalty=0.7
presence_penalty=0.5
```

### Key Takeaways

✅ Temperature controls randomness (low = consistent, high = creative)
✅ Use low temperature (0.2-0.3) for code and test cases
✅ Use higher temperature (0.7-1.0) for brainstorming
✅ Max tokens controls response length
✅ Frequency/presence penalties reduce repetition
✅ Experiment to find optimal settings for each task type
✅ Different tasks need different parameter configurations

### Quick Reference Card

```
Task Type              | Temp  | Max Tokens | Freq Pen | Pres Pen
-----------------------|-------|------------|----------|----------
Code Generation        | 0.2   | 2000       | 0.2      | 0
Test Cases (Detailed)  | 0.3   | 1500       | 0.3      | 0
Bug Analysis           | 0.3   | 1000       | 0.2      | 0
Edge Case Ideas        | 0.9   | 800        | 0.7      | 0.6
Test Strategy          | 0.5   | 1200       | 0.4      | 0.3
Code Review            | 0.3   | 1500       | 0.3      | 0.2
Documentation          | 0.4   | 1000       | 0.3      | 0
Brainstorming          | 1.0   | 1000       | 0.8      | 0.7
```

### Next Steps
Move to Lesson 3: Best Practices and Prompt Optimization
