# Process control block

We'll discuss the concept of a process control block, which contains useful
information for a process to function.


### Process concept

A process is a program while in execution. When we execute the program, a OS 
creates process. A process is an active entity while it's under execution


### Process control block

The process control block represents a process in the operating system.
Also it called as task control block. It's a repository of information
associated with a specific process and it contains various pieces of information like:

**Process state**: </br>
The process state represents state of process in the operating system. A process
can be new, ready, running, waiting or terminated status.


**Program Counter**: </br>
A process contains several instructions which are executed linearly by the CPU.
A program counter (PC) indicates the address of the next instruction
to be executed for this process.


**CPU registries**: </br>
A register is a tiny bit of memory which can contain state information of a process
A CPU register stores the state information of a process
if interruptions occur in the running process. This allows the process
to continue when the scheduler schedules it to run again.
