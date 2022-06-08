---
title: "I/O and Secondary Storage"
date: 2022-06-08T11:49:23+01:00
draft: false
tags: [ "CS257", "Architecture", "I/O", "Notes" ]
---
# I/O
- Programmed I/O: CPU does all the work. 2 forms:
  - Memory mapped I/O: I/O devices treated like memory, and sending / receiving data between them uses the same instructions as writing / reading to memory. Requires reserving memory space for I/O devices.
  - Isolated I/O: Bus has seperate I/O read / write lines. Instructions specify whether an address is for I/O or main memory.

Waiting for I/O devices to be ready takes a long time.
- Polled I/O: Simply check if the device is ready every now and again.
- Interrupt driven I/O: When a device is ready, it sends an interrupt to the CPU, which then services the device. 
  - Non-maskable Interrupts (NMIs) **must** be serviced by the CPU, whereas normal IRQs can be masked out.

# DMA
- Direct Memory Access (DMA): Used for when I/O devices need to quickly transfer a lot of data. Gives the I/O device direct access to memory, bypassing the CPU. Uses a DMA Controller, located on the bus.
- Single bus, detatched DMA: All modules share a bus, DMA mimicks the CPU to exchange data between I/O and main memory.
- Single bus, intergrated DMA: I/O devices connected via a DMA to the bus.
- I/O bus: Seperate I/O bus is connected to the main bus via the DMA.

Operation modes:
- Cycle stealing: Uses the system bus in cycles where the CPU isn't using it.
- Burst mode: Locks the CPU out of the bus for a short amount of time, where DMA has full access.

# RAID
- RAID 0: Non redundant striping. Data striped across disks for better read and write performance. Any disk failure will result in data loss (no redundancy).
- RAID 1: Mirrored. 2 copies of data on 2 separate disks. All data gets written twice. Slight improvement in read speed - allows it to read from either disk.
- RAID 10: Mirrored striped: Have 2 striped systems mirroring eachother.
- RAID 2: Redundancy through hamming codes. Redundant disk stores hamming codes for all words, which allows for single bit correction. Pretty overkill. Redundant disks proportional to log of number of data disks.
- RAID 3: Bit-interleaved parity: Parity bit calculated for all bits in the same position on each data disk - so needs 1 parity disk only. Easy to implement, good performance.

For RAID 4-6, I/O requests can be satisfied in parallel, since each member disk operates
independently.

- RAID 4: Block-interleaved parity. Bit-by-bit parity strip calculated across corresponding strips on each disk. Parity bits stored in corresponding strips in parity disk.
- RAID 5: Block-interleaved distributed parity. Instead of having dedicated parity disk, parity is uniformly distributed over all disks, eliminating bottleneck.
- RAID 6: Dual redundancy. 2 disk fault tolerance - extends RAID 5 to have 2 parity stripes.

# Request level parallelism
- Infrastructure as as Service (IaaS): Cloud provider offers machines and other infrastructure, eg. Azure.
- Platform as a Service (PaaS): Cloud providers makign a platform available, consisting of eg. an operating system, programming language execution support, a database system, and a web server.
- Software as a Service (SaaS): Allowing users to access software via a client eg. Office 365, Google apps.
- Network as a Service (NaaS): Providers allowing their infrastructure to be used as a network connectivity layer.

[All topics ⟶](/posts/cs257-index/)
