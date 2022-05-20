---
title: "System Design"
date: 2022-05-20T01:39:37+01:00
draft: false
tags: [ "CS261", "Software Engineering", "Design", "Notes" ]
---
# UML
UML diagrams model aspects of our system.
- Structural diagrams illustrate objects, attributes, and operations.
  - Profile, Class, Composite structure, Component, Deployment, Object and Package diagrams.
- Behavioural diagrams illustrate dynamic system behaviour.
  - Activity, Use case, State machine, Sequence, Communication, Interaction overview and Timing diagrams.

# Context Model
Points to systems our system interacts with.

{{<figure src="/contextmodel.png" height=200 title="Context model UML Diagram">}}

# Structural Diagrams

## Class Diagram
Static structure of system objects.

{{<figure src="/class.png" height=200 title="Class UML Diagram">}}

| Symbol | Meaning               |
|--------|-----------------------|
| +      | Public                |
| -      | Private               |
| #      | Protected             |
| /      | Derived               |
| _      | Static                |
| ~      | Package (for methods) |

### Correct class diagrams
- Abstract classes' names should be in *italics*
- Interface names should be prefixed with \<\<interface\>\>
- **Do not** include getters, setters or inherited methods.
- **Do** include interface methods

Heirarchy: Draw arrow from child to parent class.
| Pointing to    | Style                        |
|----------------|------------------------------|
| Class          | Black arrowhead              |
| Abstract Class | White Arrowhead              |
| Interface      | White arrowhead, dotted line |

Association between classes.
| Association | Example               |
|--------|-----------------------|
| Aggregation (part of) | {{<figure src="/aggregation.png" height=70 >}} |
| Composition (made up of) | {{<figure src="/composition.png" height=70 >}} |
| Dependency (used temporarily) | {{<figure src="/dependency.png" height=70 >}} |

- In aggregation, one may exist without the other
- For composition, this is not true; stronger association

Additionally, write 1 or \* on associations to represent 1 to many etc. relations.

# Behavioural Diagrams

## Activity Diagram
Represents workflows of stepwise activities, like a flowchart.

{{<figure src="/activity.png" height=350 title="Activity UML Diagram">}}

## State machine Diagram
System state transitions, like a finite state machine.

{{<figure src="/statemachine.png" height=250 title="State machine UML Diagram">}}

There is a correspondance between activity & state machine diagrams. Actions in state machine corerspond to boxes in activity.

## Use-case Diagram
Interactions of actors with events.

{{<figure src="/usecase.png" height=300 title="Use-case UML Diagram">}}

## Sequence Diagram
Communications between objects.

{{<figure src="/sequence.png" height=300 title="Sequence UML Diagram">}}

- A box represents an active object (i.e. running code)
- Nested boxes indicate recursion
- To make notes on a sequence diagram, draw a lil' sticky note

Object naming schemes:
| Object type             | Naming scheme | Example       |
|-------------------------|---------------|---------------|
| Named object            | Name:Class    | Smith:Patient |
| Anonymous object        | :Class        | :Patient      |
| Object of unknown class | Name          | Smith         |
