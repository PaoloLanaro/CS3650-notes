## Non-Deadlock bugs
 - Non-deadlock bugs make up the majority of concurrency related bugs. 
 1. Atomicity-Violation Bugs
	- Simple Example: 
	```
	Thread 1::
	if (thd->proc_info) {
	  fputs(thd->proc_info, ...);
	}
	
	Thread 2::
	thd->proc_info = NULL;
	```
    - In this example thread 1 checks that `proc_info` isn't `NULL`. Thread 2 can then change `proc_info` to `NULL` before we get a chance to `fputs(...)` meaning we try and dereference a `NULL` pointer (not good).
    - “The desired serializability among multiple memory accesses is violated (i.e. a code region is intended to be atomic, but the atomicity is not enforced during execution).”
    - 
2. Order-Violation Bugs
	- Simple example:
	```
	Thread 1::
	void init() {
	  mThread = PR_CreateThread(mMain, ...);
	}
	
	Thread 2::
	void mMain(...) {
	  mState = mThread->State;
	}
	```
	- The bug arises because we are assuming that mThread is already declared and initialized in thread 1, but if thread 2 runs before thread 1 we can run into the issue of trying to access a non-instantiated object's field.
	- “The desired order between two (groups of) memory accesses is flipped (i.e., A should always be executed before B, but the order is not enforced during execution)”
- 97$ of non-deadlock bugs are either atomicity or order violations.

## Deadlock Bugs
- Deadlock occurs, for example, when a thread (say Thread 1) is holding a lock (L1) and waiting for another one (L2); unfortunately, the thread (Thread 2) that holds lock L2 is waiting for L1 to be released. ![[Pasted image 20240416215216.png]]
- There are 4 conditions for a deadlock to occur
	- Mutual exclusion: Threads claim exclusive control of resources that they require.
	- Hold-and-wait: Threads hold resources allocated to them while waiting for additional resources.
	- No preemption: Resources cannot be forcibly removed from threads that are holding them.
		- Circular wait: There exists a circular chain of threads such that each thread holds one or more resources that are being requested by the next thread in the chain.