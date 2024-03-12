*Homework 5
We don't have to worry about ==**Case B**== below*
## Fully associative
Latency (maybe large)
Bandwidth (more probably smaller.)

The call will be large, but it'll grab all the information at once / return more information than is needed so latency can be mitigated by bandwidth speeds.

Eviction Policy allows address and data block in the CPU to choose "victim" to boot off and change cache line.
Oldest used cache line is a good policy as it "evicts" a cache that hasn't been used and generally works well.

### Review for Midterm:
| Address | Data Block | V | M |
| ---- | ---- | ---- | ---- |
|  |  | 0 or 1 | 0 or 1 |
|  |  |  |  |


Case for load word (`lwiread`):

Evicting a "victim" (cache line):
	1. Assume V (valid bit)  = 1
	2. Case A: M (modified bit) = 0 
		1. Overwrite address and data block (cache line) from RAM. Set V = 1 and ==M = 0==
	3. ==**Case B**==: M = 1
		1. It would be dangerous to just overwrite the data because there's some data that isn't equivalent to the RAM.
		2. Copy the ==OLD== data block from cache to the corresponding place in RAM (we know address so given that address we can copy the data to RAM)
		3. Steps: Copy ==NEW== data block to RAM, then we set M = 0, then basically continue w/ Case A: Overwrite address and data block from RAM

Evicting a "victim" in the case of `swiwrite`:
	1. Assume V = 1, need to evict
	2. Case A M = 0:
		1. Overwrite address and data block, set V = 1, ==M = 1==
	3. ==**Case B**== M = 1:
		1. Copy ==OLD== data block to RAM
		2. Set M = 0
		3. Continue with Case A: Overwrite ==OLD== address and data block, now do the `sw` and set M = 1



## Direct Mapped Cache

| ~~Address~~ Tag | Data Block | V | M |
| ---- | ---- | ---- | ---- |
|  |  | 0 or 1 | 0 or 1 |
|  |  |  |  |


Instead of address and data block, it'll be tag and data block
While looking for "victim", there'll only be one possible cache line which is a match
What changes between Direct Map & Fully Associative: How we decide whether we've hit or missed.
We don't want to use a lot of comparators (below) like in fully assoc mapping, but rather just one for all the "tags"

| Tag | Specify Index in the DM cache (0, 1, etc.)  | Offset (what byte should we start reading from the data block) |
| ---- | ---- | ---- |
"Split up the wires into different groupings"
We will look at the "wires in the middle" or the Index part of the tag.

Key idea: One comparator
Let's look at the CPU that's providing the address (perhaps a 32 bit address), feed it to some comparator, and then we can compare the index of the CPU to get the tag, and compare it back to the CPU Address.

We will have to break up the address into pieces and select a single cache line, so it'll take some time, but the biggest slowdown is that two addresses may have the same index, but different tags, so we'll have a miss, which will evict at the index, and then replace the data at the tag.

### Example:



## Hardware for Mapping

COMPARATOR:

CPU address -> \[] <- cache address from some cache line
			  |
			  v
		Hit or miss?

If we have a fully assoc cache,
there'll be a comparator for every single cache line. If any one comparator says it's a hit, we have a hit and know which one.
The problem is that the comparators are pretty large in reality and have to compare every bit from CPU to the cache line address.
In a single cycle (fast), we'll know whether we have a hit or not, but it's only good for small data


## Differences

One comparator per line (fully-assoc) versus just one comparator and using the address (direct-mapped)

Cache

| 0 |  |
| ---- | ---- |
| ... |  |
| 7 | 4 bytes } 2 bits |
8 cache lines (3 bits ||| 2^3 = 8)
4 bytes (2 bits ||| 2^2 = 4)

| Tag | ==Index (3 bits)== | ==Offset (2 bits)==  |
| ---- | ---- | ---- |
```
# 42 % 4 = 2 == offset
# 42 / 4 = 10 == placeholder index

# 10 % 8 = 2 == true index
# 10 / 8 = 1 == tag
```

42 decomposed with ==2 bits for offset, and 3 bits for index==:
\[ 1 | 2 | 2 ] -> Miss
196 decomposed:
\[ 6 | 1 | 0 ] -> Miss
204 decomposed:
\[ 6 | 3 | 0 ] -> Miss
40 decomposed:
\[ 1 | 2 | 0 ] -> Hit because we have already seen this index, compare tags. Tags match so Hit

Identifying a hit:
Look at index, if V = 1, tag is valid so compare true tag to CPU tag. If they match, we have a hit, else we have a miss.

## Homework 5 Specs:
Data block should be 4 bytes or one `word` (2 bits because 2^2 = 4)
128 cache lines (7 bits because 2^7 = 128)

## Midterm "Review"

Main thing is to study the readings in the syllabus and homeworks 1-5

Process table:
	Show fields of the process table and ask what they're for.
`execvp` and `fork`: 
	Which one (or both) creates a new entry in the process table
`fork`: 
	Does it copy the file descriptors (yes, homework 2)
		`fd` points to `struct file` points to `inode`
		read and write permissions specifications is in the `inode` which is permanent. turning on and off keeps `inode` information intact
		struct file is per `fd` so stopping and booting computer no longer holds struct file values
`execvp` is called one, returns never. Why?
	`execvp` changes program and executes at main on the new program in the new stack. The original program never returns.
`fork` is called once, returns twice. Why?
	New process in process table, each one has their own program counter stack pointer and stack and call frames. We can return from the fork function in the original process and the child process.

