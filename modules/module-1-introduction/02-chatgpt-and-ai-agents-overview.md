# Module 1: Introduction to Prompt Engineering

## Lesson 2: ChatGPT and AI Agents Overview

### Understanding ChatGPT

**What is ChatGPT?**
- Large Language Model (LLM) developed by OpenAI
- Trained on vast amounts of text data
- Can understand and generate human-like text
- Useful for coding, testing, writing, and problem-solving

**ChatGPT Versions:**
- **GPT-3.5**: Faster, good for simple tasks
- **GPT-4**: More capable, better reasoning, slower
- **GPT-4 Turbo**: Balance of speed and capability

### AI Agents vs. Simple Chatbots

**Simple Chatbot:**
```
User → Question → AI → Answer
```

**AI Agent:**
```
User → Goal → AI Agent → Plans → Actions → Tools → Result
```

### Types of AI Agents for QA and Development

#### 1. **Code Generation Agents**
- Generate code from requirements
- Create test scripts
- Build automation frameworks

**Example Use Case:**
```
"Create a Selenium test script in Python to:
1. Open https://example.com
2. Login with test credentials
3. Verify dashboard loads
4. Take screenshot if test fails"
```

#### 2. **Test Design Agents**
- Create test cases
- Generate test scenarios
- Design test strategies

**Example Use Case:**
```
"Design a complete test strategy for an e-commerce checkout flow including:
- Cart management
- Payment processing
- Order confirmation
- Email notifications"
```

#### 3. **Bug Analysis Agents**
- Analyze error logs
- Suggest root causes
- Recommend fixes

**Example Use Case:**
```
"Analyze this error log and suggest potential root causes:
[Error log content here]"
```

#### 4. **Code Review Agents**
- Review code for bugs
- Suggest improvements
- Check best practices

**Example Use Case:**
```
"Review this Python function for:
- Security vulnerabilities
- Performance issues
- Code quality
- Best practices

[Code here]"
```

### Popular AI Tools for QA and Development

| Tool | Best For | Key Features |
|------|----------|--------------|
| ChatGPT | General purpose, coding, testing | Conversational, versatile |
| GitHub Copilot | Code completion | IDE integration |
| Cursor | Code editing | AI-powered IDE |
| Tabnine | Code completion | Privacy-focused |
| Amazon CodeWhisperer | AWS development | AWS integration |
| Jasper/Anthropic Claude | Long context tasks | Large context window |

### How AI Agents Work

```
┌─────────────────┐
│   Your Prompt   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Understanding  │ ← Context from conversation
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│    Planning     │ ← Breaking down the task
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│    Execution    │ ← Generating response
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│    Response     │
└─────────────────┘
```

### Practical Example: Testing with AI Agents

**Scenario:** You need to test a login feature

**Traditional Approach:**
1. Manually think of test cases (30 min)
2. Write test cases in Excel (45 min)
3. Create automation script (2 hours)
4. Total: ~3.25 hours

**AI-Assisted Approach:**
1. Prompt AI for test cases (2 min)
2. Review and refine (10 min)
3. Prompt AI for automation script (2 min)
4. Review and customize (20 min)
5. Total: ~34 minutes

**Time Saved:** ~2.5 hours (85% faster!)

### Limitations and Considerations

⚠️ **AI is not perfect:**
- Can make mistakes
- May generate outdated code
- Needs human verification
- Context limitations

✅ **Best used for:**
- Brainstorming
- First drafts
- Repetitive tasks
- Learning and exploration
- Code scaffolding

❌ **Not recommended for:**
- Critical security code (without review)
- Final production code (without testing)
- Replacing human judgment
- Confidential/sensitive data

### Exercise 2.1: Compare AI Tools

Try the same prompt with different AI tools:

**Prompt:**
```
Generate a Python function to validate email addresses with unit tests
```

**Try with:**
1. ChatGPT
2. GitHub Copilot (if available)
3. Any other AI tool you have access to

**Compare:**
- Response quality
- Code correctness
- Test coverage
- Execution time

### Key Takeaways

✅ AI Agents are more autonomous than simple chatbots
✅ Different AI tools serve different purposes
✅ AI can dramatically speed up QA and development tasks
✅ Always verify AI-generated content
✅ AI is a tool to augment, not replace, human expertise

### Next Steps
Move to Lesson 3: Setting Up Your Environment
