---
title: "Dependability"
date: 2022-05-20T03:53:24+01:00
draft: false
tags: [ "CS261", "Software Engineering", "Dependability", "Notes" ]
---
# Measuring reliability
- Probability of failure on demand
- Rate of errors
- Mean time to failure
- Availability

# Attributes of dependability
- Availability: Ready for use when envoked
- Reliability: Likelihood of providing service for a given period of time
- Safety: Operation without damaging or endangering environment
- Confidentiality: Nondisclosure of undue information
- Integrity: Endures improper alterations
- Maintainability: Probability of repair taking less than t time 

Systems may prioritise these differently, eg. pacemaker needs reliability > security.

# Failures
- Fault: Cause of error
- Error: Fault manifestation
- Failure: Error propogated over system boundaries
  - Hardware failure: Components do not function
  - Software failure: Errors in specification, design or implementation
  - Operational failure: Error between the chair and the keyboard

Errors propogate through the fault-error-failure cycle.
- Fault -> error -> Failure
- -> Fault in another module
- -> System failure (as error exceeds system boundaries)

## Providing dependability
- Fault avoidance: Careful development (dependable processes)
- Fault detection and correction: Validation before program deployment
- Fault tolerance: Any faults that occur get managed, system can continue

## Detection and recovery
- Graceful degredation: Allows the system to continue, with reduced capacity
- Redundancy: Spare capacity, used to take over from failures
- Diversity: Redundant components are different types, since unlikely to both fail in the same way

The entire system should be considered for dependability. Software failure must me contained as much as possible.\

## Dependable processes
- Documentable, standardised, auditable, diverse and robust processes to produce dependable software
- Well defined, repeatable, testable, fault tolerant development processes
- Can involve:
  - Requirement reviews & management
  - System modelling
  - Program inspection
  - Automated testing
  - Test management

## Dependable system architectures
- Protection system: A parallel system which monitors the main system, and performs actions if it is in an unstable state.
- Self-monitoring: Carries out computations in seperate channels, compares results to itself to see if a fault has occurred
- N-version programming: System developed n times, outputs computed on each system, final result determined by voting

These architectures all rely on diversity.

[All topics ⟶](/posts/cs261-index/)
