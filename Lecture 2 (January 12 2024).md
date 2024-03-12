### syscalls:
	system calls 
	function calls to the operating system
### Compiler: gcc
	GNU C compiler
	GNU stands for GNU is Not Unix
	file.c => executable file


## **Levels:**
Shell
____
High level Languages
____
C
____
Assembly
_________
Linux kernel


Shell:
	Read-Execute-Print-Loops

% ls
	read the command, execute the command, print it, and then move on.

Homework 1:
	Really simple shell that lets user see a menu and input a number to let them execute what command they want
	Read the command, execute it, then loop back and wait for another command.
	
	fork-exec.c -> fill in the "Homework" blanks 

Linux kernel: booting
	* Initialize drivers
	* Create process table
		Array of struct

PID: Process identifier
## **Example of a process table:**

| Program | Priority | Open files  |
| ---- | ---- | ---- |
| 0 | ... | Root Process (executing in user space) (Privileged process (such as admin)) |
| 1 | ... | Login Process |
| 2 | ... | Shell process for that user (different than a shell program) |
| 3 | ... | Command (perhaps a linux commands such as ls (list)) |
| 4 | ... | ... |
| ... |  |  |

## Login Process
Username:   ~~~
Password :   ~~~


# For Homework:

WSL2 on laptop
ssh and develop on the "login" machine
Use vi for text editing in CLI
## cmds
```
# copies files from there to current directory
cp /course/cs3650/homework/hw1/* ./
ls

vi Makefile # view the makefile from within cmd line
# syntax off
# gcc -g3 -O0 -> debugger
# -o -> create output file ${FILE} ${FILE}.c

# arrows allow you to go through previous commands
# tab key automatically "ls" the place and lets you see the files

# "pipe" |

make check

# fill in the 9 commands to make the menu work
# command q just quits

make vi # well just execute the program in the editor (i believe)

# if the user types the # 1 we have to make it execute the ls
# etc for the rest of the numbers

```

---->   +-------+     ----->
input    |  cmd  |     output


ls (list files) | wc (word count)
	connects the output of "ls" into the input of "wc"

### README files
	Can be as simple as "it works"
	If I know something isn't working, say something on the README so that they can find 
	it easier

```
make dist # make distribution
		
"You must then submit your homework.
      /course/cs3650/homework/submit-cs3650-hw1 ../hw1.tar.gz"
      
Sends the file we just made to the directory
```


### Manual pages
#### Chapter 1 linux commands
#### Chapter 2 primitive commands
#### Chapter 3 more "complex" commands


```
	man 2
fork(void); # system call means clone or fork a new process
		# creates a child pid, process id
	will return some number. if the number is 0, it is the child

	man 3
execvp(const char *file, chat *const argv[]);

gdb ./[file name]
break main (puts a break point on main)
run (runs the dbg)
where (where are we?)
ctrl+x+a (puts you in full screen mode)
n (means next line)
step (steps into the line)
set follow-fork-mode child (follows a fork)
```