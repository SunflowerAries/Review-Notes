# <center>Evaluation of relational operators</center>

[TOC]

## Selection

### Conjunctive normal form

A collection of conjuncts that are connected through the use of $\wedge$ operator. Each conjunct consists of one or more terms connected by $\vee$.    ($\wedge(\vee Selection_i)$)

When several indexes containing data entries with rids match conjuncts in the selection, we can use these indexes to compute sets of rids of candidate tuples and then intersect these sets of rids typically by first sorting them and then retrieve those records whose rids in the intersection.

## Projection

projection based on sorting

-  Scan R and produce a set of tuples that contain only the desired attributes.
-  Sort this set of tuples using the combination of all its attributes as the key for sorting.
-  Scan the sorted result, comparing adjacent tuples, and discard duplicates.

Modifications

- Project out unwanted attributes during the first pass of sorting with the aggressive replacement implementation.
- Eliminate duplicates during the merging passes.

Projection based on hashing

- Partitioning phase requires B - 1 output buffer pages(one output page per partition) and one input  buffer page. During the processing, we project out the unwanted attributes and apply a hash function *h* to the remaining attributes
- During Duplicate elimination phase we read in the B - 1 partitions one at a time to eliminate duplicates with another hash function *$h_2$*.
- To handle with the partition overflow problem that the number of pages in a partition is greater than that of the buffer, we need recursively apply the hash 

## Join

### Nested Loops Join

```
for tuple r in R do
	for tuple s in S do 
		if r_i = s_j then add <r, s> to result

---------------Refinement---------------

for page r in R do
	for page s in S do
		compare tuples in r and in s
```

### Block Nested Loops Join

If there is enough memory to hold the smaller relation with at least two extra buffer pages left for input and output page, then each relation can be scanned just once.

If enough memory is available, we can build an in-memory hash table for the smaller relation; if not, we can break the relation R into blocks that can fit into the available buffer pages.

### Index Nested Loops Join

```
for tuple r in R do
	for tuple s in S do 
		if r_i = s_j then add <r, s> to result
```

Whether the inner blocks is clustered plays a role.

### Sort-Merge Join

<div>
    <img src="Pic/Sortmerge.png">
</div>
### Hash Join

During the partitioning phase, hash both relations on the join attribute using the same hash function, and apply another hash function to the corresponding partition of R and S in the probing phase.

#### Hint

##### Difference between hash join and block nested loops join building a main-memory hash table

- For block nested loops join building a main-memory hash table, we only partition one relation and scan the other relation $\lceil \frac{M}{B - 2}\rceil$ times.
- For hash join, we partition both relations using the same hash function during the partitioning phase and apply another hash function to the corresponding partitions of both relations during the probing phase.

##### Difference between hash join and sort-merge join with the cost 3(M + N)

- For sort-merge, the buffer size B should be larger than $\sqrt L$, where L is the size of the larger relation. In this case, together with the replacement sort(we can get 2 \* B pages per run during the Pass 0). Thus in Pass 1 we have the number of runs $< \frac{2L}{2B} <\frac{L}{\sqrt L} < \sqrt L < B$, and we can combine the merging phases with join.
- For hash join, the buffer size B should be larger than $\sqrt L$, where L is the size of the smaller relation. In this case, we can build a hash table for each partition of the smaller relation during probing phase.

## Set Operation

### Sorting for union and difference

- Sort the combination of all fields
- Scan the sorted R and S in parallel and merge

### Hashing for union and difference

- Partition R and S using a hash function *h*
- Probing phase
  - Build an in-memory hash table for $S_l$ using another hash function
  - Scan $R_l$. For each tuple in $R_l$, probe the hash table for $S_l$

## Buffering

- Operations will share the buffer pool when they execute concurrently
- In an index nested loops join, for tuples having the same value in the join attribute, we can maximize the repetition of accessing the same partition to the inner relation by sorting the outer relation on the join attributes.