## Blah Blah Blah
Due tonight
March 15 midterm
Tuesday March 12 will be midterm review
Paging will be included on midterm (Virtual Memory; MMU/TLB) (Chapters 15, 18)

[Chapter 15](https://pages.cs.wisc.edu/~remzi/OSTEP/vm-mechanism.pdf)
[Chapter 18](https://pages.cs.wisc.edu/~remzi/OSTEP/vm-paging.pdf)

![[memory hierarchy.png]]
Up until now, we have been using registers - RAM. Paging goes beyond RAM and into the disk (where we see paging)
When looking at the SSD / Disk we are concerned with looking at the swapfile.
![[swapfile.png]]

Review for / from hw5 (cache)

conceptually just

| address | data block |
| ------- | ---------- |
|         |            |

Optimizations:
1. data block can be more than one word
2. data block is atomic 
3. address -> "leftover" bits of the address: tag

| tag | data block | valid | modified |
| --- | ---------- | ----- | -------- |
|     |            | 0     | 0        |
|     |            | 0     | 0        |
|     |            | ...   | ...      |

Job of the cache is to mediate between the CPU and RAM. It "sits in between" the two and acts as a "buffer".

using a store word style operation would be the only way to get the modified bit to be a 1.

RULE OF CACHE:
Don't go below the cache unless required (don't go to RAM unless required).

CONCEPT:
If the "cache" is full we need to evict a "victim".
	related concept - Eviction Policy; LRU (for hw5)

Implementation of LRU for fully associative.:
	Free and Busy List

You can also decompose the address into (tag, index, offset) for direct mapped caches.

## Paging

For paging we have to have a page table, which will contain virtual address and physical address

| virtual addr. (real address) | physical addr. (address known to the RAM chip) |
| ---------------------------- | ---------------------------------------------- |
| 0                            |                                                |
| 1                            |                                                |
| 2                            |                                                |
| 3                            |                                                |
| ...                          |                                                |
For paging, what is the data block size? In Linux it's almost always 4096 bytes (called a page).
We are still going to demand that a data block is atomic (We "evict" the whole data block into RAM if we need to get space. We don't try and optimize and only send a couple bits.)
The concepts of paging and caching are basically the same thing.

In paging, we ask about the page size. (vs asking about the data block size)
Page table is part of the hardware support for paging called the MMU: Memory Management Unit.

We can decompose address as (page #, offset). offset will be 0 - 4096 (b/c of page size).

## Homework 5

Output from cache simulator and pipe it into python script that will translate to H H M ...

We have done a `$sw` and we find out the address or tag is not in the cache (cache miss). We go to RAM. In RAM we copy the data block into cache, the question is, do we need to set the modified bit or not?

I think we need to set the modified bit to 1 b/c there's no guarantee that the value that came from the registers is the same as RAM. We have modified the data block after we stored the word so modified = 1. Every time you `$sw` we set modified to 1.
