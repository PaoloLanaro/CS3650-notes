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

## I lost some time setting up Github repo here so no notes for a bit

Current directory points to a struct inode
	
inodes represent files, so the current directory is an inode to represent the... directory

C ptrs: syntax
`int *x;`  -> decleration
`int y = *x;` -> execution (we dereference x and then assign the int value of `x` to `y`)
`&`: "address of" so `int *z = &y;` is assigning the address of y to the ptr z
`*z = 42;` -> dereference z and set the value to 42
`&y + 16;` will be similar to `4($sp)` -> It'll go to the address of `y` "plus 16"

Symbol table

| symbol name (string)                | address                        |
| ----------------------------------- | ------------------------------ |
|                                     |                                |
| "y" // global variable              | `&y`                           |
|                                     |                                |
| "foo" // function name w/ signature | signature: includes local vars |
Can only have global variables
### Recursive fnc's
could have local variables, but it won't have a symbol in the table because it's impossible to compute what local variables will bee used within the run time of the program.

GDB
gcc -g3 -O0
-g3 tells the compile that after it's compiled everything, you should keep the symbol table so that we can use it to debug
IDE - breakpoints

In assembly what do we use for variables?
	Registers in the text segment and more specifically LABELS

### Assembly:

Give 3 different examples of how labels are used
	1. Global Variables (there are no labels for local variables in assembly. local variable stored in temporary registers or call frames on the stack.)
	2. Loops or target of a branch
	3. Function names (e.x. jal foo)

1. `lw` / `sw`
2. `branch`
3. `jal` / `jr` `$ra`

Call Frames are used for "overflow" when not enough registers 

==Assembly directives:==
.word (global integer)
.text 
.data
.space (global array and specify how much space)

### Cache:
1. Fully associative
2. Direct Mapped
3. (not on the test): set associative

Virtual Memory:
similar to direct-mapped cache, but it also uses a page table + page in RAM
CPU -> address -> cache -> page table
vv Page table vv

| virtual address | physical address (ptr to data block) | data block  |
| --------------- | ------------------------------------ | ----------- |
|                 |                                      | page in RAM |


### inode:
we have a disk (ssd)


# To-Do
1. Review Assembly
2. Review Direct mapped vs Fully assoc
3. Review process table
4. Review inode
