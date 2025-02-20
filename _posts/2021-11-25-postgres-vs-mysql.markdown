---
layout: post
title:  "Postgres vs MySQL"
date:   2021-11-25 11:40:00 +0200
categories: coding sql postgres
excerpt: "Postgres is an object-relational database, while MySQL is a purely relational database. This means that Postgres includes features like table inheritance and function overloading, which can be important to certain applications."
---

Postgres is an *object-relational database*, while MySQL is a *purely relational database*. This means that Postgres includes features like table *inheritance* and *function overloading*, which can be important to certain applications. Postgres also adheres more closely to SQL standards.


Postgres handles concurrency better than MySQL for multiple reasons:

Postgres implements Multiversion Concurrency Control (MVCC) without read locks Postgres supports *parallel query plans* that can use multiple CPUs/cores Postgres can create *indexes in a non-blocking way* (through the `CREATE INDEX CONCURRENTLY` syntax), and it can create *partial indexes* (for example, if you have a model with soft deletes, you can create an index that ignores records marked as deleted)

Postgres is known for protecting data integrity at the transaction level. This makes it less vulnerable to data corruption.

These are only some of the factors a developer might want to consider when choosing a database. Additionally, your platform provider might have a preference, for instance Heroku prefers Postgres and offers operational benefits to running it. Your framework may also prefer one over the other by offering better drivers. And as ever, **your coworkers may have opinions**!

As from my personal use, when using low level syntax commands like `ADD COLUMN` I can say I missed in Postgres the possibility of using `AFTER` or `BEFORE` attributes along with it. On the other hand, it uses `LIMIT` the same way like MySQL, unlike MSSQL where I had to use `TOP`.
