# <center>Tree-structured & Hash-structured indexing</center>

[TOC]

In the tree-structured indexing, leaf pages contain data entries while non-leaf pages contain index entries.

## Indexed sequential access method(ISAM)

- Long overflow chains can develop if a number of inserts are made to the same leaf, which can significantly affect the time to retrieve a record. To alleviate this problem, the tree is initially created so that about 20 percent of each page is free.
- Since only leaf pages are modified, we can safely omit the locking step of index-level pages in concurrent transactions.

## B+ Trees: A dynamic index structure

B+ trees typically maintain 67 percent space occupancy.

Two factors favor ISAM:

- The leaf pages are allocated in sequence
- The locking overhead of ISAM is lower than that for B+ trees

### Insertion

#### Hint

- Sibling pointers only connect the adjacent nodes. For leaf nodes, it's convenient to traverse the nodes or redistribute when inserting and deleting, and for non-leaf nodes, it also helps to redistribute when deleting.
- To improve average occupancy, we can try to redistribute entries before splitting, since if a split occurs at the leaf level we have to fetch the sibling to adjust the previous and next-neighbor pointers, so a limited form of redistribution at leaf level makes sense where sibling nodes have the same parent, and do not redistribute entries at non-leaf levels.
- Duplicates is only allowed to exist between leaf level nodes and their parent nodes not allowed to exist between non-leaf level and their parent nodes.

<div>
    <img src="Pic/B+sibling.png">
</div>

<div align="center">
    <img src="Pic/B+insert.png">
</div>
### Deletion

<div align="center">
    <img src="Pic/B+delete.png">
</div>

#### Hint

- For duplicates, we can add the rid of the data record as another field while constructing the search key to have a unique index.

### Bulk-Loading

#### Reason

If we want to create a B+ tree index on a collection of data records one by one from scratch, it will be prohibitively expensive because each entry requires us to start from the root and go down to the appropriate leaf page.

## Hash-based indexing

### Static hashing

The main problem is that the number of buckets is fixed. If a file shrinks greatly, a lot of space is wasted; while a file grows a lot, long overflow chains develop. Although periodical 'rehashing' the file may improve the performance, it takes time and the index cannot be used while rehashing is in progress.

### Extendible hashing

- Global depth is the number of the last bits to express the total number of buckets, and it's kept as part of the header of the file.
- Initially, local depths are equal to the global depth, we increment the global depth by 1 each time the directory doubles and increment by 1 the local depth when splitting the bucket. 

### Linear hashing

Maintain the *Next* pointer to indicate the next bucket to be split and increment *Level* by 1 when the bucket $N_{Level} - 1$ is split.

### Comparison between Extendible hashing and Linear hashing

The directory structure can be avoided for Linear hashing by allocating primary bucket pages consecutively, while Extendible hashing's appropriate splitting the bucket may lead to a reduced number of splits and higher bucket occupancy.

#### Hint

- Linear hashing has to maintain a pointer *Next* to indicate which hash function to use, for example, $h_{Level}$ for bucket *Next* through *$N_{Level}$*,  and $h_{Level + 1}$ for bucket 0 through *Next*. For uniform distribution, it has a lower average cost for eliminating the directory.
- Extendible hashing can look up the directory to find the appropriate bucket to insert, and if a split occurs it can determine whether to double the directory by comparing the global depth and local depth. For skewed distribution, it can avoid any empty or nearly empty buckets.