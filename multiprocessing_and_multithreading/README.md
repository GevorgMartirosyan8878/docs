# Multiprocessing and multithreading

## What is threads actually and kind of threads.

So there is 2 kinds of threads physical and virtual. 

Physical threads are our cpu cores.

Virtual threads or well known as logical cores, 
they is abstract things which allows physical core
to handle some count of threads in one time.

#### Computer multitasking 

Computer multitasking is the ability of an operating system or a program
to manage multiple tasks (also known as processes or threads) concurrently.

There is two main types of multitasking

1. Cooperative multitasking
2. Preemptive multitasking


So multithreading and multiprocessing are core concepts of computer multitasking.

#### What is processes?

<first description>
A program when in execution is a process. However the program is more than program source code.
A process contains a process stack to store temporary data, such as function parameters,
variables and ect, a data section contains global variables, a heap allocated dynamically at runtime.

<second description> 
In general processes are the instances of programs that execute in a computer system.
The processes have own isolated code parts and data. So directly the process during execution
can't access to other processes code execution and data of other process.



Processes categorized as cooperative and independent.

_Independent_ processes means that the process doesn't share information about the process, 
and also there is no any process which sharing something with that process.

_Cooperative_ processes means that the process sharing some information and also has access
to some process which sharing some info during the execution of the process.


**_Since processes are isolated program, data exchanging between them was relies on IPC mechanisms_**

#### What is threads?

Threads are lightweight semi-processes that depend on a process.
Thread executes a task in the context of process.
From technical side, the threads require same resources as a process requires,
such as a memory stack, program counter, and set of registers. However, all the threads
of the same process share process code and data sections

Threads may also need to communicate with each other, 
but unlike processes they belong to the same process
which mean there is no need some additional mechanism for inter-thread communication.
In this scenario, messages are exchanged trough process data section


### Multiprocessing and multithreading basics

Multiprocessing and multithreading handles the concurrent execution of
instances process and threads, and they are responsible for implementing
multitasking in computer system.

**_Multiprocessing_** refers to possibility to execution more than one process 
at the same time

**_Multithreading_** refers to possibility of executing multiple tasks
in a particular process concurrently

In practice, multithreading occurs when at least one process with multiple threads
executes in computer system

So, multithreading does not require multiple physical processing units, as multiprocessing does
