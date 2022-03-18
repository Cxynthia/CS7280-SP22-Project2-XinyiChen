# Project 2

---

#### Name: Xinyi Chen
#### NUID: 001537334



## Introduction

---

Apache Cassandra is an open source NoSQL distributed database used by thousands of companies for scalability and high 
availability without compromising performance. Linear scalability and proven fault-tolerance on commodity hardware or 
cloud infrastructure make it the perfect platform for mission-critical data.

Features of Apache Cassandra includes: hybrid, fault tolerance, focus on quality, performance, user in control, security 
and observability, distributed, scalability, and elasticity.

Official website: https://cassandra.apache.org/

## Installation Process

---

### Step 1: Get Cassandra

Apache Cassandra could be downloaded through Docker or similar software. (If Docker is not installed, refer this 
[link](https://docs.docker.com/get-docker/) to first install Docker.) Use the following code to install Cassandra 
through Docker:

```
docker pull cassandra:latest
```

It is also available as a tarball or package download [here](https://cassandra.apache.org/_/download.html).

### Step 2: Start Cassandra

Use the following code to start Cassandra with Docker:

```
docker run --name cassandra cassandra
docker run --rm -d --name cassandra --hostname cassandra --network cassandra cassandra
```

## Operations

---

Cassandra provides the Cassandra Query Language (CQL), an SQL-like language, to create and update database schema and 
access data. CQL allows users to organize data within a cluster of Cassandra nodes using:
- **Keyspace**: Defines how a dataset is replicated, per datacenter. Replication is the number of copies saved per 
cluster. Keyspaces contain tables.
- **Table**: Defines the typed schema for a collection of partitions. Tables contain partitions, which contain rows, 
which contain columns. Cassandra tables can flexibly add new columns to tables with zero downtime.
- **Partition**: Defines the mandatory part of the primary key all rows in Cassandra must have to identify the node in 
a cluster where the row is stored. All performant queries supply the partition key in the query.
- **Row**: Contains a collection of columns identified by a unique primary key made up of the partition key and 
optionally additional clustering keys.
- **Column**: A single datum with a type which belongs to a row.

### Create Database Objects

This script will create a keyspace, the layer at which Cassandra replicates its data, a table to hold the data, and 
insert some data into that table:

#### Create a keyspace:
```
CREATE KEYSPACE IF NOT EXISTS store WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : '1' };
```

#### Create a table
```
CREATE TABLE IF NOT EXISTS store.shopping_cart (
userid text PRIMARY KEY,
item_count int,
last_update_timestamp timestamp
);
```

### CRUD Operations

#### Create Data:
```
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('9876', 2, toTimeStamp(now()));
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('1234', 5, toTimeStamp(now()));
```

#### Read Data:
```
 SELECT * FROM store.shopping_cart;
```

#### Write Data:
```
 INSERT INTO store.shopping_cart (userid, item_count) VALUES ('4567', 20);
```

## Reference

---

https://cassandra.apache.org/_/index.html
https://cassandra.apache.org/_/quickstart.html
