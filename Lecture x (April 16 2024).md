
## Final is Friday April 19th @ 1pm - 3pm
## Behrakis basement floor (010)

Exams
Simple ASM
Paging
Primarily "after the midterm"

# Review
- OSTEP mostly on concurrency, but will most likely also include paging, 

- Concept of "One big lock"
1. Mutex: Divide into several locks ==in parallel==
2. Problem Semaphores aimed to solve: Just give me any resource; but every thread wants it. The idea of one shared resource. Solution: use a count stored in the semaphore.
	- `count > 0  ->` # resources available
	- `count < 0  ->` # resources waiting
3. Condition Variables: Can we share a lock? Yes: 
	- read locks + write locks
	- What are the rules of sharing? check `acquire-release.pdf` on `thread-synch` from course page. Could be readers and writers, noah's ark (whether to put animls together) and here are the sharing conditions.
	- When we call `pthread_cond_wait`, we specify a conditional variable, and that tells us what waiting room to wait in. Why do we have to specify a mutex?
		- We have to go into the waiting room and unlock the lobby
	- Lobby is a critical section, waiting room isn't a critical section.
	- If we have two waiting rooms, (homework 7 part 2) (two condition variables), do we need two mutexes or do we just need 1? 
		- We can use only one mutex, having both waiting rooms inside of one conditional variables. and both critical sections in the mutex.
		- If we have two mutexes, we might have a race condition to the two mutexes, both accessing global variable, causing data to only be changed once or just not updating the critical section correctly.

- if we have a shared variable used in a mutex lock it has to be in the data (global) because if it's in the stack, only one thread would be able to use it / modifications wouldn't "travel upwards".

- C pointers ` int *x;` `*x = 42` `int y = *x` 
- Out parameters `int foo(&count)`
- Dereferencing `obj->field` or `(*obj).field`
- Arrays and pointers in ASM are the same because you use addresses.

- Review syscalls 
- 