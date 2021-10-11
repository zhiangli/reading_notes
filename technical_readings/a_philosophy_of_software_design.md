A Philosophy of Software Design
John Ousterhout

# Introduction
## Two ways to fight complexity
- Eliminate complexity by making code simpler and more obvious
- Encapsulate it
    - Module design: modules are designed to be relatively independent with each other

# The Nature of Complexity
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

# Working Code Isn't Enough
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
