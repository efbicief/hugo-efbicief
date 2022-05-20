---
title: "Testing"
date: 2022-05-20T10:42:34+01:00
draft: false
tags: [ "CS261", "Software Engineering", "Testing", "Notes" ]
---
# Static testing
Testing without executing, with code reviews, walkthroughs, inspections, linting.\
But why?
- Errors hide other errors
- Code doesn't need to be complete to test statically (compilers need not apply)
- Can consider code quality

# Dynamic testing
Executing with test cases.

## Structural/white-box testing
Tests derived from control/data flow of system.
- Statement adequacy: All statements are tested.
- Statement coverage: Executed statements / Total statements
- Path coverage: achieving coverage by exploring all state transitions of a system. Deriving a program's control flow helps to derive tests for a system.

## Functional/black-box testing
Tests derived from formal component specification.
  1. Identify functions you expect system to be able to perform
  2. Create input data, and define expected outputs
  3. Run test cases
  4. Compare output data with expected data
  5. Check the application works as per customer needs

# Testing granularity

## Unit tests
- Ensuring all objects and methods are working.
- Must be done for all objects, so automate as much as possible!
- Aim for effectiveness: when used as expected, action performs as intended

## Component/integration tests
Testing interactions between unit components. Types of interface errors to look for:
- Interface misuse: a component is passing wrong parameters, or receiving an unexpected return value
- Interface misunderstanding: a component doesn't understand the behaviour of another one
- Timing errors

## System testing
Looking for emergant behaviour in a complete system.
- Use case testing is useful, test for expected and unexpected emergant behaviour
- Force interactions to occur between components in the system

Exhaustive testing is impossible, so we make test cases which:
- Execute each statement ≥1 time
- Tests all functions via all menus/interfaces
- USes correct and incorrect test data/user input

# Test-driven development
Originally introduced with XP, gained popularity across the board.
1. Identify an increment, write an automated test for the new feature
2. Run it - it will fail
3. Implement the functionality and rerun the test, and repeat until the test passes

# User testing
- Alpha: Small set of users working with the development team very early on
- Beta: Much larger group of users testing an almost-complete system
- Acceptance: Customer deciding whether the system is ready to be deployed

{{<figure src="/acceptance.png" height=150 title="Acceptance testing flow">}}

[All topics ⟶](/posts/cs261-index/)
