---
layout: post
title:  "Database Sharding"
date:   2022-07-01 12:00:00 +0200
categories: architecture database sharding
excerpt: "Database sharding splits a single dataset into partitions or shards. Each shard contains unique rows of information that you can store separately across multiple computers, called nodes."
---

Database sharding splits a single dataset into partitions or shards. Each shard contains unique rows of information that you can store separately across multiple computers, called nodes. All shards run on separate nodes but share the original database's schema or design.

<h3>Horizontal partitioning</h3>

- we separate table rows into multiple new tables.
- each table has the *same* `schema` and `columns`: only row entries are different.

<h3>Vertical partitioning</h3>

- entire columns get separated into different tables.
- tables will hold both *distinct* `rows` and `columns`.
- data gets separated into multiple smaller chunks, which we call **logical shards**.
- logical shards will get distributed across separate database nodes, which we call **physical shards**.

<h3>Shared-nothing architecture</h3>
- shards will be autonomous, meaning they will not share same data or computing resources.
- sample usage case: fix conversion rates, that need to be replicated to each shard.
- in this case, all data required for a conversion is held in each shard.
- shared-nothing architecture could be implemented on both *app level* or *database level*.
- popular systems already handling sharding automatically: `MySQL Cluster`, `MongoDB Atlas`.

<br />

<h2>Sharding architectures</h2>

<h3>Key based or hash based architecture</h3>

- uses a value from data itself (e.g.: customer ID).
- value gets passed to a hash function
- result will be an output a destination shard ID.
- input values should all be from the same source column.
- data will get distributed algorithmically.

<h3>Range based architecture</h3>

- ranges of a given value will determine the destionation shard.
- sample case: products with prices `0-50$` would go to one shard and prices `50-100$` will go to another shard.
- application code will reads the price, evaluates it and writes data to corresponding shard.
- **downsides**: data may get unevenly distributed, because we may have lots or products with price `25$`.

<h3>Directory based architecture</h3>

- involves a **lookup table**: will be used to track shards.
- sample case: a `delivery_zone` column could tell us the shard ID.
- architecture is similar to range based, but static shards are used.
- a key would get checked against the lookup table and decide where to write data.
- we would do this before every query.
- **downsides**: lookup table itself might break or go down.

<br />