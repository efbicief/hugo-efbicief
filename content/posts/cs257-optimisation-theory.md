---
title: "Code Optimisation - Theory"
date: 2022-06-06T10:22:41+01:00
draft: false
tags: [ "CS257", "Architecture", "Optimisation", "Notes" ]
---
# Metrics
- Floating point operations per second
  - Peak FLOPs: Maximum attainable, assuming no cost to memory operations.
  - Peak GFLOPs = Clock speed (GHz) * Cores * FLOPs/cycle
- Speedup = unoptimised time / optimised time

# Optimisation approaches
- Algorithmic: Implementing a more efficient algorithm eg. using merge sort instead of bubble sort
- Code refactoring: Making changes to convince the compiler to apply optimisations.
  - Removing interloop dependencies: Making a loop only rely on the current iteration can allow for parallelisation.
    - Pointer aliasing: As long as a pointer alias isn't hiding an interloop dependency, you can use the *restrict* keyword in declarations to tell the compiler that each iteration can be independent.
  - Loop peeling: Prerequisite for other optimisations. Take eg. the first iteration outside of the loop.
  - Loop interchange: Make memory accesses be in sequential order by eg. swapping order of rows and columns in iteration. Lets all relevant data be stored in cache simultaneously.
  - Loop blocking: Rearrange loops to improve cache-reuse of a block of memory eg. break a large matrix into smaller ones.
  - Loop fusion: Merge two loops traversing over the same range into one loop.
  - Loop fission: Improve data locality by splitting apart loops.
  - Loop unrolling: Make there be fewer iterations, make each iteration perform the function of several.
  - Loop pipelining: reorder independent operations within a loop to allow for pipelining.

{{<figure src="/looppipeline.png" height=350 title="Loop pipelining">}}


# Vectorisation
  - SIMD (Single instruction, multiple data) instructions can be executed using vector units.
  - For examples, see - [Code optimisation (vectorisation) ⟶](/posts/cs257-optimisation/).

[All topics ⟶](/posts/cs257-index/)
