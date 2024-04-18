# General Review

## Paging
- An approach to the space-management problem.
	- There’s usually two approaches:
	1. Chop data up into variable-sized pieces (segmentation in virtual memory).
		a. This solution has inherent difficulties; when dividing a space into different-size chunks, the space itself can become fragmented, and thus allocation becomes more challenging over time.
	2. Chop up space into fixed-sized pieces.
		a. This is the idea of paging; instead of splitting up a process’s address space into some number of variable-sized logical segments, we divide it into fixed-sized units, each of which called a page.
- Paging has the benefit of flexibility: with a fully-developed paging approach the system will be able to support the abstraction of an address space effectively, regardless of how a process uses the address space.

## syscalls

### in “ASM”
1 `->` sys_exit
4 `->` sys_write (print)

### "in C"
fork `->` will create a new `struct proc` in 
execvp `->` 
read `->` modify the offset in an existing `struct file`
open `->` 
pipe `->` create a new `fd` in `struct proc` and a new `struct file`
dup2 `->` create a new `fd` in `struct proc`


## Things to remember
Only global variables (and possibly function names) show up in the symbol table. Local variables will show up in the stack.
“fork called once and returns twice” `->` new entry is created in the process table at the same point as the fork() call meaning both the child and parent can return.
“execvp called once and returns enver” `->` runs a new program in the existing process (somewhat overriding) this means that the program you call on may never return and the process may simply end. 
