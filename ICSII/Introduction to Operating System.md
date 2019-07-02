# <center>Introduction to Operating System</center>

[TOC]

The basic roles that the OS plays

- Resource allocator
- Abstract programming API

Purpose of OS

Provide an environment in which a user can execute programs in a convenient and efficient manner.

The functions that OS should offer

- Abstract

  Convenient for programming and improve the portability

- Multiplexing

- Isolation

  The dual mode of operation provides us with the means for protecting the OS from errant users and errant users from one another. Designating some of the machine instructions that may cause harm as privileged instructions and only allow the instructions to be executed in kernel mode.

- Interaction

When the computer is powered up or rebooted, it will have an initial program, bootstrap program, to run and then the bootstrap program must locate the operating-system kernel and load it into memory.

The duties that OS are responsible.

- **Process management**:
  - Scheduling processes and threads on the CPUs
  - Creating and deleting both user and system processes
  - Suspending and resuming processes
  - Providing mechanisms for process sysnchronization
  - Providing mechanisms for process communication
- **Memory management**:
  - Keeping track of which parts of memory are currently being used and who is using them
  - Deciding which processes and data to move into and out of memory
  - Allocating and deallocating memory space as needed
- **Storage management**:
  - **File-System management**:
    - Creating and deleting files
    - Creating and deleting directories to organize files
    - Supporting primitives for manipulating files and directories
    - Mapping files onto secondary storage
    - Backing up files on stable storage media
  - **Mass-Storage management**:
    - Free-space management
    - Storage allocation
    - Disk scheduling
  - **Caching**
  - **I/O Systems**:
    - A memory-management component that includes buffering, caching, and spooling
    - A general device-driver interface
    - Drivers for specific hardware devices

<div>
    <img src="Pic/Boundary of OS.png">
</div>



## System calls

Types

- Process control
- File manipulation
- Device manipulation
- Information maintenance
- Communications
- Protection

Layered Approach

- Advantage: Simplicity of construction and debugging.
- Difficulty: Appropriately defining the various layers and less efficient.

Q:

$\bigcirc$ Why not make OS work like a library function?

- OS should work all the time to manage resources while the library function is static. Processes can determine whether to invoke the library so a process can always hold the CPU.

$\bigcirc$ Why OS can manage and coordinate other applications?

- Master-slave relationship, Isolation provided by the dual mode of operation and the boot program first load the OS program into the memory from disk. What is the difference between 1 and 2.
- For the designer, they entitle the privilege to the OS including the booting and kernel mode
- And with the clock interrupt, the OS ...

$\bigcirc$

<div>
    <img src="Pic/Prob1.png">
</div>



