---
title: "Scaling Databases"
description: "2024"
date: "2024-05-02"
tags:
- interview
---

Storage services are stateful services. Compared to stateless services, stateful services have mechanisms to ensure consistency and require redundancy to avoid data loss. A stateful service may choose mechanisms like Paxos for strong consistency or eventual consistency mechanisms. These are complex decisions, and tradeoffs have to be made, which depend on the various requirements like consistency, complexity, security, latency, and performance. This is one reason we keep all services stateless as much as possible and keep state only in stateful services.

Another reason is that if we keep state in individual hosts of a web or backend service, we will need to implement sticky sessions, consistently routing the same user to the same host. We will also need to replicate the data in case a host fails and handle failover (such as routing the users to the appropriate new host when their host fails). By push- ing all states to a stateful storage service, we can choose the appropriate storage/data- base technology for our requirements, and take advantage of not having to design, implement, and make mistakes with managing state.

Storage can be broadly classified into the following.
- SQL : Has relational characteristics such as tables and relationships between tables, including primary keys and foreign keys. SQL must have ACID properties.
- NoSQL: A database that does not have all SQL properties. 
- Column oriented database : Organizes data into columns instead of rows for efficient filtering. Examples are Cassandra and HBase. 
- Keyvalue store : Data is stored as a collection of key-value pairs. Each key corresponds to a disk location via a hashing algorithm. Read performance is good. Keys must be hashable, so they are primitive types and cannot be pointers to objects. Values don’t have this limitation; they can be primitives or pointers. Key-value databases are usually used for caching, employing various techniques like Least Recently Used (LRU). Cache has high performance but does not require high availability (because if the cache is unavailable, the requester can query the original data source). Examples are Memcached and Redis.  
- Document store : Can be interpreted as a key-value database where values have no size limits or much larger limits than key-value databases. Values can be in various formats. Text, JSON, or YAML are common. An example is MongoDB.     - Graph Database : Designed to efficiently store relationships between entities. Examples are Neo4j, RedisGraph, and Amazon Neptune.
- File storage : Data stored in files, which can be organized into directories/folders. We can see it as a form of key-value, with path as the key. 
- Block storage : Stores data in evenly sized chunks with unique identifiers. We are unlikely to use block storage in web applications. Block storage is relevant for designing low-level components of other storage systems (such as databases).
- Object storage : Flatter hierarchy than file storage. Objects are usually accessed with simple HTTP APIs. Writing objects is slow, and objects cannot be modified, so object storage is suited for static data. AWS S3 is a cloud example.