Engineering Principles — Clean Code Engineering Guide
File: engineering/clean-code.md Version: 1.0

Purpose
This document defines the principles of writing clean, readable, and maintainable code across all languages and systems in this repository.

Clean code standards apply to:

Backend systems
Frontend applications
APIs
Integration layers
Data access
Automation scripts
Philosophy
Clean code is:

Easy to read
Easy to change
Easy to test
Easy to reason about
Free from unnecessary complexity
Code quality is not measured by how much code is written.

It is measured by how easily another engineer can understand and safely modify it.

Core Principles
1. Readability > Cleverness
Write code for humans first, machines second.

Code should communicate intent.

Avoid
Complex One-Liners
Example:

result.stream()
    .filter(x -> x.active)
    .map(x -> x.value)
    .sorted()
    .collect(...)
If the logic becomes difficult to understand, break it apart.

Avoid
Over-optimized code that sacrifices:

Readability
Maintainability
Testability
Optimization should be based on real requirements.

2. Small Units of Work
Every unit should have a clear responsibility.

Functions
A function should:

Do one thing
Have clear inputs
Have clear outputs
Avoid hidden behavior
Classes
A class should:

Represent one concept
Own related behavior
Avoid unrelated responsibilities
Modules
A module should:

Solve one problem
Have clear boundaries
Avoid becoming a dumping ground
3. Naming is Design
Names communicate architecture.

Bad naming usually indicates unclear thinking.

Good Names
Names should:

Reveal intent
Use domain language
Avoid ambiguity
Example:

Good:

calculateInvoiceTotal()
Bad:

processData()
Avoid
Generic names:

data
info
manager
handler
helper
unless the meaning is clear.

4. Avoid Duplication
Follow the DRY principle:

Don't Repeat Yourself

Prefer
Extract:

Shared logic
Common validations
Reusable transformations
Avoid
Copy-paste logic.

Problems:

Bug fixes must happen in multiple places
Behavior becomes inconsistent
5. Minimize Cognitive Load
Code should be:

Linear
Predictable
Easy to scan
Avoid Deep Nesting
Bad:

if

   if

      if

         if
Prefer:

Early returns
Smaller functions
Clear conditions
6. Explicit Over Implicit
Do not hide behavior.

Code should make important decisions visible.

Avoid
Magic Values
Bad:

if(status == 5)
Prefer:

if(status == APPROVED)
Hidden Side Effects
Methods should not unexpectedly modify unrelated state.

Overloaded Functions
A function should have one clear purpose.

Functions
A clean function:

Has one responsibility
Is small (ideally less than 20–30 lines)
Has clear inputs and outputs
Avoids hidden state changes
Function Design
Prefer:

Input

 ↓

Processing

 ↓

Output
Avoid
Functions that:

Validate
Transform
Save
Send notifications
Update unrelated systems
all at once.

Classes
A clean class:

Has one responsibility
Encapsulates behavior and data
Maintains clear boundaries
Avoid
God Objects
A God object knows everything and does everything.

Example:

UserService

- authentication
- email
- database
- reporting
- payments
- notifications
Split responsibilities.

Getters / Setters
Avoid creating classes that only contain:

Fields
Getters
Setters
without meaningful behavior.

Comments
Comments should explain:

WHY
not:

WHAT
Good Comment
Explains:

Business decision
Historical reason
Important constraint
Avoid
Redundant comments:

// increment counter

counter++;
The code already explains itself.

Avoid
Outdated comments.

A wrong comment is worse than no comment.

Error Handling
Good error handling:

Fails fast
Provides context
Preserves debugging information
Rules
Always:

Handle exceptions intentionally
Provide meaningful messages
Preserve root causes
Avoid
Generic swallowing:

catch(Exception e){

}
Problems:

Hidden failures
Difficult debugging
Refactoring
Refactoring should be continuous.

Improve code when changing it.

Refactoring Goals
Remove:

Duplication
Complexity
Poor naming
Unnecessary abstractions
Improve:
Readability
Structure
Testability
Code Smells
Watch for:

Long Methods
Symptoms:

Too many responsibilities
Difficult testing
Large Classes
Symptoms:

Too many dependencies
Too many reasons to change
Deep Nesting
Symptoms:

Complex control flow
Difficult reasoning
Primitive Obsession
Avoid passing meaningless primitives.

Example:

Bad:

createUser(String email)
Better:

createUser(EmailAddress email)
when domain complexity requires it.

Switch Explosion
Large switch statements often indicate missing abstractions.

God Services
Avoid services that contain the entire application.

AI Coding Rule
When generating code:

Prefer clarity over optimization
Break large logic into small functions
Use domain terminology
Avoid unnecessary complexity
Avoid over-engineering
Keep code testable
Design for future change
Engineering Principle Summary
Readable code

        ↓

Maintainable code

        ↓

Reliable systems
