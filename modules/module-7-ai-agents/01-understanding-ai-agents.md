# Module 7: AI Agents and Automation

## Lesson 1: Understanding AI Agents

### What are AI Agents?

An **AI Agent** is an autonomous system that:
1. Perceives its environment
2. Makes decisions
3. Takes actions to achieve goals
4. Learns from results
5. Operates with minimal human intervention

### AI Agent vs Traditional AI

**Traditional AI (ChatGPT):**
```
User: "Write test cases for login"
AI: [Generates test cases]
User: "Now write automation code"
AI: [Generates code]
(Manual steps required)
```

**AI Agent:**
```
User: "Create and execute automated tests for login feature"
Agent: 
  1. Analyzes requirements
  2. Generates test cases
  3. Writes automation code
  4. Executes tests
  5. Analyzes results
  6. Reports findings
  7. Suggests improvements
(Autonomous execution)
```

### Types of AI Agents for Testing

#### 1. **Code Generation Agents**

**Capabilities:**
- Generate test automation code
- Create page objects
- Build test frameworks
- Write utilities

**Example Tools:**
- GitHub Copilot
- Cursor AI
- Tabnine
- Amazon CodeWhisperer

**Prompt Example:**
```
Agent Goal: Create a complete Selenium test framework

Agent Actions:
1. Create folder structure
2. Generate base page class
3. Create page objects for each page
4. Generate test fixtures
5. Write sample tests
6. Create configuration files
7. Add README documentation
```

#### 2. **Test Execution Agents**

**Capabilities:**
- Run tests autonomously
- Analyze failures
- Retry flaky tests
- Collect evidence
- Generate reports

**Use Cases:**
- CI/CD pipeline automation
- Continuous testing
- Regression testing
- Smoke testing

**Prompt Example:**
```
Agent Goal: Execute test suite and analyze failures

Agent Actions:
1. Execute all tests
2. Capture screenshots/logs for failures
3. Analyze error messages
4. Classify failures (bug vs flaky vs environment)
5. Create bug reports for real issues
6. Re-run flaky tests
7. Generate executive summary
```

#### 3. **Bug Analysis Agents**

**Capabilities:**
- Analyze error logs
- Identify root causes
- Suggest fixes
- Prioritize bugs
- Link related issues

**Prompt Example:**
```
Agent Goal: Analyze test failures and suggest fixes

Agent Actions:
1. Read error logs
2. Identify error patterns
3. Search codebase for related code
4. Analyze recent changes (git history)
5. Suggest root cause
6. Provide fix recommendations
7. Estimate severity and priority
```

#### 4. **Test Design Agents**

**Capabilities:**
- Analyze requirements
- Generate test strategies
- Create test cases
- Identify edge cases
- Suggest test improvements

**Prompt Example:**
```
Agent Goal: Create comprehensive test strategy for new feature

Agent Actions:
1. Read feature specifications
2. Identify all user workflows
3. Extract functional requirements
4. Generate test scenarios
5. Create test cases
6. Map to requirements
7. Identify gaps in coverage
8. Generate test data requirements
```

### Building Custom AI Agents

#### Agent Architecture

```
┌─────────────────────────────────────┐
│          User Input/Goal             │
└─────────────┬───────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│       Planning Component             │
│  (Break goal into sub-tasks)         │
└─────────────┬───────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│      Execution Component             │
│  (Execute tasks using tools)         │
│                                      │
│  Tools:                              │
│  - Code execution                    │
│  - File operations                   │
│  - API calls                         │
│  - Web scraping                      │
│  - Database queries                  │
└─────────────┬───────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│      Memory Component                │
│  (Remember context & results)        │
└─────────────┬───────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│      Evaluation Component            │
│  (Check if goal achieved)            │
└─────────────┬───────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│         Output/Result                │
└─────────────────────────────────────┘
```

#### Example: Test Case Generator Agent

**Python Implementation:**

```python
import openai
import os

class TestCaseGeneratorAgent:
    """
    AI Agent that autonomously generates test cases
    """
    
    def __init__(self, api_key):
        self.api_key = api_key
        openai.api_key = api_key
        self.memory = []
        
    def analyze_requirements(self, requirements):
        """Step 1: Analyze requirements"""
        prompt = f"""
        Analyze these requirements and extract:
        1. Key features
        2. User workflows
        3. Edge cases
        4. Data requirements
        
        Requirements: {requirements}
        """
        
        response = self._call_ai(prompt)
        self.memory.append({"step": "analysis", "result": response})
        return response
    
    def generate_test_scenarios(self, analysis):
        """Step 2: Generate test scenarios"""
        prompt = f"""
        Based on this analysis, generate test scenarios:
        {analysis}
        
        Include:
        - Positive scenarios
        - Negative scenarios
        - Edge cases
        - Security scenarios
        """
        
        response = self._call_ai(prompt)
        self.memory.append({"step": "scenarios", "result": response})
        return response
    
    def create_test_cases(self, scenarios):
        """Step 3: Create detailed test cases"""
        prompt = f"""
        Convert these scenarios into detailed test cases:
        {scenarios}
        
        Format as table:
        | Test ID | Description | Steps | Expected Result | Priority |
        """
        
        response = self._call_ai(prompt)
        self.memory.append({"step": "test_cases", "result": response})
        return response
    
    def validate_coverage(self, test_cases, requirements):
        """Step 4: Validate requirement coverage"""
        prompt = f"""
        Check if these test cases cover all requirements:
        
        Requirements: {requirements}
        Test Cases: {test_cases}
        
        Identify:
        - Gaps in coverage
        - Missing scenarios
        - Suggestions for improvement
        """
        
        response = self._call_ai(prompt)
        self.memory.append({"step": "validation", "result": response})
        return response
    
    def run(self, requirements):
        """Execute the agent workflow"""
        print("🤖 Agent: Starting test case generation...")
        
        # Step 1: Analyze
        print("📊 Analyzing requirements...")
        analysis = self.analyze_requirements(requirements)
        
        # Step 2: Generate scenarios
        print("🎯 Generating test scenarios...")
        scenarios = self.generate_test_scenarios(analysis)
        
        # Step 3: Create test cases
        print("📝 Creating detailed test cases...")
        test_cases = self.create_test_cases(scenarios)
        
        # Step 4: Validate
        print("✅ Validating coverage...")
        validation = self.validate_coverage(test_cases, requirements)
        
        print("✨ Agent: Complete!")
        
        return {
            "test_cases": test_cases,
            "validation": validation,
            "memory": self.memory
        }
    
    def _call_ai(self, prompt):
        """Helper to call OpenAI API"""
        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[
                {"role": "system", "content": "You are a senior QA engineer."},
                {"role": "user", "content": prompt}
            ],
            temperature=0.3
        )
        return response.choices[0].message.content

# Usage
if __name__ == "__main__":
    agent = TestCaseGeneratorAgent(api_key=os.getenv("OPENAI_API_KEY"))
    
    requirements = """
    Feature: User Registration
    - User enters email, password, first name, last name
    - Password must be 8+ characters
    - Email must be unique
    - Send verification email
    - User must verify email to login
    """
    
    result = agent.run(requirements)
    print("\n" + "="*50)
    print("RESULT:")
    print("="*50)
    print(result["test_cases"])
    print("\n" + "="*50)
    print("VALIDATION:")
    print("="*50)
    print(result["validation"])
```

### Agent Frameworks and Tools

#### 1. **LangChain**

**Use Case:** Building complex AI agents with chains

```python
from langchain.agents import initialize_agent, Tool
from langchain.agents import AgentType
from langchain.llms import OpenAI

# Define tools
tools = [
    Tool(
        name="Test Case Generator",
        func=generate_test_cases,
        description="Generates test cases from requirements"
    ),
    Tool(
        name="Code Generator",
        func=generate_code,
        description="Generates automation code"
    ),
    Tool(
        name="Test Executor",
        func=execute_tests,
        description="Executes test scripts"
    )
]

# Initialize agent
llm = OpenAI(temperature=0)
agent = initialize_agent(
    tools,
    llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

# Run agent
agent.run("Create and execute automated tests for login feature")
```

#### 2. **AutoGPT / BabyAGI**

**Autonomous agents that:**
- Set their own goals
- Plan execution
- Execute tasks
- Learn from results

**Use Case for Testing:**
```
Goal: "Achieve 100% test coverage for user module"

Agent autonomous steps:
1. Analyze existing tests
2. Identify untested code
3. Generate new test cases
4. Write automation code
5. Execute tests
6. Check coverage
7. Repeat until 100%
```

#### 3. **Custom Agent with GPT-4**

**Advanced Pattern:**

```python
class AutonomousTestAgent:
    """
    Autonomous testing agent that can:
    - Plan test strategy
    - Generate tests
    - Execute tests
    - Analyze results
    - Fix issues
    """
    
    def __init__(self):
        self.memory = []
        self.tools = {
            "generate_tests": self.generate_tests,
            "execute_tests": self.execute_tests,
            "analyze_results": self.analyze_results,
            "fix_test": self.fix_test
        }
    
    def plan(self, goal):
        """Create execution plan"""
        prompt = f"""
        Goal: {goal}
        
        Create a step-by-step plan to achieve this goal.
        Available tools: {list(self.tools.keys())}
        
        Format:
        1. [tool_name]: [description]
        2. [tool_name]: [description]
        ...
        """
        
        plan = self._call_ai(prompt)
        return self._parse_plan(plan)
    
    def execute_plan(self, plan):
        """Execute the plan"""
        for step in plan:
            tool = self.tools[step["tool"]]
            result = tool(step["params"])
            self.memory.append({
                "step": step,
                "result": result
            })
            
            # Check if goal achieved
            if self._goal_achieved():
                break
        
        return self.memory
    
    def _goal_achieved(self):
        """Evaluate if goal is achieved"""
        # Implementation
        pass
```

### Practical AI Agent Use Cases

#### Use Case 1: Continuous Test Maintenance Agent

**Goal:** Keep tests updated as application changes

**Agent Workflow:**
```
1. Monitor code repository for changes
2. Identify affected tests
3. Analyze test failures
4. Update test code automatically
5. Create PR with changes
6. Notify team
```

#### Use Case 2: Intelligent Test Selector

**Goal:** Run only relevant tests for code changes

**Agent Workflow:**
```
1. Analyze code diff
2. Identify affected features
3. Find related test cases
4. Prioritize tests by risk
5. Execute high-priority tests
6. Report results
```

#### Use Case 3: Automated Bug Triage

**Goal:** Automatically analyze and categorize bugs

**Agent Workflow:**
```
1. Read new bug reports
2. Analyze error logs
3. Search for similar bugs
4. Classify (new vs duplicate)
5. Assign severity/priority
6. Suggest owner
7. Add to sprint backlog
```

### Exercise 7.1: Build a Simple Agent

**Task:** Create an agent that generates and validates test cases

**Requirements:**
```python
class SimpleTestAgent:
    def run(self, feature_requirements):
        """
        1. Generate test cases
        2. Check for duplicates
        3. Validate coverage
        4. Return final test suite
        """
        pass
```

Your implementation:
```
[Write your agent code here]
```

### Best Practices for AI Agents

✅ **DO:**
- Define clear goals and constraints
- Implement error handling
- Add human-in-the-loop for critical decisions
- Log all agent actions
- Set limits (time, iterations, cost)
- Validate agent outputs
- Use appropriate tools for tasks

❌ **DON'T:**
- Let agents run without supervision
- Give unlimited access/permissions
- Skip validation of agent decisions
- Ignore costs (API usage)
- Use for critical production decisions without review

### Key Takeaways

✅ AI Agents operate autonomously to achieve goals
✅ Agents plan, execute, and learn from results
✅ Different agent types for different purposes
✅ Can be built with frameworks or custom code
✅ Useful for test generation, execution, and maintenance
✅ Always include human oversight
✅ Monitor costs and set appropriate limits

### Next Steps
Move to Lesson 2: Building Testing Agents with LangChain
