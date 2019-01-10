# Test Driven Development in Angularjs Project

[Test-driven development (TDD)](https://technologyconversations.com/2014/09/30/test-driven-development-tdd/) is a software development process that relies on the repetition of a very short development cycle: first the developer writes an (initially failing) automated test case that defines a desired improvement or new function, then produces the minimum amount of code to pass that test, and finally refactors the new code to acceptable standards.

The following sequence of steps is generally followed:

- Add a test
- Run all tests and see if the new one fails
- Write some code
- Run tests
- Refactor code
- Repeat

## Level 1. Unit Testing

The core units which make up features should be verified with accompanying unit tests. In JavaScript apps, the smallest units of code you can test are usually individual functions.

## Recommended Unit Testing Tools

1. Jasmine
2. Mocha
3. Qunit

## When is a good time to write Unit Tests?

It's easy for a semi-technical manager to say *"write all tests at the start"*, but the truth is, you often don't know all the small components which are required when presented with a high-level user story.

I usually write unit tests as I go along, as I develop all the 'units' required to implement a user story. You're going to constantly be changing functions around, refactoring and abstracting parts away, tidying up what's going in and coming out of functions, trying to preempt any of this is going to cause a lot of wasted time.