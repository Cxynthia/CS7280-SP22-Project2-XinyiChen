# Apache Cassandra

---

#### Name: Xinyi Chen
#### NUID: 001537334

---

## Introduction

---

Apache Cassandra is an open source NoSQL distributed database used by thousands of companies for scalability and high
availability without compromising performance. Linear scalability and proven fault-tolerance on commodity hardware or
cloud infrastructure make it the perfect platform for mission-critical data.

Features of Apache Cassandra includes: hybrid, fault tolerance, focus on quality, performance, user in control, security
and observability, distributed, scalability, and elasticity.

Official website: https://cassandra.apache.org/

## Architecture

---

Apache Cassandra was initially designed at Facebook to implement a
combination of **Amazon’s Dynamo** distributed storage and replication techniques and **Google’s Bigtable** data and
storage engine model. Cassandra was designed as a best-in-class combination of both systems to meet emerging large
scale, both in data footprint and query volume, storage requirements.

Cassandra is designed following these design objectives:
- Full multi-master database replication
- Global availability at low latency
- Scaling out on commodity hardware
- Linear throughput increase with each additional processor
- Online load balancing and cluster growth
- Partitioned key-oriented queries
- Flexible schema

#### Dynamo

Apache Cassandra relies on Amazon's Dynamo distributed storage key-value system, especially on the following styles:
- Dataset partitioning using consistent hashing
- multi-master replication using versioned data and tunable consistency
- Distributed cluster membership and failure detection via a gossip protocol
- Incremental scale-out on commodity hardware

#### Storage Engine

Apache Cassandra also relies on the storage engine model. There are several steps to take when writing to the storage:

- **Commitlog**: append-only log of local mutations to a Cassandra node. Any data written will first be written to a
  commit log before being written to a memtable.
- **Memtable**: in-memory structure of Cassandra buffers. In general, there is one active memtable per table. Finally,
  memtables are flushed onto disk and stored as immutable SSTables.
- **SSTable**: SSTables are the immutable data files that Cassandra uses for persisting data on disk.

## Data Model

---

Apache Cassandra stores data in tables, and uses CQL to query the data in table. The data model is based around and
optimized for querying. The data access patterns and application queries determine the structure and organization of
data which is then used to design the database tables.

#### Partitions

Apache Cassandra is a distributed system which stores data across a cluster of nodes. Therefore, a partition key is
needed to distribute data evenly among the cluster. Making the number of partition read for a query as low as possible
is the key to reduce latency and overhead. Because different partitions could be located on different nodes so the
coordinator need to send requests to every node. This would be more efficient though all the partitions involved in on
query are on the same node.

The data model of Cassandra is a conceptual model that must be analyzed and then be optimized based on it. Measurements
used in the analyzation include the partition size, data redundancy, disk space, and the lightweight transactions(LWT).

## Applications

Apache Cassandra is used by various notable organizations, such as Apple, Netflix, Reddit, etc. It has a vast area of
application. Some of the major applications are listed below:

- **Storage**: User can store any kind of data in various nodes provided by Apache Cassandra.
- **Back-end Development Applications**: Cassandra provides a wide platform for the back-end development and a huge
  database for the data.
- **Monitoring**: Developers can use Cassandra to monitor the user activity.
- **Time-series-based Applications**: applications in real time including hits on the internet browser, traffic light
  data, GPS location tracking data, etc.
- **Analytics**: analyse data from various sources including social media, product feedback catalogues, retail inputs
  and lookups.
- **Message Management**: Cassandra provides the platform for the message providers to manage these data.

Applications of Cassandra are not limited to these points, there are many other ways to use this technology.

## Reference

---

https://cassandra.apache.org
https://data-flair.training/blogs/cassandra-applications
