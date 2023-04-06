---
title: "Introduction"
---
> An operating system is software that manages a computer's hardware. It also provides a basis for application programs and acts as an intermediary between the computer user and the computer hardware.

## What Operating Systems Do
A computer system can be divided roughly into four components : the hardware, the operating system, the application programs and a user. The hardware provides the basic computing resources for the system. The application programs definde the ways in which these resources are used to solve user's computing problems. The operating system controls the hardware and coordiantes its use among the various application programs for the various users.
### User View
The user's view of the computer varies according to the interface being used. The operating system is designed mostly for **ease of use**, with some attention paid to performance and security and none paid to **resource uilization** - how various hardware and sofrware resources are shared. 
### System View
In this context, we can view an operating system as a resource allocator. The operating system acts as the manger of these resources. Facing numerous and possibly conflicting requests for resources, the operating system must decide how to allocate them to specific programs and users so that it can operate the computer system efficiently and fairly. An operating system is a control program. A control program manages the execution of user programs to prevent errors and improper use of the computer.
### Defining Operating System
The fundamental goal of computer system is to execute programs and to make solving user problems easier. The operating system is the one program running at all times on the computer - usually called the **kernel**. Along with the kernel, there are two other types of programs: system programs, which are associated with the operating system but are not necessarily part of the kernel, and application programs, which include all programs not associated with the operation of the system. Mobile operating systems often include not only a core kernel but also **middleware** - a set of software frameworks that provide additional services to application developers. 

## Computer-System Organization
A modern general-purpose computer system consists of one or more CPUs and a number of device controllers connected through a common bus that provides access between components and shared memory. The device controller is responsible for moving the data between the peripheral devices that it controls and its local buffer storage. Typically, operating systems have a device driver for each device controller. This device driver understands the device controller and provides the rest of the operating system with a uniform interface to the device. The CPU and the device controllers can execute in parallel, competing for memory cycles. To ensure orderly access to the shared memory, a memory controller synchronizeds access to the memory.