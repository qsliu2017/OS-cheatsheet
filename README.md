# OS-cheatsheet

## Virtualization: Virtualizing Memory

> *Virtualizing Memory (虚拟化内存)* is different with *Virtual Memory (虚拟内存)*.
> The former is a goal of OS, and the later is part of the solutions to achieved it.

### Address Translation

Address Translation creates the illution to each process that it owns the whole address space.

|Method|Approach|Advantage|Disadvantage|
|:-:|--|--|--|
|[Segmentation](https://en.wikipedia.org/wiki/Memory_segmentation)|chop memory up into *variable-sized* pieces (e.g., code, heap, stack)|logical Access Control|fragmented|
|[Paging](https://en.wikipedia.org/wiki/Memory_paging)|chop up space into *fixed-sized* pieces (page)|simplicity, no external fragmentation, flexible|memory waste (for page table), internal fragmentation|
|[Segmentation with paging](https://en.wikipedia.org/wiki/Memory_segmentation#:~:text=Segmentation%20with%20paging%5Bedit%5D)|

### Free Memory Management

|Method|Applicable|Real World Usage|
|:-:|--|--|
|Free List|General purpose|glibc `malloc`|
|(Binary) Buddy Allocator|Fixed-sized pages|Linux [physical page allocation](https://www.kernel.org/doc/gorman/html/understand/understand009.html)|
|Segregated Lists|Popular-sized blocks|Linux kernel [SLAB allocator](https://www.kernel.org/doc/gorman/html/understand/understand011.html)|

### Virtual Memory

Virtual Memory frees virtual memory management from the size limitations of physical memory.

|Level|Methods|
|:-:|--|
|Kernel|[Demand paging](https://en.wikipedia.org/wiki/Demand_paging) (Page fault, Swap)|
|Process (e.g., `glibc`)|`brk`/`sbrk`|

## Concurrency

### IPC

- signal
- communication
  - data transfer
    - byte stream
      - pipe:
        [`pipe()`](https://man7.org/linux/man-pages/man2/pipe.2.html)
      - FIFO:
        [`mkfifo()`](https://man7.org/linux/man-pages/man3/mkfifo.3.html)
      - stream socket:
        [`socket(AF_UNIX, SOCK_STREAM, 0)`](https://man7.org/linux/man-pages/man7/unix.7.html)
    - message
      - System V message queue:
        [`msgget()`](https://man7.org/linux/man-pages/man2/msgget.2.html),
        [`msgsnd()`](https://man7.org/linux/man-pages/man2/msgsnd.2.html),
        [`msgrcv()`](https://man7.org/linux/man-pages/man2/msgrcv.2.html),
        [`msgctl()`](https://man7.org/linux/man-pages/man2/msgctl.2.html) 
      - [POSIX message queue](https://man7.org/linux/man-pages/man7/mq_overview.7.html):
        [`mq_open()`](https://man7.org/linux/man-pages/man3/mq_open.3.html),
        [`mq_send()`](https://man7.org/linux/man-pages/man3/mq_send.3.html),
        [`mq_receive()`](https://man7.org/linux/man-pages/man3/mq_receive.3.html),
        [`mq_close()`](https://man7.org/linux/man-pages/man3/mq_close.3.html),
        [`mq_unlink()`](https://man7.org/linux/man-pages/man3/mq_unlink.3.html),
        [`mq_notify()`](https://man7.org/linux/man-pages/man3/mq_notify.3.html)
      - datagram socket: [`socket(AF_UNIX, SOCK_DGRAM, 0)`](https://man7.org/linux/man-pages/man7/unix.7.html)
  - shared memory
    - System V shared memory:
      [`shmget()`](https://man7.org/linux/man-pages/man2/shmget.2.html),
      [`shmat()`](https://man7.org/linux/man-pages/man2/shmat.2.html),
      [`shmdt()`](https://man7.org/linux/man-pages/man2/shmdt.2.html),
      [`shmctl()`](https://man7.org/linux/man-pages/man2/shmctl.2.html)
    - [POSIX shared memory](https://man7.org/linux/man-pages/man7/shm_overview.7.html):
      [`shm_open()`](https://man7.org/linux/man-pages/man3/shm_open.3.html),
      [`mmap()`](https://man7.org/linux/man-pages/man2/mmap.2.html),
      [`munmap()`](https://man7.org/linux/man-pages/man2/munmap.2.html),
      [`shm_unlink()`](https://man7.org/linux/man-pages/man3/shm_unlink.3.html)
    - memory mapping:
      [`mmap()`](https://man7.org/linux/man-pages/man2/mmap.2.html),
      [`munmap()`](https://man7.org/linux/man-pages/man2/munmap.2.html)
      - anonymous mapping
      - mapped file
- synchronization
  - semaphore
    - System V semaphore:
      [`semget()`](https://man7.org/linux/man-pages/man2/semget.2.html),
      [`semctl()`](https://man7.org/linux/man-pages/man2/semctl.2.html),
      [`semop()`](https://man7.org/linux/man-pages/man2/semop.2.html)
    - [POSIX semaphore](https://man7.org/linux/man-pages/man7/sem_overview.7.html)
      - named:
        [`sem_open()`](https://man7.org/linux/man-pages/man3/sem_open.3.html),
        [`sem_wait()`](https://man7.org/linux/man-pages/man3/sem_wait.3.html),
        [`sem_post()`](https://man7.org/linux/man-pages/man3/sem_post.3.html),
        [`sem_close()`](https://man7.org/linux/man-pages/man3/sem_close.3.html),
        [`sem_unlink()`](https://man7.org/linux/man-pages/man3/sem_unlink.3.html)
      - unnamed (memory-based semaphores):
        [`shmget()`](https://man7.org/linux/man-pages/man2/shmget.2.html)/[`shm_open()`](https://man7.org/linux/man-pages/man3/shm_open.3.html),
        [`sem_init()`](https://man7.org/linux/man-pages/man3/sem_init.3.html),
        [`sem_destroy()`](https://man7.org/linux/man-pages/man3/sem_destroy.3.html)
  - file lock:
    [`flock()`](https://man7.org/linux/man-pages/man2/flock.2.html),
    [`fcntl()`](https://man7.org/linux/man-pages/man2/fcntl.2.html)
  - mutex (threads)
  - condition variable (threads)

### Primitive

|Primitive|
|:-:|
|Lock|
|Condition Variable|
|Semaphore|

### Deadlock

|Condition||Prevention|
|:-:|--|--|
|Mutual exclusion||-|
|Hold-and-wait||1. *hold*: release all resources before request a new one<br> 2. *wait*: request all resources required in advance|
|No preemption||release all held resources if occurs wait|
|Circular wait||request by linear order|

## Reference

1. [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/)
1. [Understanding the Linux Virtual Memory Manager](https://www.kernel.org/doc/gorman)
1. [The Linux Programming Interface](https://man7.org/tlpi/)
