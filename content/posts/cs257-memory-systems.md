---
title: "Memory Systems and Paging"
date: 2022-06-06T10:22:41+01:00
draft: false
tags: [ "CS257", "Architecture", "Memory", "Notes" ]
---
# Types of memory
## Static vs Dynamic
- Static RAM (SRAM): Flip-flop used to store each bit. Expensive but fairly simple.
- Dynamic RAM (DRAM): Capacitor used to store each bit, charge leaks away so refresh circuitry needed - increases complexity. Otherwise, generally cheaper once you have the refresh circuitry, so more common.

## Chip types
| Type                                | Read/Write                                       | Volatility   |
|-------------------------------------|--------------------------------------------------|--------------|
| Random Access Memory (RAM)          | Electrically, at byte level                      | Volatile     |
| Read Only Memory (ROM)              | Read only - written by mask at manufacture       | Non-volatile |
| Programmable ROM (PROM)             | Read only - write once, electrically             | Non-volatile |
| Erasable PROM (EPROM)               | Can erase whole chip with UV light, then rewrite | Non-volatile |
| Electrically Erasable PROM (EEPROM) | Electrically erase whole chip, then rewrite      | Non-volatile |
| Flash Memory                        | Can erase each block and rewrite                 | Non-volatile |

## Advanced DRAM organisation
- Synchronous DRAM (SDRAM): Synchronously exchanges information with the processor on clock cycles. Means the processor knows how many clock cycles to wait for communication, so can be busy while waiting.
- Rambus DRAM (RDRAM): Uses dedicated RDRAM bus rather than explicit R/W / chip enable signals to improve performance.
- Double data rate (DDR) SRAM: Data sent on both rising and falling edge of clock to double data rate.
- Cache DRAM (CDRAM): Small amount of SRAM with a larger DRAM to act as a cache for fetches, or a buffer to speed up accesses by prefetching DRAM -> SRAM.

# Interleaved memory
- Multiple 'banks' of memory units
- Each one can serve it's own r/w request, allowing for parallel accesses if data is in different banks
- Striping data across banks improves performance

# Virtual memory
- Memory heirarchy: motivated by the designer's dilemma - we need low cost, high capacity and high performance memory, so we organise memory into a heirarchy to get the best of each type.
- Virtual memory: heirarchical memory system managed by the OS, presented to the programmer as a single contiguous memory.
- Allows programs to be independent of actual memory organisation, facilitates memory heirarchy.

# Paging
## Page tables
- Virtual memory addresses are split into (MSBs) page address, and (LSBs) line number.
- Page table contains entry for each page address, mapping to physical memory location and prescence bit. If prescence bit is 0, the page must be loaded into main memory.
- Line number used to index into location in physical memory.
{{<figure src="/pagetable.png" height=350 title="Page table virtual address translation">}}

## Inverted page tables
- 1 entry for each physical memory frame
- Index in using hashed virtual page address, maps to either a physical frame address or another entry in the inverted page table
- If maps to another entry, follow the chain to the other entry
- In practice, best to have 2*no. of page frames entries in table.
- {{<figure src="/invpagetable.png" height=300 title="Inverted page table">}}

## Translation lookaside buffer (TLB)
- "Cache" for addresses - holds most recently referenced table entries
- Removes overhead of searching main memory page tables on TLB hits
- TLB miss usually ≤0.01ish

## Page sizes
- The probability of two fetches being in the same page increases with page size - larger page sizes increase hit ratio
- However, large pages limit the number of pages we can store in cache - meaning fetches in different pages are more likely to incur a miss
- Therefore we get the following relationship between page size and hit ratio:
{{<figure src="/pagesize.png" height=200 title="Relationship between page size and hit ratio">}}

## Page replacement algorithms
- Random: keep random pages loaded. Poor performance.
- First in, first out (FIFO): Replace the page that has been in memory longest. No additional hardware required, but can unnessecarily unload frequently used pages.
- Clock replacement: Uses a 'use' bit. On page fault, identifies earliest page. If use=1, set to 0. If already 0, replace the page. Use bit set to 1 on access of the page.
- Least recently used (LRU): Swaps out the least recently used page. Requires an age counter per entry.
- Working set: Pages not referenced in the preceding interval T get replaced. T can be tuned for performance.


[All topics ⟶](/posts/cs257-index/)
