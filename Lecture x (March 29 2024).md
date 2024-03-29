### Mutex Review

```
pthread_mutex_lock(&mymut);
// critical section 1 of code
// (code using shared data)
pthread_mutex_unlock(&mymut);
.
.
.
pthread_mutex_lock(&mymut);
// critical section 1 of code
// (code using shared data)
pthread_mutex_unlock(&mymut);

// both critical section 1 because "could be one is read, another is write, so we don't want the shared data to be mismatched."
```
- Could we change the second critical section to use `&mymutex2`?
	- It's best practice to use one mutex for each specified piece of shared data.
	- Would be best used for an unrelated piece of shared data with threads.
	- Can think of it as each "set of interrelated data" should have its own mutex.

```
// mutex API
pthread_mutex_lock
pthread_mutex_unlock
```

### Semaphores
- Critical section is protecting a "resource":
	- Shared data, 
	- or maybe a file descriptor (to avoid a bunch of characters being written together, jumbled),
	- or maybe a network socket (similar idea to fd, but for reading and writing on a socket that might go across a network)
- Instead of selecting one resource (and one mutex), suppose we can use any available resource. 
	- If we want to access 4 servers, but we don't want to use one lock per server, we can just have a semaphore that tells us how where and how to go to servers.

```
// sem API
sem_init // -> 4 resources ready.
sem_post // -> analogous of mutex_unlock, but more than just flip a bit. We will increment the count and wake up a thread that's waiting (if no thread is sleeping, "lost wakeup" which is fine because there was nobody to "wake up").
sem_wait // -> analogous of mutex_lock. "wait" until we're not busy. decrements the count and then we can use one of the resources. Will go down to 0, meaning "there are 0 resources available atm". Will still decrement the count, but now negative count means that we have count number of threads waiting for a resource.

// internally there's some parameter "count" set to 4 on init
```

##### Invariant:
- `count`: If positive, we have `count` available resources a future thread can use,
     and if negative, we have `abs(count)` threads currently waiting on a resource.

#### Producer Consumer Problem
- \["Bounded Buffer Problem\] [OSTEP Page 6](https://pages.cs.wisc.edu/~remzi/OSTEP/threads-sema.pdf)
- We will need two semaphores, one for the "producer" side, and one for the "consumer" side.
	- We will `sem_init(&sem_cons,..., 0)` initially for the consumer, because we don't have any available resources.
	- We will `sem_init(&sem_prod,..., n)` initially for the producer, because we will have `n` empty slots. `n ->` number of empty or available buffer slots to produce in.
- We want the producer to produce, but the producer doesn't say this purchase has gone to shipping and billing. 

# ==IMPORTANT; will be on final 🚨🚨🚨🚨🚨==

```
Producer -> [ | | | ] -> Consumer
man pthread_create

sem_t sem_prod;
sem_t sem_cons;

int main() {
  sem_init(&sem_prod, ..., N); // -> assuming N buffer slots 
  sem_init(&sem_cons, ..., 0);
  
  for (int i = 0; i < 4; ++i) {
    pthread_create( ..., ..., producer(), ...);
    pthread_create( ..., ..., consumer(), ...);
  }
  
}

void producer() {
  loop:
  sem_wait(&sem_prod); // inc this prod sem
  int empty_slot = get_empty_slot();
  produce(empty_slot);
  sem_post(&sem_cons); // tell the cons sem that there's a task in the buffer
}

void consumer() {
  loop:
  sem_wait(&sem_cons); // inc this cons sem
  Task task = get_task();
  consume(task);
  sem_post(&sem_prod) // tell the prod sem that there's now an empty slot in buffer
}
```

### Condition Variables

```
CV API

pthread_cond_init
pthread_cond_wait
pthread_cond_signal // -> equiv of post in semaphores
pthread_cond_broadcast // -> to everyone 
```

#### Basics:
`pthread_mutex_lock`
Critical Section with a cutout which is the following: "the waiting room"
`pthread_mutex_unlock`

Within the critical section:
There will be user "hidden" variables
and we'll call `pthread_cond_wait(...)`, either blocking or continuing.
