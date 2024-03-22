Midterms: Raw scores, of which around 1/3 have been graded

90 - 100: around 1/4
80 - 89: around 1/4
70 - 79: around 1/4
60 - 69: around 1/4
less: not as many

There will be around a 10 point curve (so that most of the class gets 70 or above)

Cores
Multi-threading
GPUs
Neural Network "killer application"
"Deep learning"
Current:
Generative AI (ChatGPT)

Think of data models when thinking of neural networks: training data + updating weights and sigmoid function.

Online resources for lectures going forward:
[OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/)
Concurrency (general name for multi-threading)
[Course Website](https://course.khoury.northeastern.edu/cs3650/parent/thread-synch/)

```
pthread_create
pthread_join

pthread: Posix Threads
```
![[Pasted image 20240322160049.png]]
![[Pasted image 20240322160107.png]]

pthread lock can be implemented as just one bit because whoever locks it has to unlock it, and it doesn't matter who locked it previously or who looked it currently. "putting a 1 in the box "locks" the thread". 

In early Linux they had "One Big Lock". Whenever they saw a critical section they just grabbed the one lock.

The job of one thread is to give out tasks and the other threads jobs is to do the task.

`man pthread_create`, and when you want to wait for a thread to complete, you use the `pthread_join` command. `int pthread_join(pthread_t thread, void **value_ptr`

```
int pthread_mutex_lock(pthread_mutex_t *mutex);   // an out parameter
int pthread_mutex_unlock(pthread_mutex_t *mutex); // an out parameter
```

`*mutex` is an out parameter so that we can mutate the bit to be either locked or not

```c
#include <pthread.h>

pthread_mutex_t mutex;

~thread_1_start() {
  pthread_mutex_lock(&mutex);
}

~thread_2_start() {
  pthread_mutex_lock(&mutex);
}
```
example:
```c
pthread_mutex_t lock;
pthread_mutex_lock(&lock);
x = x + 1; // if this wasn't locked we would only incrememnt once instead of twice 
// x++; does the same thing as x = x + 1 so we don't gain an instruction or anything
pthread_mutex_unlock(&lock);
```

[Chapter 24](https://pages.cs.wisc.edu/~remzi/OSTEP/threads-api.pdf)  has most of the information on mutex's simple purposes like lock, unlock, init.

Spin lock is the 'obvious implementation'. loop through, checking whether mutex is equal to 0. whenever it does become 0 do the process!

### Semaphores
Mutex lock in memory is one bit: 0 or 1.
A semaphore is an int, negative or positive.

semaphore:
- Typically used in a buffer, or individual resources, etc. 
- `count` (value of the semaphore) - count regulates whether \[the buffer\] is full or not full.
- Case 1: count is negative:
	- A total of `count` other threads are waiting on this semaphore
- Case 2: count is positive:
	- "Plenty of space": `count` more threads can join us

```
sem_wait();
sem_post();

threads wanting to "enter" will want to sem_wait() and only when there's enough space will this thread complete: (--)
sem_post() means "i just left and there's one more space now": (++)
```