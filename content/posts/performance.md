---
title: "System Performance"
description: "2023"
date: "2023-01-27"
draft: true
tags:
- fundamentals
---


### Performance 
Measure of how fast or responsive a system is under 
- A given workload 
- A given hardware setup

Performance Goals  - As we increase workload the performance be stable or should not severely degrade the performance. If we increase hardware capacity the performance should ideally increase. 

How to spot performance problems ? 
Every performance problems is result of some queue building somewhere. Ex: Network socket queue, DB IO queue, OS run queue etc. 

Reasons for queue build up ? 
- Inefficient slow processing
- Serial resource access
- Limited resource capacity 

So while designing systems identify where queues build up can happen and avoid it. 

--- 

### Performance Principles 
- Efficiency :
    - Efficient resource utilization
        - IO - memory, network, disk
        - CPU 
    - Efficient logic 
        - Aglorithms 
        - DB queries 
    - Efficient data storage 
        - Data Structures
        - DB schema 
    - Caching 

- Concurrency 
    - Hardware
    - Software 
        - Quequeing 
        - Coherence 

- Capacity : Identifying a performance problem whether it is a concurrency/efficiency or capacity problem is a hard task. 

--- 

### System Peformance Objectives 
- Minimize request-response latency
    - Latency is measured in time units
    - Depends on wait/idle time and processing time

- Maximize throughput 
    - Throughtput is measured as Rate of request processing and depends on latency and capacity. 


### Performance Measurement Metrics

- Latency 
    - Affects : user experience
    - Desired : As low as possible

- Throughtput 
    - Affects : number of users that can be supported
    - Desired : greater than the request rate 

- Errors 
    - Affects : functional correctness
    - Desired : None 

- Resource saturation 
    - Affects : hardware capacity required
    - Desired : Efficient utilization of all system resources

- Tail latency : Indication of quequing of requets and gets worse with higher workloads 

- Measure 99 percentile latency because average latency hides effects of tail latency


--- 

### Serial Request Latency 

- Network Latency 
    - Data Transfer(Global/Regional/Local Network)
    - TCP connection - Three way handshake
    - SSL/TLS connection - On top of TCP 

- Minimizing Network Latency 
    - On Database side:
        - Connection pool : We can create connection and reuse the connection and reduce connection creation latency. 
        - Data transfer Overhead : Reduce the size of data or don't transfer the data at all(use cache).

    - On client side:
        - Use Persistent connections 
        - Static data caching 
        - Data format and compression
        - SSL Session caching : reduce the repeated new connection creation between client and server 

- Memory Latency 
    - Finite Heap memory : Increase garbage collection frequency
    - GC Alogrithm : Use proper alogrithm 
    - Finite Buffer memory on Database 
    
- Minimizing memory latency 
    - Avoid memory bloat : process should accupy as little memory as possible
    - Weak/Soft References : allows GC to destroy the objects when running out of memory
    - Multiple smaller processes are better than single process
    - Garbage collection algorithm : Different flavors available(live process vs batch process)
    - Allocate Finite buffer memory on database to improve database performance 
        -  Normalization : reduce redundancy 
        -  Compute Over Storage 

- Disk Latency 
    - Disk access latency - Web server need to access files like html and javascript files which need to loaded from disk and can cause huge latency

- Disk Latency Approaches 
    - Sequential IO and Random IO 
    - Asynchronous logging 
    - Static data can handled by reverse proxy(Page cache and Zero copy)
    - Query optimization and Indexing 
    - Data caching 
    - Hardware Level - SSD Disk, RAID(Parallel Access), Higer Input Output per seconds IOPS.

- CPU Latency 
    - Inefficient Algorithms 
    - Context Switching 

- CPU Latency Approaches
    - Batch / Async IO 
    - Single Threaded Model 
    - Thread Pool Size 
    - Multiprocess in Virtual Env 

--- 

### Concurrent Request Latency 

- Concurrent Processing 
    - Amdhal's Law : Have minimum serial request processing.  
    - Univsersal Scalability Law : Queueing + Coherence( caching of variables and there synchronization across threads)
 







