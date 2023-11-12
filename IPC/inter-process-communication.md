# IPC (Inter-Process-Communication)

### Intro

Most modern computer systems use processes to execute multiple tasks at any time, 
and as multiple processes execute at the same time, often they need to communicate
with each other.
<br/>
<br/>

## What is processes?

A program when in execution is a process. However the program is more than program source code.
A process contains a process stack to store temporary data, such as function parameters,
variables and ect, a data section contains global variables, a heap allocated dynamically at runtime.
<br/>
<br/>

### What is process control block?

A process control block represents a process in the operating system. 
Process control block contains information related the to a process 
such as process state, program counter, CPU register details,
memory management information. Its look like some repository, that contains all details
that belong to the process.
<br/>
<br/>

#### What is process state?

Once the process is in execution, it changes its state, based on the stage of execution.

`New`:
- when the process is created it is in a `new` state

`Ready`:
- process moves to the ready state once it is ready to execute in a processor

`Running`:
- Once the process is in execution in a processor, it is in a running state. 
- Only one process can be in running status in a processor at any time. 
- A process moves to the ready state if there is an interrupt

`Waiting`:
- While executing if the process needs to perform some additional event such as i/o access
- it is moved to waiting for the state.

`Terminated`:
- Once a process completes execution, it's terminated
<br/>
<br/>

### Type of processes

There are 2 type of processes, which are executing concurrently in a computer system

1. Independent processes
2. Cooperating processes


We can say that `process is independent` if it can't affect or be affected
by any other process executing in the computer system. Besides,
any process that doesn't share data with any other process is also `independent process`.  
<br/>

on the other hand

We can say that `process is cooperating` if it can affect
or can be affected by other processes executing in the computer system. 
Also any process that shares data with other processes is `cooperating process`
<br/>
<br/>

## What is IPC(Inter-Process-Communication) ?

The cooperating processes need to communicate with each other
to exchange data and information. Inter-Process-Communication is the 
mechanism of communication between processes.
<br/>
<br/>

### Needs for IPC(Inter-Process-Communication) !

Lets mention several reasons for which a process needs to communicate or share data 
with other processes.


`Information sharing`:
- several users may need to access the same piece of information. It means there needs to be an environment
- for concurrent access to the shared information


`Computation speedup`:
- often a task is split into several sub-tasks to speed up its execution.
- This also requires related processes to exchange information related to the task


`Modularity`:
- most of the time applications are build in a modular way and divided into separate processes.
- For example, the Google Chrome web browser spawns a separate process for each new tab</br>


## Modes of IPC.

There are two modes through which processes can communicate with each other 
**_shared memory_** and **_message passing_**. As the name suggest, shared
memory region shares a memory between the processes. On the other hand
passing messages allows processes to communicate through messages</br>


### Shared memory

It requires communicating processes to establish a shared memory region. In general
the process who wants to communicate creates a shared memory region in its own 
address space. If other processes wish to communicate to this process, they
need to attach their address space to this shared memory segment.

![Screenshot 2023-06-13 at 13.40.59.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_P9NBdh%2FScreenshot%202023-06-13%20at%2013.40.59.png)


Process A and B establish a shared memory segment and exchange information
through shared memory region

By default OS prevent processes to accessing other process memory. 
The shared memory model requires processes to agree remove that restriction. 
Besides, as a shared memory is established based on agreement between processes,
also they able to synchronize to avoid writing data in same time in same location.</br>  


### Message passing

shared memory model is not always suitable and achievable.
For instance, in a distributed computing environment, 
the processes exchanging data might reside in different computer systems.

The message passing give a way communicating between processes. In this mode,
processes interact with each other trough messages with assistance from the
underlying operating system.</br>


![Screenshot 2023-06-13 at 14.31.53.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_ZwUyS8%2FScreenshot%202023-06-13%20at%2014.31.53.png)</br>


In above diagram we can see that processes A and B communicating through messages,
process A sending message to OS, and then it read by process B.</br>


In order to successfully exchange messages, there needs to be a communication link
between the processes. There are several techniques:


 - **_Direct communication_**:</br>
In the mode, each process explicitly specifies the recipient or the sender. 
For instance, if process A needs to send a message to process B,
it can use the primitive send(B, message)


- **_Indirect Communication_**:</br>
In this mode, processes can exchange messages through a mailbox. 
A mailbox is a container that holds the messages.
For instance, if X is a mailbox, then process A can send a message
to mailbox X using the primitive send(X, message)


- **_Synchronization_**:</br>
This is an extension to direct and indirect communication with an additional
option of synchronization. Based on the need. a process can choose to block
while sending or receiving messages.
Besides, it can also asynchronously communicate without any blocking


- **_Buffering_**:</br>
The exchanged messages reside in a temporary queue. 
These queues can be of zero, bounded, and unbounded capacity
