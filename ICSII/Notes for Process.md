# <center>Process</center>

[TOC]

## Scheduling

Multicore systems mean having several processors on one chips sharing the same memory, so it corresponds to the process and thread that the threads of one process also share the same memory.

There are many queues of linked list containing PCBs.

**Long-term scheduler**: job scheduler, selects processes from the mass-storage device(typically a disk) and loads them into memory for execution.

**Medium-term scheduler**: Remove a process from memory and thus reduce the degree of multiprogramming. Later, reintroduce the process into memory and continue its execution where it left off. This scheme is called swapping.

**Short-term scheduler**: CPU scheduler, selects from among the processes that are ready to execute and allocates the CPU to one of them.

## Operation

### Termination

- pid = wait(&status);
- The exit status of the child and pid are both passed to the parent
- When a process terminates, its resource are deallocated but its entry in the process table must remain there until the parent calls wait()
- zombie processes are those have terminated but their parents have not yet called wait()
- If a parent did not invoke wait() and instead terminated, leaving its child processes as orphans, Linux just assign the init process as the new parent to orphan processes.

### Communication

- **Shared memory**: The form of the data and the location of the shared memory are determined by these processes and are not under the operating system's control. Code for accessing and manipulating the shared memory is written explicitly by the application programmer.

- **Message passing**: 

  - Direct communication

    Disadvantage: Changing the id of a processor may necessitate examining all other process definitions to modify the link

  - Indirect communication

<div align="center">
    <img src="Pic/Process communication.png">
</div>

#### Pipes

- **Ordinary Pipes**
  - Unidirectional
  - Parent and child
- **Named Pipes**
  - Bidirectional
  - No parent-child relationship

## Thread

- Comprise a thread ID, a program counter, a register set and a stack
- Share with other threads the **code section, data section** and other operating-system resources, such as open files and signals.
- Process creation is time consuming and resource intensive, however, it's better to create a new thread when performing the same tasks as the existing process.

### Benefits of multithreaded programming

- **Responsiveness**
- **Resource sharing**
- **Economy**: It's more economical to create and context-switch threads.
- **Scalability**: Run in parallel on different processing cores.

### Multicore programming

- **Parallel**: Perform more than one task simultaneously.
- **Concurrent**: Allow more than one task to make progress.

#### Five challenges

- **Identifying tasks**: Examine applications to find areas that can be divided into separate tasks which can run in parallel on individual cores.
- **Balance**
- **Data splitting**
- **Data dependency**
- **Testing and debugging**

#### Operation

- **Pthread_join**: Each thread belonging to the same process can invoke this function to wait for the others instead of requiring the parent-child relationship.

#### Models

##### Many-to-One

The entire process will block if a thread makes a blocking system call. Not take advantage of multiple processing cores.

##### One-to-One

Have to restrict the number of threads since creating user thread requires creating the corresponding kernel thread.

##### Many-to-Many

#### Thread pool

**Benefits**

- Servicing a request with an existing thread is faster than waiting to create a thread.
- A thread pool limits the number of threads that exist at any one point.