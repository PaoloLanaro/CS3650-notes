## Homework 2 due this Friday, January 26

- Questions for homework 2 today and friday
### Questions

- Grades from Homework 1
	- Individual emails from prof
- Should we be leaving comments in the home
	- Should just be literal code. If the variable name makes sense it's fine, etc.
- In the description for hw2 the pseudo code recursively calls itself, how does that work?
	- execvp("./record", args);![[Pasted image 20240123153407.png]]
	- Won't this recur on itself?
```c

//EDITS:

// BUT BEWARE: This can cause an infinte loop
//             You'll need to create your own logic so that the
//               program 'record' recognizes if this is the
//               parent or child process.
// Probably replace "./record" by prgm on cmd line.

# How can we do that?

int main(int argc, char *argv[]) {
  if (argv[1] == '-v') {
    printf("we are in case b");
  }
  # defunct: char *new_argv[256] // Bytes or elements. 256 * 8
  // to populate new_argv, we can either loop through argv and add each element after the first, or we can reuse the original argv and do:
  char *new_argv[] = argv + 1; // shortcut to point to the elements of argv after the 0th
  execvp(new_argv[0], new_argv); //
}
# if command was ./record ls -l
# argv[0] == "./record";
# argv[1] == "ls";
# argv[2] == "l";
# argv[3] == "\0";
```
```
TLDR;
some modified code. The program shouldn't recur on itself so with these changes it'll execvp with the actual wanted commands.
```
- During lecture can we talk about what parts of hw1 we recommend replicating when working on hw2
	- Should be up to us. We can take some or none of hw1 and use it in hw2
### Case B

- Special fds:
	- stdin
	- stdout
	- stderr
-----
- more special file descriptors for homework 2
	- pipefd[0]
	- pipefs[1]

man 2 pipe
```
int pipe(int pipefd[2]);

uniderctional dtaa channel (pipe) that can be used for interprocess communication (different process in the table). array pipefd returns two file descriptors. refer to the ends of the pipe. 
pipefd[0] referes to the read end of the pipe.
pipefd[1] refers to the write end of the pipe. 
data written to the write end of the pipe is buffered by the kernel until it is read from the read end of the pipe.

kernel stdout | pipefd[1] | pipefd[0] | stdin kernel
```
#### pipefd

- essentially a buffer. will keep buffering until the other stdin takes in the buffer.
- glorified buffer with normal semantics

In every language a variable (or symbol):
1. Can be declared (for the type)
	1. example: int `pipefd[];`
2. Can be allocated
	1. example: int `pipefd[2];`
3. Can be initialized
	1. example: int `pipefd[2] = {3, 4}`

#### duplicate standard output into `pipefd[1]`

ls > out.txt   - basically case A
wc < in.txt    - calls word count with contents of in.txt  
ls | wc           - calls word count with contents of ls

### Pipe overview

system call (syscall) pipe points to stdout by default. 
preferred command was the "./record" program, but we'll change it to the new_argv.
execvp inherits the old file descriptors, so a child we fork now points to different file descriptor.

we need to make something that compiles into the filter program
we need to write 2 programs
record and filter
case A:
record
case B:
record and filter

we need to modify makefile 
