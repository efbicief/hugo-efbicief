---
title: "Embedded systems"
date: 2022-06-08T11:49:23+01:00
draft: false
tags: [ "CS257", "Architecture", "Embedded", "Notes" ]
---
Small systems dedicated to a particular task.

# Dependability
- Reliability(t): probability of system still working given it worked at t=0.
- Maintainability(d): probability of system working d time units after a fault occurred.
- Availability(t): Probability of system working at time t.
- Safety: No harm caused by system.
- Security: Communication kept confidential.

# Efficiency
- Must consider Code size, Runtime, Weight, Cost and Energy efficiency.
- Power and energy efficiency are particularly important - they tend to constrain embedded systems
  - Minimising power important for PSU design, short term cooling, voltage regulation, etc.
  - Energy is the integral of power. Minimising energy important for device longevity, low temperatures, cooling, energy cost (eg. costs a lot to get energy in space), battery capacities.

# Types of embedded processors
- ASIC (Application specific integrated circuit): Most performant, full custom etched chip. Long design times, expensive, inflexible once made.
- FPGA (Field programmable gate array): Next most performant, has programmable connections and interconnects, can be a 'hybrid' system containing a multipurporse processsor.
- DSP (Digital signal processors): Specialised to a certain type of task (digital signal processing).
- MPU: Multipurpose microprocessors that we all know and love.

# Dynamic power management
- Can reduce power use by forcing the processor to idle or sleep for a period of time.
- Voltage scaling: Power proportional to voltage squared. This means that increasing voltage quadratically increases power consumption, whilst only linearly increasing algorithm run speed.

# Embedded memory
- Often, no caches / very little heirarchy are used, and instead a simple scratch pad memory is used. This is simple in hardware and uses less energy for memory accesses.

{{<figure src="/embeddedmemory.png" height=300 title="Power consumption of different caching models, incl. scratchpad memory.">}}

[All topics ⟶](/posts/cs257-index/)
