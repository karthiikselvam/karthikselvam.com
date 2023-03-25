---
title: "Capacity Estimation on the fly"
description: "2023"
date: "2023-01-23"
tags:
- interview
---

Capacity estimation is a crucial component of system design interviews, and it can be quite challenging if one is not adequately prepared. However, with the right approach, it is possible to accurately estimate the storage, bandwidth, and memory/cache requirements needed for a particular system. This article provides a comprehensive framework for capacity estimation, which will enable you to confidently tackle capacity-related questions during system design interviews.

Typically following estimates are required :
1. Storage 
2. Bandwidth 
3. Memory/Cache

---
Assumptions:

**1. Storage Estimates**
* Single character requires 2 bytes, while long and double require 8 bytes of space.
* An average photo takes up 200 KB of space, while a high-quality photo takes up 2 MB of space.
* For videos, we assume 50 MB of space per minute of video.

Examples:
* Social media: tweet can be assumed 140 char then 140*2 bytes = 280 bytes tweet.
* Tiny URL: Average URL length <100 char. Longer URLs needing tiny URL will generally be more than 150 char, lets say 200 char. then each URL assume as 200*2 = 400 bytes
* Database id or price etc field can be double or long so 8 bytes per field.


**2. Traffic estimates**
* For social media applications like Facebook, Instagram, Twitter we can assume 1 billion total users, with 500 million daily active users. 
* A chat application like WhatsApp, and Facebook Messenger, has 500 million total users, with 100 million daily active users.
* For video streaming applications like YouTube, Netflix, and Hulu, we can assume 1 billion total users, with 800 million daily active users.
* For cloud or file storage applications like Google Drive, Dropbox, and Microsoft OneDrive, we can assume 1 billion total users, with 500 million daily active users. 

**3. Time Assumptions**
* A year has 365 days, so 5 years have 1825 days, which we round up to 2000 days.
* A day has 24 hours, which is 86400 seconds, which we round up to 100,000 seconds.

---
**Capacity Estimation for Social Media application**

Assuming each post or tweet has 140 characters, and each character requires 2 bytes, each tweet or post has a size of approximately 300 bytes. Let’s assume 1 billion total users and 500 million daily active users, we can assume that 10 million users post photos daily, with an average size of 200 KB. Using these assumptions, we can calculate the following:

**Storage**:
Text data storage: 300 bytes x 500,000,000 = 150,000,000,000 = 150 GB of tweet/post data per day
Photo storage: 200 KB x 10,000,000 = 2,000 GB = 2 TB per day.

Total storage for 5 years: 150 GB x 2000 days = 300,000 GB = 300 TB for tweet/post data, and 2 TB x 2000 days = 4,000 TB for photos.

**Bandwidth**:
Text data bandwidth: 150 GB per day / 100,000 seconds = 1.5 MB per second
Photo bandwidth: 2 TB per day / 100,000 seconds = 200 MB per second

**Memory/Cache**:
Assuming we want to cache 20 posts/tweets per user, 300 bytes * 500,000,000 daily users * 20 = 150 GB * 20 = 3000 GB = 3 TB of cache. If one machine/server can keep 150 GB of cache, we need 20 machines/servers for caching.

---

**Capacity Estimation for TinyURL**

Assuming the average length of a URL is 100 characters, and each character requires 2 bytes, each URL has a size of approximately 200 bytes. Let’s assume 1 billion total users, 100 million daily active users. So 100 million urls are generated per day. Using these assumptions, we can calculate the following:

**Storage**:
URL data storage: 200 bytes x 100,000,000 = 20,000,000,000 = 20 GB per day

Total storage for 5 years: 20 GB per day x 2000 days = 40,000 GB = 40 TB

**Bandwidth**:
URL bandwidth: 20 GB per day / 100,000 seconds = 0.2 MB per second.

**Memory/Cache**: 
Assuming we want to cache 20 urls per user, 200 bytes * 100,000,000 daily users * 20 = 20 GB * 20 = 400 GB of cache. If one machine/server can keep 150 GB of cache, we need 3 machines/servers for caching.


The framework outlined above provides a clear approach to tackle capacity estimation problems during interviews.