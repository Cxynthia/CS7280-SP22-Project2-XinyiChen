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

#### Create Data: INSERT

Inserting data for a row using an **INSERT** statement:
```
insert_statement::= INSERT INTO table_name ( names_values | json_clause )
	[ IF NOT EXISTS ]
	[ USING update_parameter ( AND update_parameter )* ]
names_values::= names VALUES tuple_literal
json_clause::= JSON string [ DEFAULT ( NULL | UNSET ) ]
names::= '(' column_name ( ',' column_name )* ')'
```
For example:
```
INSERT INTO NerdMovies (movie, director, main_actor, year)
   VALUES ('Serenity', 'Joss Whedon', 'Nathan Fillion', 2005)
   USING TTL 86400;

INSERT INTO NerdMovies JSON '{"movie": "Serenity", "director": "Joss Whedon", "year": 2005}';
```

#### Read Data: SELECT

Querying data from data using a **SELECT** statement:
```
select_statement::= SELECT [ JSON | DISTINCT ] ( select_clause | '*' )
	FROM `table_name`
	[ WHERE `where_clause` ]
	[ GROUP BY `group_by_clause` ]
	[ ORDER BY `ordering_clause` ]
	[ PER PARTITION LIMIT (`integer` | `bind_marker`) ]
	[ LIMIT (`integer` | `bind_marker`) ]
	[ ALLOW FILTERING ]
select_clause::= `selector` [ AS `identifier` ] ( ',' `selector` [ AS `identifier` ] )
selector::== `column_name`
	| `term`
	| CAST '(' `selector` AS `cql_type` ')'
	| `function_name` '(' [ `selector` ( ',' `selector` )_ ] ')'
	| COUNT '(' '_' ')'
where_clause::= `relation` ( AND `relation` )*
relation::= column_name operator term
	'(' column_name ( ',' column_name )* ')' operator tuple_literal
	TOKEN '(' column_name# ( ',' column_name )* ')' operator term
operator::= '=' | '<' | '>' | '<=' | '>=' | '!=' | IN | CONTAINS | CONTAINS KEY
group_by_clause::= column_name ( ',' column_name )*
ordering_clause::= column_name [ ASC | DESC ] ( ',' column_name [ ASC | DESC ] )*
```
For example:
```
SELECT name, occupation FROM users WHERE userid IN (199, 200, 207);
SELECT JSON name, occupation FROM users WHERE userid = 199;
SELECT name AS user_name, occupation AS user_occupation FROM users;

SELECT time, value
FROM events
WHERE event_type = 'myEvent'
  AND time > '2011-02-03'
  AND time <= '2012-01-01'

SELECT COUNT (*) AS user_count FROM users;
```

#### Write Data: UPDATE

Updating a row using an **UPDATE** statement:
```
update_statement ::=    UPDATE table_name
                        [ USING update_parameter ( AND update_parameter )* ]
                        SET assignment( ',' assignment )*
                        WHERE where_clause
                        [ IF ( EXISTS | condition ( AND condition)*) ]
update_parameter ::= ( TIMESTAMP | TTL ) ( integer | bind_marker )
assignment: simple_selection'=' term
                `| column_name'=' column_name ( '+' | '-' ) term
                | column_name'=' list_literal'+' column_name
simple_selection ::= column_name
                        | column_name '[' term']'
                        | column_name'.' field_name
condition ::= `simple_selection operator term
```
For example:
```
 UPDATE NerdMovies USING TTL 400
   SET director   = 'Joss Whedon',
       main_actor = 'Nathan Fillion',
       year       = 2005
 WHERE movie = 'Serenity';

UPDATE UserActions
   SET total = total + 2
   WHERE user = B70DE1D0-9908-4AE3-BE34-5573E5B09F14
     AND action = 'click';
```

#### Delete Data: DELETE

Deleting rows or parts of rows using **DELETE** statement:
```
delete_statement::= DELETE [ simple_selection ( ',' simple_selection ) ]
	FROM table_name
	[ USING update_parameter ( AND update_parameter# )* ]
	WHERE where_clause
	[ IF ( EXISTS | condition ( AND condition)*) ]
```
For example:
``` 
DELETE FROM NerdMovies USING TIMESTAMP 1240003134
 WHERE movie = 'Serenity';

DELETE phone FROM Users
 WHERE userid IN (C73DE1D3-AF08-40F3-B124-3FF3E5109F22, B70DE1D0-9908-4AE3-BE34-5573E5B09F14);
```

## Reference

---

https://cassandra.apache.org
