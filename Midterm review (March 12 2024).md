Midterms will be around 6-8 questions.
Questions will be relatively short, but they may include having to write a couple lines of code (whether it be assembly or C). 
Code snippets may be provided from ostep.org or another course adjacent website and asked what it does.

`fork`, `execvp`, ... (aphorisms)
- `fork`: "function that's called once, returns twice."
	- Process gets "duplicated"
- What happens to the `fd` of the new process in a `fork` call?
	- Same as the parent process

- `execvp`: "function that's called once, returns never."
	- Program counter will be kept in whatever program was specified in the execvp call

- Program vs Process: 
	- Program is actual code on disk (typically binary code) 
	- Process is the running process for which you have an entry in the process table

- `man 2 open` returns a `fd` which is just an integer

- C pointers or indexing into a table

- stdin and stdout point to terminal by default
- struct file will point to an inode 

- c ptrs:
- what's the name in assembly?
	- address
- give an example in assembly?
	- `lw`, `sw`: they have a register and some syntax for an address
	- const(register) -> address 

- what does it mean to copy the `fd` in fork (to a child process)? (are we going to make a copy of the struct file? inode? neither?)
	- if the parent's entry points to a struct file, the child's entry will point to the same struct file. we are NOT copying the struct file itself.
```
fd1 = open("...", ...);
fd2 = dup(fd1)
fd3 = open("...", ...)

fork a child
fd1'
fd2'
fd3'

suppose we have a file (txt) abcdefgh
fd1 = open("txt",...)
fd2 = dup(fd1)

both fd1 and fd2 will point to the same offset. that means that if we've read two chars from txt with fd1, fd2 will start at offset of 2

what happens if we read two characters from fd2 though? the same thing because they point to the same struct file which has the offset.

opening the same file twice makes two entries to the table of struct file so they will have their own (possibly unique) offset and fork / dup properties.
```


`lw \$t4, 16($sp)`
C ptr vs index
`$sp: ptr or index?`   == stack pointer
`16: ptr or index?`    == index


### inode:
we have a disk (ssd)

