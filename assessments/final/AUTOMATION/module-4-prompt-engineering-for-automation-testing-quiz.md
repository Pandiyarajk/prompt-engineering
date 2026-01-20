================================
FINAL ASSESSMENT – AUTOMATION
================================

Module 4: Prompt Engineering for Automation Testing

========== QUESTION SHEET ==========
Q1.
Which is the most important constraint when asking an AI to generate test automation code?
A. Provide no framework information
B. Specify language, framework, and target interfaces
C. Avoid defining locators or selectors
D. Require the longest possible code

Q2.
Which is a best practice for AI-generated automated tests?
A. Prefer brittle sleeps over explicit waits
B. Use stable selectors and deterministic assertions
C. Assert only that the page loaded
D. Avoid negative cases

Q3.
Which statement best reflects maintainable test design?
A. Duplicate setup code in every test
B. Use reusable helpers/fixtures and clear separation of concerns
C. Put all logic in one large test
D. Hardcode environment-specific values

Q4.
What is a key reason to prefer explicit waits (or condition-based waits) over fixed delays?
A. Fixed delays are always faster
B. Condition-based waits reduce flakiness and improve stability
C. Explicit waits require no assertions
D. Fixed delays improve determinism

Q5.
Which practice best supports CI reliability for automation?
A. Non-deterministic tests are acceptable if they pass locally
B. Tests should be hermetic where possible and avoid external dependencies
C. Increase randomness to find more bugs
D. Skip reporting artifacts on failure

Q6.
Which is a correct approach to data management in automated tests?
A. Use shared mutable data across tests without reset
B. Isolate test data and clean up to avoid cross-test pollution
C. Never seed data
D. Only use production data

Q7.
Which statement about “test pyramid” is correct?
A. UI tests should be the majority because they are most realistic
B. Prefer more unit/component tests and fewer end-to-end UI tests
C. Only end-to-end tests are useful
D. The pyramid is about code formatting rules

Q8.
Which output from an AI assistant is most appropriate to trust “as-is” in automation work?
A. Direct claims about system behavior without evidence
B. Boilerplate scaffolding that is verified by running and reviewing
C. Assertions that cannot be executed
D. Credentials embedded in code

Q9.
Which is a best practice to reduce flaky assertions in UI automation?
A. Assert on stable business outcomes rather than transient UI states
B. Assert on exact timestamps displayed on screen
C. Rely on visual layout pixel-perfect checks only
D. Avoid assertions altogether

Q10.
Which is an important security consideration when using AI tools for automation code?
A. Paste secrets into prompts to speed up work
B. Avoid sharing credentials and sensitive environment details
C. Store tokens in source control
D. Disable reviews for generated code
