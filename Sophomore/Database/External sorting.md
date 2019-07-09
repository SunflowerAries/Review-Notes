# <center>External sorting</center>

[TOC]

## External merge sort

### Procedure

- In Pass 0, read in B pages at a time and sort internally to produce $\lceil N/B \rceil$ runs of B pages each.
- In Pass i = 1, 2, ..., use B - 1 buffer pages for input, and use the remaining page for output.

### Refinement

#### Replacement sort

In Pass 0, reserve one page for use as an input buffer and one page for use as an output buffer, the other B - 2 pages of tuples as the current set. We can write out runs of approximately 2 \* B sorted pages on average.

#### Blocked I/O

We can merge $F = \lfloor B / b \rfloor -1$ runs at a time, where *b* denotes the number of pages a buffer block consists of.

#### Double buffering

To keep CPU busy, especially when the time to consume a block is greater than the time to read in a block. Double buffering includes the input and output buffer.

<div>
    <img src="Pic/Double buffer.png">
</div>

## Comparison between B+ trees and External sorting

Superiority in terms of I/Os

- Clustered B+ tree with alternative (1) used for data entries
- Clustered B+ tree with alternative (2) used for data entries
- External sorting
- Unclustered B+ tree

