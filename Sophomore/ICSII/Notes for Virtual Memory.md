# <center>Notes for Virtual Memory</center>

[TOC]

## Benefits

- The program would no longer be constrained by the amount of physical memory.
- More programs could be run at the same time, increasing CPU utilization and throughput.
- Less I/O would be needed to load or swap user programs into memory.

<div>
    <img src="Pic/Demand page.png">
</div>
Non-uniform memory access(NUMA)

## Allocating of frames

### Global allocation

Can replace the frames of other processes.

### Local allocation

## Thrashing

- Working set
- Page-fault frequency

## Allocating kernel memory

- Kernel data structures have various sizes which may not correspond to the page size.
- Devices connect with physical memory directly without involving the virtual memory, so contiguous memory is needed.

## Page size

- Large page size
  - Smaller page table
  - Less I/O time
  - Less time-consuming page fault
- Smaller page size
  - Smaller internal fragmentation
  - More accurate allocation of size and less I/O(locality)