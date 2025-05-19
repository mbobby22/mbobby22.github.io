---
layout: post
title:  "Kafka Essentials"
date:   2023-12-01 08:00:00 +0200
categories: architecture devops kafka
excerpt: "The way consumption is implemented in Kafka is by dividing up the partitions in the log over the consumer instances so that each instance is the exclusive consumer of partitions-share at any point in time."
---

The way consumption is implemented in Kafka is by dividing up the partitions in the log over the consumer instances so that each instance is the exclusive consumer of partitions-share at any point in time. Did you know: Kafka was created at Linkedin?

{% highlight yaml %}
topic: {
    name, partitions: 3
    replications: 3
    insync-replicas: 2
    deletePolicy: retention | compaction retention: -1
}
{% endhighlight %}

- Topics go into multiple (3+, 6 etc easy to divide) partitions
- We can't attach multiple (x 2) consumers to 1 partition
- Order is guaranteed only for 1 single partition
- Messages are distributed by `modulo` on the number of partitions, resulting in an assigned partition
- We should usually create more partitions then needed (to avoid repartitioning later)
- How to properly repartition, if needed: create a new topic with multiple partitions and then redirect messages from old-topic to new-topic
- Default partition replication factor: x 3
- Replicas should be split between brokers, to avoid losing all when broker dies: `111 222 333 => 1/2/3, 2/3/1, 3/2/1`
- One of the brokers gets selected as `leader`
- Work continues as long as `acknowledged` gets back for the message:
    - 0 = fire and forget (get ack)
    - 1 = ensure leader persisted event
    - ALL = ensures events are stored on all (in-sync) replicas
- `Zookeeper` is responsible for leader election, when broker dies
- `KIP-500`: use to make kafka run with no zookeeper

<h3>Consumers</h3>

- The way [consumption is implemented in Kafka][kafka-consumer]{:target="_blank" rel="noopener"} is by dividing up the partitions in the log over the consumer instances so that each instance is the exclusive consumer of a "fair share" of partitions at any point in time. This process of maintaining membership in the group is handled by the Kafka protocol dynamically. If new instances join the group they will take over some partitions from other members of the group; if an instance dies, its partitions will be distributed to the remaining instances.
- Kafka only provides a total order over records within a partition, not between different partitions in a topic. Per-partition ordering combined with the ability to partition data by key is sufficient for most applications. However, if you require a total order over records this can be achieved with a topic that has only one partition, though this will mean only one consumer process per consumer group.

<h3>Ordering</h3>

- When a consumer reads from just one partition, we can ensure the [order of the reading][kafka-order]{:target="_blank" rel="noopener"}, but when a single consumer reads from two or more partitions, it will read in parallel, so, there's no guarantee of the reading order.
- The ideal is to have the same number of consumers in a group that we have as partitions in a topic, in this way, every consumer read from only one.
- When adding consumers to a group, you need to be careful, if the number of consumers is greater than the number of partitions, some consumers will not read from any topic and will stay idle.

<h3>Retention</h3>

- `Events` can stay in kafka, after consumed (this is different than AMQ), even `7 days`, retention period is configurable. To never have the data deleted, you can use `-1`
- Compact topic: when we are only interested in latest item
- `KIP-405`: use to implement kafka tiered storage feature

<h3>Schema registry</h3>

- consumer => json schema - need to know what is stored in topic / based on contract
- Consumers need a `json schema` to know what is stored in a particular topic.
- This is based on a contract, `schema registry`
- Make sure to put `magic byte` in front of a message that goes from producer to consumer

Typical Kafka-based architectures, schema registries are usually managed by a dedicated component such as Confluent Schema Registry or other schema management tools. Confluent Schema Registry uses an internal topic called _schemas to store schema data, and schemas are accessed via the registry REST API, not directly from Kafka topics.

[kafka-order]: https://medium.com/swlh/apache-kafka-what-is-and-how-it-works-e176ab31fcd5#a178
[kafka-consumer]: https://kafka.apache.org/20/documentation.html#intro_consumers