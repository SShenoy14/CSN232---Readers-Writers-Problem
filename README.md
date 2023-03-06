# CSN232 Assignment - Readers-Writers Problem
## **Problem Description**
The readers-writers problem is a classical problem of process synchronisation; it involves a data set, such as a file, that is shared by many processes at the same time. Among these processes, some are Readers, which can only read the data set and cannot make any changes, while others are Writers, which can read and write in the data sets. The readers-writers problem is used to manage synchronisation across many reader and writer processes in order to avoid data set difficulties, i.e. inconsistency amongst elements of the data set.

There have been two main iterations to the Readers-Writers problem: (i) Readers Preference (*no reader shall be kept waiting if the share is currently opened for reading*) and (ii) Writers Preference (*no writer, once added to the queue, shall be kept waiting longer than absolutely necessary*). 

Both problem statements' proposed solutions may result in starvation – the first may starve writers in the queue, while the second may starve readers. Thus, an additional constraint is sometimes added: *no thread should be allowed to starve*.

## Solution
The main idea behind the solution to the problem is that we take the solution of the Readers Preference problem (solution provided in slides) and modify it to prevent starvation of the writers.

This can be done by introducing an additional semaphore `lock`. The objective of this semaphore is to introduce a FIFO queue in which both readers and writers are added to as they come. Starvation of writers is prevented by this queue as readers who come after a writer will have to wait for the writer to free the `lock` before they can enter continue. Thus, the algorithm is starve free.

## Pseudocode

#### Semaphores
``` 
lock -- To bring readers and writers to the same queue
mutex -- To ensure mutual exclusion while updating 'readcount'
wrt -- To ensure mutual exclusion between writers
```

#### Initial Conditions
```
lock = 1
mutex = 1
wrt = 1
readcount = 0 (A variable to keep count of active readers)
```

#### Reader Process
``` 
wait(lock);
wait(mutex);
readcount++;
if(readcount == 1)
   wait(wrt);
signal(mutex);
signal(lock);

# reading is performed

wait(mutex);
readcount--;
if(readcount == 0)
   signal(wrt);
signal(mutex)
```

#### Writer Process
```
wait(lock);
wait(wrt);

# writing is performed

signal(wrt)
signal(lock)
```

  

   
