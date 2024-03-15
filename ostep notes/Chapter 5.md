## 5.1 The `fork()` System Call
- `fork()` is used to create a new process.
	- **Process Identifier** is known as a **PID**.
	- Child processes don't starting running at `main()`, but instead come into life on the line on which `fork()` is called. The `fork()` method returns an int; 0 if the PID is the child, -1 on failure, and the PID of the child process in the parent.
	- Because of the idea of **time sharing**, the child or parent may run when `fork()` is called depending on the CPU, PC, etc.
	- The CPU **scheduler** determiners which process runs at a given moment in time; because the scheduler is complex we cannot usually make strong assumptions about what it will choose to do, and hence which process will run first.

## 5.2 The `wait()` System Call
- The `wait()` syscall allows us to make a parent wait for a child process to finish before continuing.
	- This allows us to remove the issue of non-determinism brought upon by the CPU **scheduler**.

## 5.3 The `exec()` System Call
- The `exec()` syscall allows us to run a program that is different from the calling program.
	- Calling `fork()` only allows us to continue running the current program, but the `exec()` syscall allow us to pivot to another program, syscall, etc.
	- ==It doesn't create a new process but rather it transforms the currently running program into a different running program==

## 5.4 Why? Motivating The API
- The separation of `fork()` and `exec()` is what allows the shell to seemingly produce multiple processes while it really just takes up one process. 
- The abstraction of the two syscalls allows you to type something into the prompt of the terminal and not have the terminal just quit out. 
	- Not calling `fork()` before code you would like terminal to run, therefore overriding the current process with the code. This will cause it to 1. run the program you specify 2. finish running program you specify. 3. the terminal exits because the process was overriden for the program you wanted and the shell program isn't "active" anymore.
	- Not calling `exec()`, so nothing happens.
- `pipe()` syscall information: With the `pipe()` syscall, the output of one process is connected to an in-kernel **pipe** (i.e., queue), and the input of another process is connected to that same pipe; thus, the output of one process seamlessly is used as input to the next, and long and useful chains of commands can be strung together.

## 5.5 Process Control and Users
- Beyond `fork()`, `exec()`, and `wait()`, there are other useful interfaces such as `kill()`.
	- `kill()` syscall is used to send **signals** to a process, including directives to pause, die, and other useful imperatives.
	- `signal()`; A process should use this syscall to "catch" various signals; doing so ensures that when a particular signal is delivered to a process, it will suspend its normal execution and run a particular piece of code in response to the signal.

## 5.6 Useful Tools
- `ps` command allows you to see which processes are running; man pages has useful flags to pass to `ps`
- `top` displays the processes of the system and how much CPU and other resources they are using.

## 5.7 Summary
- Introduction of some of the APIs dealing with UNIX process creation: `fork()`, `exec()`, and `wait()`. 
- Each process has a name; in most systems that name is a number known as the process ID (==PID==)
- The `fork()` syscall is used in UNIX systems to create a new process. The creator is called the parent; the newly created process is called the child.
- The `wait()` syscall allows a parrent to wait for its child to complete execution.
- The `exec()` family of syscalls allows the current process to break free and execute an entirely new program.
- UNIX shell often uses `fork()`, `wait()`, and `exec()` to launch user commands; the separation of fork and exec enables features like input/output redirection, pipes, and other features without changing anything about the programs being run
- 