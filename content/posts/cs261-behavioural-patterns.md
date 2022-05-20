---
title: "Behavioural Patterns"
date: 2022-05-20T02:32:22+01:00
draft: false
tags: [ "CS261", "Software Engineering", "Implementation", "Patterns", "Notes" ]
---
# SOLID design principles
- Single responsibility - a single class is responsible for each bit of functionality
- Open for extension, closed for modification - ie. extend a class to add new functionality, not edit the original
- Listov substitution (Behavioural subtyping) - Allow a child object to masquerade as it's parent for all purporses
- Interface segregation - Have many specific interfaces, not a few overgeneralised ones
- Dependency inversion - Instead of making high-level classes depend on low-level ones, have them both depend on well-defined interfaces

# Behavioural Patterns

## Iterator
- Object which traverses a container to access it's elements
- Allows containers to be accessed without exposing their inner workings
- Iterators can be defined for any data-containing structure

## Observer
- Allows dependent objects to "subscribe" to object state changes
- Dependents get notified that a state has changed, rather than forcing it to constantly poll for changes

# Memento
- Allows for undo/redo behaviour
- Keeps object "snapshots" in a caretaker object
- Caretaker can be accessed to restore snapshots later

# Strategy
- Methods to complete task implemented in seperate object
- Original context object doesn't know details of strategies, just selects one to use
- Good example is a graph iterator - strategies can be implemented seperately (eg. depth first, breadth first) and selected based on conditions of the graph at runtime

[All topics ⟶](/posts/cs261-index/)
