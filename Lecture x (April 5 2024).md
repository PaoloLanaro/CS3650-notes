### Agenda
- hw7 (due next Friday April 12th)
-  Condition Variables
- McMini
	- Model checker
- `./mcmini [flags] a.out`
	- flags will allow us place limits on how many "code" paths we want to check (or depth of check), and allows us to put a "stopping point" on our checker

- Final is April 19th and hw7 due April 12th
- Class April 16th will be a final review

### One writer ==OR== any number of readers so that we can't modify (write) the statement critical section while we're reading or writing

```
cp -pr /course/cs3650/homework/hw7 ./

cd /tmp/gene
cd hw7

tar zxvf mcmini[tab]

cd mcmini-april-2024/
./configure
make -j8 // -> will allow us to make with multiple cores
../../mcmini-april-2024/mcmini -h // -> help
../../mcmini-april-2024/mcmini -m30 --print-at-trace ./producer-consumer
```

Trace is more or less each thread / type of thread

### [OSTEP Concurrency Bugs](https://pages.cs.wisc.edu/~remzi/OSTEP/threads-bugs.pdf)

There's two main categories for bugs:
1. Atomicity Violation
	- critical section has an atomic section that's missing some atomic field
	- 
2. Ordering Violation
	- Reading and writing some atomic variable. Start off with VAR as a global variable
		- Problem is that we might look up VAR when it's still global and "NULL" where as we haven't gotten around to writing that VAR is actually somewhere.
		- Thread A looked it up too soon, so we have to do a sem_wait() on a semaphore with count 0 on thread A, and thread B will be the one who does the sem_post() to allow the first thread to actually start reading. 

