---
title: "Non Functional Requirements"
description: "2024"
date: "2024-05-01"
tags:
- interview
---

A system has functional and non-functional requirements. Functional requirements describe the inputs and outputs of the system. You can represent them as a rough API specification and endpoints. 

Non-functional requirements refer to requirements other than the system inputs and outputs. Typical non-functional requirements include the following

1. Scalability—The ability of a system to adjust its hardware resource usage easily and with little fuss to cost-efficiently support its load. The process of expanding to support a larger load or number of users is called scal- ing. Scaling requires increases in CPU processing power, RAM, storage capacity, and network bandwidth.Scaling can refer to vertical scaling or horizontal scaling.

Vertical scaling is conceptually straightforward and can be easily achieved just by spending more money. It means upgrading to a more powerful and expensive host, one with a faster processor, more RAM, a bigger hard disk drive, a solid-state drive instead of a spinning hard disk for lower latency, or a network card with higher bandwidth. 

There are three main disadvantages of vertical scaling.
- We will reach a point where monetary cost increases faster than the upgraded hardware’s performance. For example, a custom mainframe that has multiple proces- sors will cost more than the same number of separate commodity machines that have one processor each.

- Vertical scaling has technological limits. Regardless of budget, current technological limitations will impose a maximum amount of processing power, RAM, or storage capacity that is technologically possible on a single host.

- Vertical scaling may require downtime. We must stop our host, change its hardware and then start it again. To avoid downtime, we need to provision another host, start our service on it, and then direct requests to the new host. Moreover, this is only possible if the service’s state is stored on a different machine from the old or new host.

Horizontal scaling refers to spreading out the processing and storage requirements across multiple hosts. “True” scalability can only be achieved by horizontal scaling.

* Stateless and stateful services 
- HTTP is a stateless protocol, so a backend service that uses it is easy to scale horizontally

2. Availability - Availability is the percentage of time a system can accept requests and return the desired response.
