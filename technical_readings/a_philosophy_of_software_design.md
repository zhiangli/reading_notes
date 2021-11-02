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

# 7. Different Layer, Different Abstraction
Each layer provides a different abstraction from the layer above and blow it; if you follow a single operation as it moves up and down through
layers by invoking methods, the abstractions change with each method call.
## Pass-through methods
When adjacent layers have similar abstraction, the problem often manifests itself in the form of pass-through methods
## Interface versus implementation
- The representation used internally should be different from the abstractions that appear in the interface
## Pass-through variables
- force all of the intermediate methods to be aware of their existence
- solution: introduce context object
## Conclusion
In order for an element to provide a net gain against complexity, it must eliminate some complexity that would be present in the absence of the
design element. Otherwise, you are better off implementing the system without that particular element.

# 8. Pull Complexity Downwards
It is more important for a module to have a simple interface than a simple implementation
## Configuration parameters
Configuration parameters provide an easy excuse to avoid dealing with important issues. When you do need it, see if you can provide reasonable
default, so user will only need to provide values under exceptional conditions.
## Taking it too far
pulling complexity down makes sense when:
- complexity pulled down is closely related to the class's existing functionality
- pulling complexity down will result in simplifications elsewhere in the application
- pulling complexity down simplifies the class's interface

# 9. Better Together or Better Apart
Two pieces of code are related:
- share information
- used together
- overlap conceptually
- hard to understand one without looking at the other
## Bring together if information is shared
## Bring together if it will simplify the interface
## Bring together to eliminate duplication
## Separate general-purpose and special-purpose code
Special-General Mixture
- a general-purpose mechanism also contains code specialized for a particular use of that mechanism.
- it will make mechanism more complicated and creates information leakage between the mechanism and the particular usage
## Splitting and joining methods
- In general, developers tend to break up methods too much
- You shouldn't break up a method unless it makes the overall system simpler
- Each method should do one thing and do it completely
### Split up methods
- split by extracting a subtask
    - make sense if the subtask is cleanly separable from the rest of the original method
    - Typically, it means the child method is relatively general purpose
- divide its functionality into two separate methods (rarely works)
## Conclusion
The decision to split or join should be based on complexity:
- best information hiding
- fewest dependencies
- deepest interface

# 10. Define Errors out of existence
## Why exceptions add complexity
An exception disrupts normal flow of the code; it usually means something didn't work as expected and an operation cannot be completed as planned.
## Too many exceptions
- if you are having trouble deal with exceptions, there's a good chance that caller won't know how to deal with it either.
- exceptions thrown by a class are part of its interface.
- throwing exception is easy but handling them is hard. Thus, the complexity comes from exception handling code
## Ways to reduce exceptions
### Define errors out of existence
Overall, the best way to reduce bugs is to make software simpler
### Mask exceptions
- An exceptional condition is detected and handled at a low level in the system, so higher levels of software need to be aware of the condition
- This is an example of pulling complexity downwards
### Exception aggregation
- Rather than writing distinct handlers for many individual exceptions, handle them all in one place with a single handler
- Works best if an exception propagates several levels up the stack before it is handled; this allows more exceptions from more methods to be handled in the same place
### Just crash
- There will be certain errors that are not worth trying to handle

# 11. Design it twice
- After you have roughed out the designs for the alternatives, make a list of the pros and cons of each one
- Design it twice principle can be applied at many levels in a system

# 12. Why Write Comments? The Four Excuses
The overall idea behind comments is to capture information that was in the mid of the designer but couldn't be represented in the code

# 13. Comments Should Describe Things that Aren't Obvious from the Code
## Pick conventions
- Interface
- Data structure member
- Cross-module comment
## Don't repeat the code
## Lower-level comments add precision
- Comments augment the code by providing information at a different level of detail
- Comments at the same level as the code are likely to repeat the code
- Precision is most useful when:
    - class instance variables
    - method arguments
    - return values
- Think nouns, not verbs. Focusing on what the variable represents, not how it is manipulated
## Higher-level comments enhance intuition
- Omit details and help the reader to understand the overall intent and structure of the code
- Commonly used for
    - inside methods
    - interface comments
- Step back from the details and think about a system at a higher level. Think about most fundamental characteristics
## Interface documentation
- Document the abstractions with comments
- Interface comments provide information that some needs to know in order to use a class or method; they define the abstraction
- Interface comment needs to include both higher-level information for abstraction and lower-level details for precision
    - starts with a sentence or two describing the behavior -> high level abstraction
    - describe each argument and return value -> precision
    - side effects
    - exceptions
    - preconditions
- Implementation documentation should go inside the method
## Implementation comments: what and why, not how
- The main goal of implementation comments is to help readers understand what the code is doing (not how it does it)
- Also useful to explain why
## Cross-module design decisions
