# <center>File organizations & Indexes</center>

[TOC]

## Cost model

Heap file: randomly ordered

Clustered file: have search key



B: the number of **data pages**

R: the number of **records per page**

D: the average time to **read or write a disk page**

C: the average time to **process a record**

H: the time required to apply the **hash function** to a record

F: fan-out

D = 15 milliseconds , C and H = 100 nanoseconds

N = the number of pages consisting of qualifying  records

In a **hashed** file, pages are kept at about **80 percent occupancy**

For clustered file, the pages are usually at about 67 percent occupancy. Thus the number of physical data pages is about $1.5B$.  And in practice, the root page is likely to be in the buffer pool (B+ tree).

For heap file with unclustered tree index, we assume that each data entry in the index is a tenth the size of an employee data record. Therefore, the number of leaf pages is $0.1 * 1.5B = 0.15B$, and  the pages are at about 67 percent occupancy similarly.

For heap file with unclustered hash index, we assume that each data entry in the index is a tenth the size of an employee data record. And the pages are kept at about 80 percent occupancy, therefore, the number of leaf pages is $0.1 * 1.25B = 0.125B$.

**Hint**

- For the cost of searching in the File organizations except for the heap, it doesn't contain the $DN$, which means searching for several qualifying pages
- For the range search in unclustered tree index, since the records is unordered, we cannot only locate the first qualifying records and read sequentially. And we shall also update the index when we insert in the unclustered tree index.

| File Type | Scan | Equality Search | Range Search | Insert | Delete |
| :--: | :--: | :--: | :--: | :--: | :--: |
| Heap | $B(D + RC)$ | $0.5 B(D + RC)$, assuming only one qualifying record | $B(D + RC)$ | $2 D + C$ | cost of searching + C + D, if rid is offered maybe only need D to search otherwise 0.5 \* scan |
| Sorted |$B(D + RC)$ | $D\log_2B + C\log_2R + DN$ | cost of searching + $DN$ | cost of searching + $2 * 0.5 B(D + RC) + \\2D + C$have to shift subsequent records | cost of searching + $2 * 0.5 B(D + RC) + \\2D + C$ |
|  Hashed   | $1.25B(D + 0.8RC)$ |                   $H + D + 0.5RC$                    |    $1.25B(D + 0.8RC)$    |                 cost of searching +   D + C                  |                 cost of searching +   D + C                  |

## Indexes

### Alternatives for data entries in an index

- A data entry k\* is an actual data record
- A data entry is a <k, rid> pair
- A data entry is a <k, rid-list> pair

### Dense versus Sparse indexes

- **Dense**: Contain one data entry for every search key value that appears in a record in the indexed file.
- **Sparse**: Contain one entry for each page of records in the data file. Have to be clustered.

### Primary and Secondary indexes

- **Primary**: An index on a set of fields that includes the primary key.
- **Secondary**: All but Primary indexes.