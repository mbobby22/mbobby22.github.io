---
layout: post
title:  "Database Replication"
date:   2022-07-01 12:00:00 +0200
categories: architecture database replication
excerpt: "Database replication refers to the process of copying data from a primary database to one or more replica databases in order to improve data accessibility and system fault-tolerance and reliability."
---

Database replication refers to the process of copying data from a primary database to one or more replica databases in order to improve data accessibility and system fault-tolerance and reliability. Here are 3 techniques which could be used.

<h3>Transactional</h3>

- we would replicate each **transaction**, from a *publisher* to a *subscriber*.
- the initial transaction would be a snapshot of the publisher inself.
- we would use a **Log Reader Agent** and each database would keep a Log Reader.
- flow:
```
publisher -> distributor -> subscriber
```
- sample case: applicable for real time data, like **online trading or bank specific** operations, like backing up live data.

<h3>Snapshot</h3>

- flow:
```
publisher -> subscriber
```
- all subscriber data will be overwritten: we would drop tables and recreate them.
- recommended for low data access frequency, that is needed on certain intervals.
- sample case: data changes with `EOBD` deadlines.
- will be slower than Transactional techniques, as it will move millions of records at a time.

<h3>Merge</h3>

- required by complex database replication needs.
- technique joins both subscriber and publisher data.
- we would have consistent data on multiple ends.
- uses a **Merge Agent**:
    - agent traces changes on both ends
    - sends reports to distributor database (propagates data)
    - when runs on distributor end, it will push subscription data.
    - when runs on subscriber end, it will pull subscription data.
- system will work `peer-to-peer`, in real-time and bi-directional.
- flow:

```
subscriber 1 -> publisher -> subscriber 2, subscriber 3, subscriber 4
```

- sample case: **retail chains**, where stocks get managed per each location.

<br />