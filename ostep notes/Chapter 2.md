## 2.5
- OS takes physical ==resources==, such as a CPU, memory, or disk, and virtualizes them.

### Goals of OS

- One obvious goal in designing and implementing an operating system is to provide high performance; another way to say this is our goal is to minimize the overheads of the OS.
- Another goal is ==protection== between applications, as well as between the OS and applications.
- OS must run non-stop; when it fails, _all_ applications running on the system fail as well.
	- AKA provide a high degree ==reliability==
- Mixed other goals:
	- Energy-efficiency
	- Security - protection malicious applications is critical 
	- Mobility - increasingly important as OSes are run on smaller and smaller devices

## 2.6 (Simple timeline)
- Early operating systems were just a set of libraries of commonly-used functions
- Moving beyond being simple library of commonly-used services, operating systems took on a more central role in managing machines.
	- Syscalls invented and pioneered so that instead of providing OS routines as a library, the idea here was to add a special pair of hardware instructions and hardware state to make the transition into the OS a more formal controlled process.
- Multiprogramming (mainframes started phasing out in favor of minicomputers)
	- Instead of just running one job at a time, the OS would load a number of jobs into memory and switch rapidly between them, thus improving CPU utilization.
	- This created issues related to ==memory protection== because we wouldn't want one program to be able to access the memory of another program.
	- Another issue was ==concurrency== issues introduced by multiprogramming. Making sure the OS was behaving correctly despite the presence of interrupts is a great challenge.
- Modern era (PC's)
	- Early operating systems such as DOS didn't think memory protection was important so malicious applications could overwrite all memory.
	- First generation of Mac OS took cooperative approach to job scheduling so a thread in an infinite loop could take over the entire system, forcing a reboot.

## 2.7 (Summary)
- Introduction to OS