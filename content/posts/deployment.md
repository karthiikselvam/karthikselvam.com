---
title: "Note on Large Scale Deployment"
description: "2023"
date: "2023-01-27"
tags:
- fundamentals
---

### Challenges in Large scale deployment
1. Application Deployment - problems at the application level
2. Infrastructure Deployment - problems faced by hosting the application on the infrastructure
3. Operations - problem that arise during the maintainence of deployment


### Application Deployment
Typical components of large scale systems: 
- Web Apps and replicas
- Microservices and replicas
- Databases
    - RDBMS and NoSQL
    - Replication and Sharding 
- Message Queues 
    - Replication and partitioning 
- Caches 
- Content Storage 
- Log file storage 
- Search & Analytics 
- Directory / LDAP servers 

### Infrastructure Deployment 
Things we need to take into considersation for deploying the application at infrastructure level
- Compute Infrastructure : CPU, RAM, Disks
- Network
    - Secure Access : Firewalls, Certificates
    - Routing 
- Load Balancers 
    - Hard loadbalancers
    - Software loadbalancers 
- DNS and Discovery Services 
- Storage for Application  : Content, VM/Container Images, Backups, Logs
- Mail Servers 
- CDN 

We must consider infrastructure support for Dev, Test, Stagging, and Prod enviroment as well.

### Operations
- Development Team :Develop - Build - Test - Package - Image
- Operations Team : Deployment - Logging - Monitoring - Scale - Failover - Hotbackups / Coldbackups 

---

Deployment Solutions 
- Application Deploypment : Containers(Docker)
- Infrastructure Deployment: Cloud(Aws/Azure/Gcp)
- Operations : Kubernetes
- Automations : Devops Tools(Vagrant/Ansible/Chef)




