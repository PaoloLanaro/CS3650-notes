[Mutex OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/threads-locks.pdf)
[Semaphores OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/threads-sema.pdf)

### General Idea: Thread management for special conditions
- When you have a situation where 2+ threads are running concurrently on one piece of code you want executed twice, how do you make sure that it'll run twice total, and not twice individually, concurrently, therefore NOT running the code twice in total?


# Mutexes


# Semaphores
- A semaphore is an object with an integer value which gets manipulated in the threads with two routines: the `sem_wait()` and `sem_post()` routines.
- Semaphore initialization is similar to mutex initialization, but we also pass in an int that represents the semaphore value.
	- `sem_t s; sem_init(&s, 0, 1)` where `1` initializes the semaphore to a value of `1`
- After a semaphore is initialized, we can call the routines in order to:
		- `sem_wait(sem_t *s)` - decrement the value of the semaphore `s` by one, wait if the value of `s` is negative
		- `sem_post(sem_t *s)` - increment the value of the semaphore `s` by one, if there are threads waiting (and `s` value >0), wake them.
- The value of the semaphore, when negative, are the number of threads currently waiting to "spring" into action.