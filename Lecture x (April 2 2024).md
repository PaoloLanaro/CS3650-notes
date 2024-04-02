### Agenda
- hw6 questions
- Threads: Condition Variables
- Friday: Model Checking

## Mutex
#### Globals
- 0 or 1
#### API
```
pthread_mutex_lock
pthread_mutex_unlock
```
#### Invariants
- 0 or 1 thread in the ==critical section==
- 
## Semaphores
#### Globals
- count: int (+ or -)
#### API
```
sem_wait
sem_post
```
#### Invariants
- If `count` < 0, than the `abs(count)` is the number of threads waiting.
- Else `count` is number of resources available. 
## Condition Variables
#### Globals
- Anything
#### API
```
pthread_cond_wait
pthread_cond_signal
pthread_cond_broadcast (signal to everyone)
```
#### Invariants
- User-defined

## Resources in terms of mutexes
- Every mutex (with its corresponding critical section / protected resource) guards a unique resource.
```
lock(&mutex1);
  read num_threads  // we don't want to maybe read it while it's being updated by                      // the future mutex, we want the resource to be "atomic"
unlock(&mutex1);
...random code...
lock(&mutex1);
  write num_threads 
unlock(&mutex1);
```
- Bank account model problem
## Resources in terms of semaphores
- Every semaphore guards several identical resources (we can use any of them).
- Invariant is based on the count
```
initialize count = 3
sem_init(..., 3);

sem_wait: // change the count to count--; wait if all resources busy
sem_post: // increment the count and wake any sleeping threads
```
- Producer consumer model problem where we have producers and consumers all "injecting" to a buffer
	- We have resource A available slots (producer) | resource B available tasks (consumer)

## Condition Variables
#### Model Problem for Condition Variables
- Acquire-release / reader-writer model problem
- thread A, B, C, ... wants to read, and thread 1, 2, 3, ... wants to write
- EASY: Emulate a semaphore
APIs: `pthread_cond_wait`, `pthread_cond_signal`

==He drew some picture on the board==

### User defined conditions for the reader-writer problem
###### Globals: `num_readers`, `num_writers`
- You have ==acquire code== (which is a lock on a resource) and ==release code== (which is the lock)
- Acquire condition:
	- While I'm a reader and `num_writers > 0`: `pthread_cond_wait(...)`, `num_readers++` 
- Release condition:
	- `num_readers--`, and here, make sure to signal the waiting room in the acquire condition that we have "available threads". `cond_signal`.

```
man pthread_mutex_lock
man pthread_mutex_unlock
man sem_wait
man sem_post
man pthread_cond_wait
man pthread_cond_signal

man pthread_barrier_init
man pthread_barrier_wait
```

### Brief piece on "barriers"
- Multi-threaded code
- we generally want our threads to finish the first "phase" before continuing or interacting with another phase. We don't want the two phases to get mixed up. We will wait for everyone to enter the barrier, and then release everyone.
- PHASE 1:
- ... `phtread_barrier_wait(n)` (where `n` is the number of threads) ...
- PHASE 2:
- ...
