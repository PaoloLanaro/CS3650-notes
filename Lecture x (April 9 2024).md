### Mutex

### Semaphore

### Condition Variables



LLMs depend on neural networks depend on GPUs depend on cores for processing

Architecture of Multi-Threaded Programs
1. Resources
	- Mutex: Protects a single atomic resource.
	- Semaphores: Protects a group of identical resources.
		- Count gives us a nice intuition into # of resources available / currently in queue.
	- Condition Variables
		- Single resource with multiple permissions (e.g. reader-writer)
2. Ordering of Thread Operations
	- hw7 ex: parent-child-tasks.c
		- We can run into an error if the parent tries to get the address before the child has even written the address. We want a barrier to block the parent from trying to get the address 
		- Case F (optional)

Debugging:
1. Guess approx. where the bug is and check it
2. Usually, it means that some thread either: a) was slow to do the next operation, or b) did the next two operations before any thread could do anything.
	- Have a thread sleep and now other threads will do their work.

mcmini "demo"
``` 
Model Checking // -> look at each state of the program
./mcmini -m 30 --print-at-trace deadlock-simple  


```

Look for three kinds of bugs
a) Deadlock
b) Atomicity (make sure the data is atomic)
c) Ordering

b) use "sleep" to change it. (e.x. fast and slow thread)
c) use 'mcmini' (or other model checker)

Overall help: use assert statements.