---
title: "Architectural Design"
date: 2022-05-20T02:32:22+01:00
draft: false
tags: [ "CS261", "Software Engineering", "Design", "Patterns", "Notes" ]
---
# Why?
To understand how a system should be organised.
- Conceptual Integrity
- Quality driven eg. fault tolerance, maintainability, backwards compatability
- Recurring architecture styles
- Separation of concerns to reduce complexity

# System overview
Represented with box/arrow diagrams. Arrows show direction of data/control flow, can also show breakdown of larger subsystems.

{{<figure src="/overview.png" height=300 title="Architectural overview">}}

# Layered structure
Each layer relies on the one below only, and provides services to the one above only.

{{<figure src="/layered.png" height=300 title="Layered architecture">}}

# Repository structure
All subsystem interaction done via repository.

{{<figure src="/repository.png" height=200 title="Repository architecture">}}

# Pipe & Filter structure
Linear data pathways formed, components process & filter the data to correct outputs.

{{<figure src="/pipefilter.png" height=200 title="Pipe & Filter architecture">}}

# Modev-view-controller (MVC) structure
Focuses on interpreting user actions (controller -> model), updating the data (model), and presenting it to the user (view).

{{<figure src="/mvc.png" height=300 title="Model-view-controller architecture">}}

[All topics ⟶](/posts/cs261-index/)
