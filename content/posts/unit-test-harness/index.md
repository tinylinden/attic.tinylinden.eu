---
title: "Unit Test Harness"
date: 2024-10-13T21:37:09+02:00
draft: true
tags: ["tdd","toolbox","kotlin"]
summary: "What makes it quick, easy and fun to write tests"
---

## How would you test this piece of code?

That was the question I heard a few days ago. It wasn't a general one that 
could be answered briefly with "well, you know -- use the given--when--then 
(aka. [arrange--act--assert][arrange-act-assert]) pattern and just do it". 
It was a question about very specific and rather complex piece of code.

I am aware that such question should trigger a discussion on how to refactor 
the code to make it more test-friendly. But that is a topic for another story. 
For now, let's focus on **what makes it quick, easy and fun to write tests**. 
Even for complicated use case which operate on complex data structures and 
communicate with several dependencies. Because in so-called real life we rarely
check if 42 is equal to 42.

{{< figure
    src="template.png"
    alt="given-when-then"
    caption="Dead simple test pattern"
>}}

So the original question about how to test something, has evolved into a more
general one. **What is needed to reduce each test to a dead simple pattern?**
Without multi-line, unreadable blobs configuring half the universe at the 
beginning. And similarly confusing constructions wrapping around the end
like a boa constrictor, completely suffocating any sense of meaning.

Let's deep dive, breaking down the steps.

## 1. Given -- Prepare Input and Dependencies

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore
eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt
in culpa qui officia deserunt mollit anim id est laborum.

### Fixtures -- Test Data Factories

{{< figure
    src="fixtures.png"
    alt="..."
    caption="..."
    class="dark"
>}}

### Traits -- Dependency Providers and Configurators

Traits can be found in Scala and Groovy, and as described [here][groovy-traits]
they are considered as interfaces, carrying default implementation and state.

{{< figure
    src="traits.png"
    alt="..."
    caption="..."
>}}

## 3. Then -- Check if Actual Result Meets Expectations

{{< figure
    src="assertions.png"
    alt="assertions"
    caption="..."
>}}

...

[groovy-traits]: https://www.baeldung.com/groovy-traits
[arrange-act-assert]: https://automationpanda.com/2020/07/07/arrange-act-assert-a-pattern-for-writing-good-tests
