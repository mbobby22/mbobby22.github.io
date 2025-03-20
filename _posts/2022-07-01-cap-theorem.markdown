---
layout: post
title:  "CAP Theorem"
date:   2022-07-01 12:00:00 +0200
categories: database cap theorem
excerpt: "CAP theorem asserts that all three of the following qualities cannot be concurrently guaranteed in any distributed data system."
---

CAP theorem asserts that all three of the following qualities cannot be concurrently guaranteed in any distributed data system:

- Consistency
- Availability
- Partition tolerance

Dsitributed database systems are exposed to faults or communication errors.

The scope of such a system is to have multiple computers working together, but offer a single database feeling.

Each node is an instance of a database server and nodes communicate with each other.

Data can be written to any node, but is read from the closest node always.

Such systems can be scaled by simply adding nodes to the system. This is cheaper than adding hardware to an existing node.
*Horizontal scaling will always be less expensive than vertical scalling*.

<h3>Consistency</h3>

- a change within a node is replicated to all nodes.
- change should happen instantly, but that is not always possible, therefore is shouldbe as fast as it will not be noticeable.
- users should be able to view the same data, even if it is financial data or personal profile info.

<h3>Availability</h3>

- a user request should always get back a response from the system.
- user should be able to query and see data, even if it is not updated to latest version.
- sample case: being able to query bank account from ATM, mobile app, desktop browser web app.

<h3>Partition Tolerance</h3>

- if a break happens between the nodes, the system should still work.
- replicas of data records should be kept in multiple, different nodes.
- partition tolerance is `MUST` in any distributed system.

Monolith applications (and database) are only able to meet `CA` (there is no `P`).

NoSQL database systems meet either `CP` or `AP`.

<h2>CP with MongoDB</h2>

- Data is stored in one **Primary** node (or even more).
- Primary node has multiple secondary replicas.
- **Secondary** replicas get updated `async`, using a Log file (from Primary node).
- Nodes ping themselves (using `heartbeat` pattern).
- If there is no response within `10 seconds`, then node is marked as inaccessible.
- If Primary node becomes unavailable, then a secondary node is choosed as primary.
- The system will not be avaibale during the above process, therefore MongoDB will compromise `Availability`.

<h2>AP with Cassandra</h2>

- System is a peer-to-peer architecture, with multiple nodes that accept `Read` and `Write` operations.
- Multiple replicas live in separate nodes and there is **no Primary** node.
- Uses a `replication factor` of three: 3 nodes get updated in a clockwise manner.
- Partitioning: there will be nodes avilable with old data.
- Old data will get updated eventually.
- The system allows divergent versions of the same data, therefore Cassandra will compromise `Consistency`.

<br />

Read more [here][cap-link]{:target="_blank" rel="noopener"}

[cap-link]: https://www.bmc.com/blogs/cap-theorem/
