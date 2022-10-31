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