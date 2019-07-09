# <center>Notes for Main Memory</center>

[TOC]

## Background

### Basic hardware

Using two registers, usually a **base** and a **limit**, such that $base \le address \lt base + limit $. These two registers can only be changed by OS in kernel mode.

Logical address: [0, limit)

Physical address: [R, limit + R)

### Address Binding

- **Compile time**
- **Load time**
- **Execution time**

### Logical vs Physical address space

- Compile-time and load-time address-binding generate identical logical(virtual) and physical addresses.
- Execution-time generate different logical and physical address.

**Memory-management unit** maps virtual to physical addresses.

### Dynamic loading

**Advantage**: A routine is loaded from the disk only when it is needed.

## Swapping

### Standard swapping

- Swapping time is dominated by the transfer time of the disk.
- Not swap a process with pending I/O or execute I/O operations only into operating-system buffers

## Contiguous memory allocation

Each process is contained in a single section of memory that is contiguous to the section containing the next process.

<div>
    <img src="Pic/Address.png">
</div>

### Fragmentation

- **External fragmentation**: it exists when there is enough total memory space to satisfy a request but the available spaces are not contiguous.
- **Internal fragmentation**: Unused memory that is allocated to the process.
- Solution to external fragmentation---**compaction**: shuffle the memory contents so as to place all free memory together in one large block.

## Segmentation

<div>
    <img src="Pic/Segmentation.png">
</div>

## Paging

Paging avoids external fragmentation but suffers from some internal fragmentation.

**Pages**: The logical memory blocks.

**Frames**: The physical memory blocks.

<div>
    <img src="Pic/Paging.png">
</div>

### Translation look-aside buffer(TLB)

Each process has its own page table. TLB is just like a cache of page table, since the page table is kept in main memory with a page-table base register pointing to it. Have valid bit just like cache and also support protection bits.

<div>
    <img src="Pic/TLB.png">
</div>
### Hierarchical paging

Reasons to introduce: The page table can be prohibitively expensive and resides in the memory contiguously.

For 64-bit architectures, hierarchical page tables are generally considered inappropriate.

### Hashed page tables

Linked list of elements to handle collisions.

**Clustered page tables**: Have several page entries in a bucket.

### Inverted page tables

<div>
    <img src="Pic/Inverted page table.png">
</div>



Decrease the amount of memory needed to store each page table, it increases the amount of time needed to search the table