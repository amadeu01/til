---
title: Multithreading
layout: post
date: 2020-06-07 18:19:19
---

# Defining Multithreading Terms

|       Term      |                     Definition                 |
| --------------- | ---------------------------------------------- |
| Process | The UNIX environment (such as file descriptors, user ID, and so on) created with the fork(2) system call, which is set up to run a program. |
| Thread | A sequence of instructions executed within the context of a process. |
| pthreads (POSIX threads) | A POSIX 1003.1c compliant threads interface. |
| Solaris threads | A Sun MicrosystemsTM threads interface that is not POSIX compliant. A predecessor of pthreads. |
| Single-threaded | Restricting access to a single thread. |
| Multithreaded | Allowing access to two or more threads. |
| User- or Application-level threads | Threads managed by the threads library routines in user (as opposed to kernel) space. |
| Lightweight processes | Threads in the kernel that execute kernel code and system calls (also called LWPs). |
| Bound threads | Threads that are permanently bound to LWPs. |
| Unbound threads | A default Solaris thread that context switches very quickly without kernel support. |
| Attribute object | Contains opaque data types and related manipulation functions used to standardize some of the configurable aspects of POSIX threads, mutual exclusion locks (mutexes), and condition variables. |
| Mutual exclusion locks | Functions that lock and unlock access to shared data. |
| Condition variables | Functions that block threads until a change of state. |
| Counting semaphore | A memory-based synchronization mechanism. |
| Parallelism | A condition that arises when at least two threads are executing simultaneously. |
| Concurrency | A condition that exists when at least two threads are making progress. A more generalized form of parallelism that can include time-slicing as a form of virtual parallelism. |
