---
layout: post
title:  "High Availability"
date:   2022-06-01 12:00:00 +0200
categories: architecture high availability systems
excerpt: "High Availability is a component of a technology system that eliminates single points of failure."
---

High Availability is a component of a technology system that eliminates single points of failure, by meeting these requirements:

- ensure constant operations
- uptime extended period

<h2>HA clusters</h2>

HA clusters are groups of servers meant to support business critical applications, by assuring:

- minimal downtime
- constant availability

HA clusters have to meet these **5 design principles**:

1. Auto failover to a redundant system to pick up an operation when component fails.
2. Auto detect app level failures as they happen.
3. No amount of data should be lost during failure.
4. Minimize downtime by implementing auto-failover redundancy.
5. Ability to manually failover and fallback: planned maintenance.

<h3>Numbers</h3>

Widely-known, but very difficult to achieve, are the five 9s: `99.999%` availability.

For eCommerce, the industry standard would be `99.99%`, meaning `52 minutes per year` or `8 seconds per day` as downtime.

For desktop and non-critical applications, the industry standard would be `99%` accepted downtime, meaning `8 hours per year` or `1.4 minutes per day`.

<h3>Acceptable downtime</h3>

When calculating acceptable downtime for your app, you should consider:

- unplanned downtime
- planned downtime (for routine work)
- updatime at both *database level* and *app level*.

<h3>Metrics</h3>

Keys metrics here would be `RTO` and `RPO`.

The Recovery Time Objective should be a *few seconds*.

The Recovery Point Objective should be *no data loss*.

<br />

Read more [here][ha-link]{:target="_blank" rel="noopener"} and [here][9s-link]{:target="_blank" rel="noopener"}

[ha-link]: https://us.sios.com/resource/high-availability/
[9s-link]: https://blog.quest.com/high-availability-architecture-considerations-and-techniques-to-achieve-five-9s/
