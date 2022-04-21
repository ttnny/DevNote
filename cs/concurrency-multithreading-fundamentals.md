# Concurrency & Multithreading Fundamentals

### Hardware

Sequences of bits (0s and 1s) are input to CPU.

Each sequence of bits is an **instruction** that command CPU of what to do.

A set of commands forms an **instruction set**.

An ordered sequence of instructions is a **task**, in **machine code** (a very long sequence of 0s and 1s).

Compilers convert human-readable code into machine code.

CPU executes the instructions one-by-one and keeps track of the execution using an internal pointer called **program counter** (PC).

CPU has its internal storage called **register**.



### System: Hardware + OS

Parallelism is a special case of concurrency but NOT vice versa.

**Single-tasking System**

MS-DOS is an example.

CPU executes one instruction at a time in a specific task (but doesn't need to be in sequence, it can jumps in response to special instructions like branch instructions) until PC reaches the last instruction. CPU then continues to execute another task.

No other programs can run during these executions.

**Cooperative Multitasking System**

Windows 3.1 is an example. Tasks get executed concurrently, not in parallel.

CPU executes two or more tasks followed the **yield** instructions. When CPU reaches a yield, it jumps to execute another instruction in of another task and will come back to resume the first task right at the PC where it left.

If yield instructions are not well-managed or a program do not yield enough, the entire system can be unresponsive.

**Preemptive Multitasking System**

Android is an example + macOS, iOS, Linux, etc. Tasks get executed concurrently, not in parallel.

OS will manage CPU resources by allocating CPU time slices to multiple tasks according to its scheduling algorithm. Similar to the Cooperative but OS will now manage the yields for you. This guarantees the responsiveness of the system. No poorly designed program (e.g. infinite loops) can take up all CPU resources.

Preemptive system is complex to implement but its complexity is abstracted out from app developers.

**Multiprocessing System**

True parallelism of task executions. This system deals with multiple CPUs (main CPU with multiple internal CPUs or Cores).
