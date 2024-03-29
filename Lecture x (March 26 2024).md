## Agenda
1. Review midterm
2. hw6 online (due Tuesday next week)
3. Semaphores

I got: 85

## Homework 6
`#include semaphores.h` or something like that for the semaphores part of this assignment.
main will create however many philosophers, and they'll each need to grab left and right forks to eat. 
Simple solution: have a mutex "between" each philosopher and before you eat, you lock the current fork. This could cause each philosopher to grab the left fork and "lock" their left hand, but this makes all philosophers wait for a right hand, which disallows any progress. 

`gcc -o [exe name] [naive-solution].c`, error: `undefined reference to 'pthread_create'`
`gcc -o [exe name] -lpthread [naive.c]` -> `gcc -o [exe name] -pthread [naive.c]`

Won't cause a deadlock in the naive solution (or at least very rare).

### Part 1: examine code and explain how deadlock can happen
### Part 2: write code that'll make deadlock never occur using mutex
### Part 3: write code that'll make deadlock never occur using semaphore

## Mutexes
protect one thread
```
pthread_mutex_t reference = PTHREAD_MUTEX_INITIALIZER; // define and init it
pthread_mutex_lock(&reference)
pthread_mutex_unlock(&reference)
```
- uses 1 bit to initialize the "lock" to 0, "unlocked"
- same thread locks and unlocks
## Semaphores
protect a buffer or multiple resources:
- Example: the multiple philosopher's forks
```
// sempahore:
#include <sem.h>
int sem_init(sem_t *sem, int pshared, usigned int value) // man page for semaphore => man sem_init(3)

sem_t mysem;
sem_init(&mysem, 0, count); // count can be the number of forks

sem_wait(&mysem); // this will cause the 'count' to decrement by 1
// whatever we need this thread to do
sem_post(&mysem); // this will cauxe the 'count' to increment by 1
```

- so the semaphore is basically an int that establishes: 
	- if the semaphore count < 0, then this is the number of threads waiting
	- if the semaphore count > 0, then this is the number of extra resources / buffer slots available for the next thread.

Initialize the semaphore count to NUM_THREADS - 1 so that if they want to use the last fork, they can't and we always have a fork available. This will allow us to guarantee some fork is available, and some philosopher sitting next to it that can use it.