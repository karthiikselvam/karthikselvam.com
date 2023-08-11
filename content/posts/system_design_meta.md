---
title: "System Design Concepts"
description: "2023"
date: "2023-08-05"
tags:
- fundamentals
---

**1. How to transfer data at large scale ?**
1. Non blocking I/O
2. Buffering and Batching 
3. Network Protocols
4. Message Formats
5. Load Balancing
6. Partitioning
7. Consistent Hashing

**2. How to aggregate data efficiently ?**
1. Push vs Pull
2. Deduplication
3. Checkpointing
4. Data enrichment
5. Embeded database
6. State management 
7. Fallback 

**3. How to store data reliably ?**
1. Reverse Proxy
2. Coordination service
3. Health checking 
4. Peer and service discovery
5. Replication 
6. Quorum 
7. Availability zone

**4. How to retrieve data quickly ?**
1. Aggregate on write
2. Eventual consistency
3. Denormalization
4. Data rollup 
5. Hot and cold storage
6. Polyglot persistence
7. Distributed cache

**4. How to define system requirements ?**
Functional requirement - Define the qualities of the system. What a system is supposed to do ? Example The system must allow application to exchange messages.
Non Function requirement - Define the qualites of a system how a system is supposed to be. Example scalable, highly available and fast. 

How to go about defining functional requirement ? 
Start with the customer and work backward.
We need to identify who is going to use the system and how ? 
For well know systems like youtube, twitter, facebook etc its easy. But for systems such has rate limiting, content delivery network, it can be quite challenging. In such systems its better to start with customer/clients/users and work backwards on how they use the system. For example, In youtube the customers are content creators and viewers and how they are going to use the system ? content creators will upload videos, create posts where as viewers search for videos, watch videos, and comment. Similarly in the case of content delivery network the customer are webservices and the system will trottle there request.

Non functional requirements - High availability 
Availability - Defines the system uptime, the percentage of time the system has been working and available. Example, 99% availbility, The system was unavialable about 3.65 days a years.

Success ration of request - 1 request out of 100 fails. 

What is a highly aviable system ? 
Its not about a number. Its is about architecture and process. 

Design principles behind high avilability
1. Build redundancy to eleminate single points of failure. Example : regions, availability zones, fallback, data replication, high availability pair.

2. Switch from one server to another without losing data. Example: DNS,load balancing, reverse proxy,API gateway, peer discovery, service discovery.

3. Protect the system from atypical client behavior. Example : load shedding, rate limiting, shuffle sharding, cell- based architecture.

4. Protect the system from failures and perfomance degradation of its dependencies Example: timeouts, circuit breaker, bulkhead, retries, idempotency.

5. Detect failure as they occure. Example : monitoring

Process behind high availability:
1. Change management - All code and configration changes are reviewed and approved.
2. QA - regaularly execercise tests to validate that newly introduced changes meet functional and non-functional requirements.
3. Deployment - Deploy changes to a production environment frequently, quickly, safely, automated rollback.
4. Capacity planning : Monitor system utilization and add resources to meet growing demand.
5. Disaster recovery : Recover system quickly in the event of a diaster, regularly test failover to diaster recovery.
6. Root cause analysis : Establish the root cause of the failure and identify preventive measures
7. Operational readiness review : Evaluate system's operational state and identify gaps in operations. Define actions to remediate risks.
8. Game day : simulate a failure or event and test system and team responses.
9. Team culture : Good team culture promotes process discipline.

Nonfunctional requirements - Fault tolerance
1. Fault tolerance - is the property that enables a system to continue operating properly in the even of one or more faults within some of its components.
2. Fault tolerance vs High availability - fault tolerant system has the goal of zero downtime. Where as high availability system the down time is possible and the system trie to minimize it. Fault tolerance can be achieved by using same design principles and processes as of high availability and requires more redundancy.

Nonfunctional requirements - Resilience
1. Reilience - Systems that in the face of faults can provide and maitain an acceptable level of service are called resilient systems.
2. To ensure resiliency we need faults to happen in the system periodically to test resilience also called chaos engineering. Example - Killing instances of a server.




    


