# <center>Storage</center>

[TOC]

## Disk

<div align="center">
    <img src="Pic/Disk.png" style="width:70%">
</div>

### Checksum(check bit)

It's computed when data is written to the disk and will be computed again when read from the disk to detect whether the sector is corrupted or the read fails for some reason. The disk controller will try to read the sector again when detecting an error, and will signal a failure when the sector is corrupted or read fails repeatedly.

### Time to access a disk block

- **seek time** is taken to move the disk heads to the track every time we change the track.
- **rotational delay**: When we read blocks from disk sequentially, the rotational delay should be ignored.
- **transfer time**

### In terms of **closeness**

same block > same track > same cylinder > adjacent cylinder

## RAID(redundant arrays of independent disks)

### Data striping

The data is segmented into equal-size partitions that are distributed over multiple disks. The size of partition is called the striping unit.

If any D successive data bits are spread over D data disks in the array, we can and have to achieve the transfer rate of each request D times the transfer rate of a single disk while the disk access time of the array equals to the access time of a single disk. When reading blocks, it surely accelerate the response speed due to its bandwidth D times that of single disk, however, when reading few bits, it may harden the burden of disks since there may be other transactions requesting disks.

### Redundancy

Store the redundant information on a small number of check disks or distribute the redundant information uniformly over all disks.

The disk array is partitioned into **reliability group** consisting of a set of data disks and a set of check disks.

### Levels of Redundancy

#### Level 0: Non-redundant

Level 0 uses **data striping **to increase the maximum bandwidth available. No redundant information is maintained. Level 0 has **the best write performance** without redundant information need to be updated and does not have the best read performance. Transfer rate \* D.

#### Level 1: Mirrored

Level 1 is the **most expensive solution**, having two identical copies of data on two different disks. For sake of a global system failure, we always write a block on one disk first and then write the copy on the mirror disk. Thus we can distribute reads between two disks and allow parallel reads of different disk blocks. 

#### Level 0+1: Striping and Mirroring

Level 0+1 combines striping and mirroring, thus read requests of the size of a disk block can be scheduled both to a disk and its mirror image. And read requests of the size of several contiguous blocks can benefit from the aggregated bandwidth of all disks. Transfer rate \* D.

#### Level 2: Error-Correcting Codes

In Level 2 the striping unit is a single bit and the redundancy scheme used to detect the failed disk is Hamming code. A write of a block involves reading D blocks into main memory, modifying the D + C blocks and writing D + C blocks to disk, where C is the number of check disks. This sequence of steps is called a **read-modify-write cycle**. Transfer rate \* D.

#### Level 3: Bit-Interleaved Parity

Level 3 has a striping unit of a single bit. Redundant Information to identify which disk has failed is not necessary, since disk controllers can easily detect which disk has failed. Information to recover the lost data is sufficient, so Level 3 has a single check disk with **parity** information. A Level 3's write also requires a read-modify-write cycle. Transfer rate \* D.

#### Level 4: Block-Interleaved Parity

Level 4 has a striping unit of a disk block. The write of Level 4 also requires a read-modify-write cycle but only one data disk and the check disk are involved.
$$
NewParity = (OldData\oplus NewData) \oplus OldParity
$$

#### Level 5: Block-Interleaved Distributed Parity

Level 5 improves on Level 4 by distributing the parity blocks uniformly over all disk. Two advantages:

- Write requests can potentially be processed in parallel.
- Read requests have a higher level of parallelism.

<div align="center">
    <img src="Pic/RAID 5.png" style="width:70%">
</div>

#### Level 6: P+Q Redundancy

Level 6 system uses Reed-Solomon codes to recover from up to two simultaneous disk failures. It requires two check disks but also uniformly distributes redundant information at the block level as in Level 5.

## Disk space management

The lowest level of software in the DBMS architecture called **disk space manager**, manages space on disk. Abstractly, the disk space manager supports the concept of a page as a unit of data, the size of which is chosen to be the size of a disk block.

### Keeping track of free blocks

Keep track of which pages are on which disk blocks. Two ways for free blocks

- maintain a list of free blocks. A pointer to the first block on the free block list is stored in a known loacation on disk.
- maintain a bitmap with one bit for each disk block indicating whether the block is in use or not.

## Buffer manager

<div align="center">
    <img src="Pic/Buffer.png" style="width:80%">
</div>

The main memory is partitioned into a collection of pages which we collectively refer to as the **buffer pool** and the pages in the buffer pool are called **frames**.

Two variables for each frame in the pool

- **pin_count**: the number of current users of the page
- **dirty**

### Hint 

The buffer manager will not read another page into a frame until its pin_count becomes 0, which means the buffer manager only replace the unpinned pages when empty pages is unavaliable and if there are no unpinned pages the request should wait or be aborted.

### Prefetching

Two benefits

- The pages are available in the buffer pool when requested.
- Reading in a contiguous block of pages is much faster than reading the same pages at different times in response to distinct requests.

## Files and indexes

### Heap files

We must be able to find the id of the page containing the record with given rid. In order to support scans we must keep track of the pages in each heap file and pages that contain free space in order to implement insertion.

### Linked list of pages

Maintain a table containing the pairs of <heap_file_name, page_1_addr> to indicate where the first page is located. We call the first page of the file the header page.

We can maintain a doubly linked list of pages with free space and a doubly linked list of full pages.

<div>
    <img src="Pic/Linked list.png">
</div>
**Disadvantage**: Virtually all pages may be on the free list when the records are of variable length, and to insert a typical record, we may retrieve and examine several pages to find one with enough free space.

### Directory of pages

Each directory entry identidies a page in the heap file. The directory contains entries to the data page and we can maintain a count per entry to indicate the amount of free space on the page.

<div align="center">
    <img src="Pic/Directory.png">
</div>
## Page formats

A record is identified(rid) by using the pair <page id, slot number>.

### Fixed-length records

Two alternatives:

- Whenever a record is deleted, we move the last record on the page into the vacated slot and always insert new records at the end of the page.
- Maintain a bitmap to indicate whether the slot is free.

### Variable-length records

Maintain a directory of slots for each page with a <record offset, record length> pair per slot. When a new record is too large to fit into the remaining free space, we have to reorganize the page to reclaim the space freed by records that have been deleted before and make all records contiguous, followed by the avaiable free space.

##  Record formats

### Fixed-length records

In a fixed-length record, each field has a fixed length, and the number of fields is also fixed. Given the address of the record, the address of a particular field can be calculated using information about the lengths of preceding fields.

### Variable-length records

Two organizations

- Store fields consecutively, separated by delimiters which do not appear in the data itself. To locate a desired field, we have to scan the record in order.
- Reserve some space at the beginning of a record for use as an array of integer offsets.

Three Problems for variable-length records

- Modifying a field may cause it to grow, which requires us to shift all subsequent fields.
- A record that is modified may no longer fit into the space remaining on its page and have to be moved to another page. If we use the rids to locate the record, we may have to leave a 'forwarding address' on this page to identify the new location of the record.
- A record may grow too large to fit on a page, so we have to break a record into smaller records can maintain a chain of these pieces.

