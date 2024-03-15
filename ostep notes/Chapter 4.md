## Processes
- The most fundamental abstraction provided by OS is the ==process==.
	- Process - a running program; the program itself is a lifeless thing that sits on the disk (a bunch of instructions) waiting to spring into action.
- OS takes the programs bytes and turns them into something useful.
### How can we provide the illusion of many CPUs?

- ==Time sharing== - OS creates the illusion by virtualizing the CPU. Running one process, stopping it, running another, and so forth. The OS can promote the illusion that many virtual CPUs exist when in fac there is only one physical CPU.
	- Allows users to run as many concurrent processes as they would like with the potential cost being performance.

## 4.1 The Abstraction: A Process
- The abstraction provided by the OS of a running program is something called a ==process==
	- Process is a running program at any instant in time
- To understand a process, we have to look at its ==machine state==
	- Machine state - What a program can read or update when it is running. At any given time, what parts of the machine are important to the execution of this program?
	- Includes the _memory_. Instructions lie in memory; the data that the running program reads and writes sits in memory as well. The memory the process can address is part of the process.
	- Includes the _registers_. Many instructions explicitly read or update registers and thus clearly they are important to the execution of the process.
		- Some special registers include the **program counter** (**PC**, **instruction pointer** or **IP**) tell us which instruction of the program will execute next. 
		- Similarly the **stack pointer** and associated **frame pointer** are used to manage the stack for function parameters, local variables, and return addresses.
	- Includes *I/O information* which might include a list of the files the process currently has open.

## 4.2 Process API
- Create: Some method to create new processes.
- Destroy: Systems also provide an interface to destroy processes forcefully. The user may wish to kill a process and thus some interface to halt a process is useful.
- Wait: Sometimes it's useful to wait for a process to stop running.
- Miscellaneous Control: Most OS provide some kind of method to suspend a process and then resume it.
- Status: Interfaces to get some status information about a process as well, such as how long it has run for or what state it's in.

## 4.3 Process Creation: A Little More Detail
- How does the OS get a program up and running? How does process creation actually work?
- Step 1: ==load== the code and any static data (initialized variables) into memory, and into the address space of the process.
	- Program initially reside on disk (or SSD) in some kind of executable format so the process of loading a program and static data into memory requires the OS to read those bytes from disk and place them in memory somewhere.
	- In simple OS, loading process is done eagerly (all at once before running the program), whereas modern OS perform the process lazily (by loading pieces of cod or data only as they are needed during program execution). This idea is related to ==paging and swapping==
- Step 2: Memory must be allocated for the program's **run-time stack**.
	- C programs use the stack for local variables, function parameters, and return addresses. The OS allocates this memory and gives it to the process. OS will likely initialize the stack with arguments; specifically it will fill in the parameters to the `main()` function (`argc` and `argv`).
	- The OS may also allocate memory for the program's **heap**. In C programs, the heap is used for explicitly requested dynamically-allocated data. This is oftentimes done by calling `malloc()` and frees it explicitly by calling `free()`. The heap is needed for data structures such as linked lists, hash tables, trees, etc. The heap will be small when the program starts, but as the program runs the OS may get involved and allocate more memory.
- Other initialization tasks: Related to input / output.
	- In UNIX systems each process by default has three open **file descriptors** (stdin, stdout and stderr).
	- By loading code and static data into memory, creating and initializing a stack, and by doing other works as related to I/O setup, the OS has now set the stage for program execution.
- Final step: Program Execution
	- Start the program running at the entry point, namely `main()`

## 4.4 Process States
#### The different states a process can be in a given time.
- Running: In the running state a process is running on a processor.
	- Executing instructions.
- Ready: In the ready state a process is ready to run but the OS has chosen not to run it at that given moment.
- Blocked: In the blocked state a process has performed some kind of operation that makes it not ready to run until some other event takes place.
	- Common example: A process initiates an I/O request to a disk and becomes blocked, thus some other process can use the processor.

- Being moved from ready to running means the process has been **scheduled**; being moved from running to ready means the process has been **descheduled**.
## 4.5 Data Structures
- To track the state of each process the OS likely keeps some kind of **process list** for all processes that are ready and some additional information to track which process is currently running.
- The **register context** will hold, for a stopped process, the contents of its registers.

## 4.6 Summary
- Introduction of most basic abstraction of the OS: the process.
	- Simply viewed as a running program.