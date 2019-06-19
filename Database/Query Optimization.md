# <center>Query Optimization</center>

[TOC]

Query optimization and execution layer in the DBMS 

<div align="center">
    <img src="Pic/Query optimizer.png">
</div>

On-the-fly means the input relation to a unary operator is pipelined into it.

## System Catalog

System catalog = data dictionary

### Storage

- Relation
  - Relation name, the file name and the file structure
  - Attribute name and type of each attributes
  - Index name
  - Integrity constraints
- Index
  - Index name and the structure
  - Search key attributes
- View
  - View name and definition
- Statistics
  - Cardinality: The number of tuples for each relation R
  - Size: The number of pages for each relation R
  - Index Cardinality: Number of distinct key values
  - Index Size: Page size
  - Index Height
  - Index Range

## Estimation

### Improved statistics: Histograms

Assume that the distribution within a histogram bucket is uniform.

equiwidth: Divide the range into subranges of equal size.

equidepth: The number of tuples within each subrange is equal.

###	 Single-relation queries

#### Utilizing an index

- Single-index access path

  Select the access path that the DBMS estimates will result in retrieving the fewest pages.

- Multiple-index access path

  When several indexes match the selection condition using Alternatives (2) or (3), each can be used to retrieve a set of rids and then intersect these sets of rids, sort by page id.

- Sorted index access path

  The list of grouping attributes is a prefix of a tree index, the index can be used to retrieve tuples in the ordered required by the group by clause.

- Index-only access path

  All the attributes mentioned in the query is available in the search key

### Multiple-relation queries

#### Left-deep plans

Left-deep trees allow us to generate all fully pipelined plans, materializing the inner relation as the base relation.

