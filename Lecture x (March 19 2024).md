CPU - Central Processing Unit
GPU - Graphics Processing Unit
	- Very many cores -- programmed through very many threads

Both share the idea of cores and threads

> Deep Learning (~2005 ish, b/c of GPUs) came from the idea of Neural Networks
> - We have inputs that are "connected" to nodes which feed to outputs
> Generative AI  => LLMs

Large Language Models take Neural Nets + "transformers"
Training: train on English corpus(es)

CPU chip 
- MMU: Store buffer L2 cache might be 4mb
- A bunch of cores L1 cache so that this individual core can keep loading and storing just from the cache. might be 8kb 

CPU core permissions
pthread_create(start_fnc, args)

text segment: r-x
(no write, so no conflict between threads)
stack segment: rw-
(if we write, write only to our own stack)
data segment: rw-
(can write to "thread private" data)
	- We programmed this so that we promise no other thread will write on this thread
or "shared data"
	- read-only
	- readwrite shared data


pthread_create -> roughly fork + exec
pthread_join -> roughly waitpid
`-man 2 fork`
`-man 3 execvp`
`-man waitpid`
`-man pthread_create`

`int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine) (void *), void *arg);`

man page says to compile and link with -pthread
`gcc -pthread ~~~~`

```
bank_account
  account_total // global variable; we want the partners to be able to communicate to each other.
  // if both partners decide to withdraw at the same time, than we can end up with account_total -= amount instead of account_total -= 2*amount; The bug stems from the fact that both have their own registers and both individually removed one amount, but did so individually and stored it back to just account_total -= amount, not double.
  
  thread_partner1 // withdraw()
  thread_partner2 // withdraw()
  deposit(), withdraw()

withdraw(int amount) {
  account_total -= amount;
}
```



### Mutex
- Model Problem: Designed for the 'bank account' problem
### Semaphore
- Model Problem: Producer-consumer
	- Different processes that lead into an output buffer that lead into O/S
### Condition Variable
- Most general model problem

`man pthread_mutex_init`
`man pthread_mutex_lock`
`man pthread_mutex_unlock`

mutex one bit that tells us whether someone is using it or not

```
main() {
mymutex = pthread_mutex_init;
create -> thread2
create -> thread3
}

// thread 2 wants to withdraw
// thread 3 wants to withdraw
// how do we avoid the trap of it "not doing enough work"?

withdraw() {
  pthread_mutex_lock(&mymutex) // sets "busy bit" to busy. if another thread calls this after, it'll "sleep" that thread until it's unlocked
  pthread_mutex_unlock(&mymutex) // sets "busy bit" to not busy. this will allow another thread waiting at the lock to continue 
}
// pthread_mutex_lock on mymutex allows us to

// pthread_mutex_unlock on mymutex allows us to 
```

semaphores allows many threads to communicate efficiently instead of having to wait a lot of time like mutex.