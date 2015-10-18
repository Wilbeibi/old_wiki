# Operating System Three Easy pieces
## Concurrency
#### Thread VS process P263-264
+ thread share the same address space and thus can access the same data
+ Each thread has its own private set of registers it uses for computation
+ There is one major difference, though, in the context switch we perform between threads as compared to processes: the address space remains the same (i.e., there is no need to switch which page table we are using).
+ One other major difference between threads and processes concerns the stack. Fortunately, this is usually OK, as stacks do not generally have to be very large (the exception being in pro- grams that make heavy use of recursion).
#### Thread VS Coroutine
#### Lock
+ A way to provide mutual exclusion. (fairness, performance)
+ To ensure atomic, it will disabling interrupts and hardware support for multi cores.
#### Condition Variables
+ A condition variable is an explicit queue that threads can put themselves on when some state of execution (i.e., some condition) is not as desired (by waiting on the condition). Come with a mutex to ensure exclusion.
+ **Producer Consumer Problem**
	Use two condition variables. In producer, if count == MAX, cond wait empty. In consumer if count == 0, cond wait fill
#### Semaphores
+ A semaphore is as an object with an integer value that we can manipulate with two routines (mutex is a binary semaphores)
##### Read-Writer Locks P351
+ Use a binary semaphore basic lock, a write lock(sem too) a reader counter.
	For Writer, just wait or signal  write lock
	For Reader,  wait basic lock, reader++, if reader == 1, wait write lock, signal basic lock/ wait basic lock , reader--, if reader == 0, signal write lock. signal basic lock. (May cause starvation.)
##### Dining philosophers
+ if p == 4, wait fork right, wait fork left. other people wait left, wait right.

### Common Concurrency Problems
#### Non-Deadlock Bugs
+ Atomicity-Violation Bugs
   Fix: add locks around the shared-variable references
+ Order-Violation Bugs
   Fix: using condition variables

#### Deadlock Bugs
***Large code base/encapsulation may cause deadlocks***
+ Hold-and-wait: Threads hold resources al located to them(e.g.,locks that they have already acquired) while waiting for additional resources (e.g., locks that they wish to acquire).
   Fix: we can prevent deadlock by always acquiring L1 before L2. Such strict ordering ensures that no cyclical wait arises; hence, no deadlock.
+ No preemption ( if lock2 not get, release lock1)
   ```
   top:
	lock(L1);
	if (trylock(L2) == -1) {
    unlock(L1);
    goto top;
    }
   ```
+ Better scheduling algorithm/ Detect and Recover
### Event-based Concurrency
+ Basic: IO Loop
+ Block API: select(), poll() (These may blocking event loop)
+ Solution: asynio (Based on interrupt. use signals to inform application)
 + Pros:
	 + Lock free
	 + when systems moved from a single CPU to multiple CPUs, some of the simplicity of the event-based ap- proach disappeared
 + Cons:
	 + when systems moved from a single CPU to multiple CPUs, some of the simplicity of the event-based approach disappeared (need lock again)
	 + Another problem with the event-based approach is that it does not integrate well with certain kinds of systems activity, such as paging.

## Persistence
#### File and Directories
+ A file is simply a linear array of bytes, each of which you can read or write. Low-level name of a file is often referred to as its inode number.
+ A directory, like a file, also has a low-level name (i.e., an inode number), but its contents are quite specific: it contains a list of (user-readable name, low-level name) pairs.
+ **Hard link**:
	+ The way link works is that it simply creates another name in the directory you are creating the link to, and refers it to the same inode number (i.e., low-level name) of the original file. So each time you create a hard link, reference count add one.
	+ When you delete a hard link, reference count minus one. When reach zero, original file deleted.
	+ You can’t create one to a directory (for fear that you will create a cycle in the directory tree)
+ When you create a file, you are really doing two things. First, you are making a structure (the inode) that will track virtually all relevant infor- mation about the file, including its size, where its blocks are on disk, and so forth. Second, you are linking a human-readable name to that file, and putting that link into a directory.
+ **Symbolic link**: a shortcut of path
### File System Implementation
Also ref: [Lab3 - A Simple File System](http://web.mit.edu/6.033/1997/handouts/html/04sfs.html)
+ Keys: Data structure and Access methods
1. divided the disk into blocks
2. index node, aka inode ( the most important on-disk structures of a file system)
3. Free space management ( B-tree and so forth).
4. Caching querys

## Reference
1. [Operating System: Three Easy Pieces](http://pages.cs.wisc.edu/~remzi/OSTEP/)
2. [Computer Systems: A Programmer's Perspective 3/E](http://csapp.cs.cmu.edu/3e/students.html)
3. [Linux IO模式及 select、poll、epoll详解](http://segmentfault.com/a/1190000003063859)
