A Philosophy of Software Design
John Ousterhout

# 1. Introduction
## Two ways to fight complexity
- Eliminate complexity by making code simpler and more obvious
- Encapsulate it
    - Module design: modules are designed to be relatively independent with each other

# 2. The Nature of Complexity
## Definition
- Complexity is anything related to the structure of a software system that makes it hard to understand and modify the system
- Overall complexity = complexity of each part * fraction of time spent working in that part
## Symptoms of complexity
- Change amplification
- Cognitive load
- Unknown unknowns

## Cause of complexity
- Dependencies
    - dependencies is not avoidable but the goal of software design is to reduce dependencies
- Obscurity
    - important information is not obvious
    - inconsistency
    - inadequate documentation
    
## Complexity is incremental
It makes it hard to control. Easy to convince little bit of complexity is okay

# 3. Working Code Isn't Enough
## Tactical programming
- Main focus is to get something working. 
- short-sighted. 
## Strategic programming
- Working code is not enogh
- Your primary goal must be to produce a great design, which also happens to work
- Slow you down a bit in the short term but speed you up in the long term
## How much to invest
- Make lots of small investments on a continual basis
- By programming tactically you are borrowing time from the future

# 4. Modules Should Be Deep
## Modular design
Module in two parts: interface and implementation

- Software system is decomposed into a collection of modules that are relatively independent.
- The goal of modular design is to minimize the dependencies between modules
- The best modules are those whose interfaces are much simpler than their implementations
## What is an interface
- formal and informal parts
## Abstractions
- An abstraction is a simplified view of an entity, which omits unimportant details
## Deep modules
The best modules are deep: they have a lot of functionality hidden behind a simple interface
## Shallow modules
The interface is relatively complex in comparison to the functionality it provides

*Interfaces should be designed to make the common case as simple as possible*

# 5. Information Hiding (and Leakage)
## Information hiding
- each module should encapsulate a few pieces of knowledge, which represent design decision

Information hiding reduces complexity:
- simplify the interface to a module
- easier to involve the system
## Information leakage
Information leakage occurs when a design decision is reflected in multiple modules.
Tactics:
- If the affected classes are relatively small and closed tied to the leaked information, merge them into a single class
- pull the information out of all the affected classes and create a new class that encapsulates just that information
## Temporal decomposition
When designing modules, focus on the *knowledge* that's needed to perform each task, not the order in which tasks occur
## HTTP server example
### Information hiding can often be improved by making a class slightly larger
- bring together all the code related to a particular capability
- raise the level of the interface
### Defaults
- Interface should be designed to make the common case as simple as possible
- Partial information hiding: normal case, the caller need not be aware of the existence of the defaulted item
## Information hiding within a class
## Taking it too far
If information is needed outside the module, then you must not hide it

# 6. General-Purpose Modules are Deeper
## Make classes somewhat general-purpose
The module's functionality should reflect your current needs, but its interface should not. Instead, the interface should be general enough to 
support multiple uses.
## Questions to ask yourself
- What is the simplest interface that will cover all my current needs?
Reducing the number of methods makes sense only as long as the API for each individual method stays simple
- In how many situations will this method be used?
If a method is designed for one particular use, this is the red flag that it may be too special-purpose
- Is this API easy to use for my current needs?
## Push specialization upwards (and downwards!)
Specialized code should be cleanly separated from general-purpose code. This can be done by pushing the specialized code either up or down in
the software stack
- The suggestion to separate general-purpose code from special-purpose code refers to code related to a particular mechanism
## Eliminate special cases in code
- The best way to do this is by designing the normal case in a way that automatically handles the edge condition without any extra code
