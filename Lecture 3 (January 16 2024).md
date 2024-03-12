
## From Terminal (once logged in):
``` shell
cd /course/cs3650/homework
ls
```

``` shell
cd /tmp/NAME
ls
cp /course/cs3650/help/vitutor.vi
cp /course/cs3650/help/vitutor.vi ./
vi vitutor.vi
```

``` shell
cd hw1/

*in gdb*
(n)ext       -    is next command
(s)tep       -    is step into the next function
where        -    shows you the call frames
frame x      -    goes to x (integer) frame
(p)rint var  -    shows you what is saved in the var
```
call frame:
	frame for a function call

### What's the definition of "Computer Systems"
- The study of software that cannot be written in a high-level language.
	- (AKA things you cannot do in a regular programming language) 

## Homework 1 (fork and execvp)
- fork-exec-example.c

Process table

| pid | program |  |  | process |
| ---- | ---- | ---- | ---- | ---- |
| 0 | init.c |  |  | init process |
| 1 | login.c |  |  | login process (username and password) |
| 2 | shell.c |  |  | shell process (allows execution of cmds) |
| 3 | ls.c (or whatever-command.c) |  |  | command execution |
| ... |  |  |  |  |

First, create (or fork) a new subprocess, or child process (because we don't want to get rid of process), and then replace the new subprocess' program with a new process.
### Difference between "exec" and "execvp"
``` shell
man 3 execvp
echo 'execvp("ls", ... )'
```

 execvp takes current process in program and replaces it with a different process.
 
``` shell
exec ls    - will print out ls, but replace the current process and just close the shell
```
## Fork example

``` c
int main(int argx, char *argv[]) {
	char str[] = "abc";
	printf("string: %s\n", str);
	
	int childpid = fork();
	if (childpid == 0) {
		int x = 0;
		for (x = 0; x < 10000000; x++) {
			printf("x: %d", x);
			// sleep a couple seconds		
		}
	}
}
```
## Homework 2
```
cp -r /
```

We have to write the main 
We have to write the targets for the makefile

!! We need a make check !!
!! We need a readme (can just say "yes, it works") !!

``` shell
% ./record ls -l /usr/local/bin
```

``` c
// The record.c program
int main(int argc, char *argv[]) {
	// Skip argv[0], and then create an array args[0] for the remaining argv
	// man 3 execvp: args must have NULL as the last element.
	// argv will already have NULL as last element, and args should have it also.
	execvp(args[0], args);
}

# gcc -o record record.c

# ./record echo hi mom
// record.out:  hi mom
# ./record ls -l /usr/local/bin
// expected output
# ./record ...command... 
// output
```

In original Unix you could have 16 open files.

| program |  | fd0 | fd1 | fd2 | ... | fd15 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|  |  |  |  |  |  |  |
fd0 = file descriptor 0 = stdin
fd1 = file descriptor 1 = stdout
fd2 = file descriptor 2 = stderror
### How to make stdout go to record.out??? Case A

``` c
int fd = open("record.out", FLAGS, MODE); // if successful, we'll have a file descriptor for "record.out" 
close(1);    // close stdout
dup2(fd, 1); // replace stdout by the new 'fd'.
close(fd); 
// Create 'char *args[]' for the new command, based on argv[] from main.
 execvp("./record", args);
 ```
### Pipe char
``` shell
man 2 pipe
pipe();
```
