================================
FINAL ASSESSMENT – AUTOMATION
================================

Module 2: Prompt Engineering Fundamentals

========== QUESTION SHEET ==========
Q1.
What does “temperature” primarily control?
A. Maximum input length
B. Degree of randomness/creativity in token sampling
C. Model size on disk
D. Network timeout

Q2.
What is the main purpose of “top_p” (nucleus sampling)?
A. Limit output to a fixed character count
B. Sample from the smallest token set whose cumulative probability is p
C. Force deterministic decoding
D. Increase context window size

Q3.
Which setting most directly limits response length?
A. max_tokens (or equivalent)
B. temperature
C. top_p
D. frequency_penalty

Q4.
Which approach best improves reliability for structured outputs?
A. Ask for “a detailed answer” without constraints
B. Specify a fixed schema/sections and forbid extra text
C. Increase temperature significantly
D. Avoid using examples

Q5.
What is the primary goal of iterative prompting?
A. Avoid specifying constraints
B. Refine instructions based on observed outputs
C. Increase model training data
D. Guarantee correctness without review

Q6.
Which is a best practice to reduce hallucinations in factual tasks?
A. Require citations even when none are available
B. Provide the relevant source text and constrain answers to it
C. Use only vague instructions
D. Increase temperature

Q7.
Which is an effective way to prevent scope creep in an automation-related request?
A. Combine multiple deliverables in one question
B. State explicit inclusions/exclusions
C. Avoid acceptance criteria
D. Ask for “everything about automation”

Q8.
What is a key tradeoff of overly long prompts?
A. They always reduce cost
B. They can dilute priorities and reduce clarity
C. They increase context window size
D. They guarantee better accuracy

Q9.
Which technique is most aligned with “few-shot” prompting?
A. Providing several labeled examples of desired input-output behavior
B. Setting temperature to 0
C. Increasing max_tokens
D. Removing all constraints

Q10.
Which is a correct principle for instruction design?
A. Implicit requirements are safer than explicit ones
B. Constraints should be testable and unambiguous
C. Output format should be left open-ended
D. Conflicting goals improve accuracy
