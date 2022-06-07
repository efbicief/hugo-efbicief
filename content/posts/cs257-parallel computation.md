---
title: "Parallel Computation"
date: 2022-06-07T06:30:43+01:00
draft: false
tags: [ "CS257", "Architecture", "Parallelism", "Notes" ]
---
# Flynn's taxonomy
|                    | Single Data stream                     | Multiple Data stream      |
|--------------------|----------------------------------------|---------------------------|
| Single Instruction | SISD: "Regular" processor              | SIMD: Vectorisation       |
| Single Data        | MISD: Uncommon - n-version programming | MIMD: Distributed systems |

More detail below.
{{<figure src="/flynn.png" height=350 title="Flynn's taxonomy">}}

## MIMD
- Available as shared memory and distributed memory.
  - Distributed memory: Communication can be done between autonomous processors in software. Examples include Ethernet, Infiniband, etc.
- Hardware interconnection structures allow processors to access memory and communicate with other processors.

# Hardware interconnection

[All topics ⟶](/posts/cs257-index/)
