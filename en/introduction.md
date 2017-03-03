## Let's talk about standard to NoSQL database in Java

The NoSQL DB is a database that provides a mechanism for storage and retrieval of data which is modeled by means other than the tabular relations used in relational databases. These databases have speed and high scalability. This kind of database has becoming more popular in several applications, that include financial one. As result of the increase, the number of a user the number of vendors is increasing too.

The NoSQL database is defined basically by its model of storage, those have four kinds:

### Key-value

This database has a structure look like a java.util.Map API, where we can storage any value from a key.

###### Examples:

* ###### AmazonDynamo
* AmazonS3 
* Redis 
* Scalaris 
* Voldemort 

| Relational structure | Key-value structure |
| :--- | :--- |
| Table | Bucket |
| Row | Key/value pair |
| Column | ---- |
| Relationship | ---- |

### Document collection

This model can storage any document, without this model be defined previously their structure. This document may be composed of numerous fields, with many kinds of data, that include a document inside another document. This model looks like either XML or JSON file.

###### Examples:

* AmazonSimpleDb 
* ApacheCouchdb 
* MongoDb 
* Riak 

| Relational structure | Document Collection structure |
| :--- | :--- |
| Table | Collection |
| Row | Document |
| Column | Key/value pair |
| Relationship | Link |

### Column Family

This model became popular with the BigTable's paper by Google, with the goal of being a distributed system storage, projected to have either a high scalability and volume.

###### Examples:

* Hbase
* Cassandra
* Scylla
* Clouddata
* SimpleDb
* DynamoDB

| Relational structure | Column Family structure |
| :--- | :--- |
| Table | Column Family |
| Row | Column |
| Column | Key/value pair |
| Relationship | not supported |

### Graph

In computing, a graph database is a database that uses graph structures for semantic queries with nodes, edges, and properties to represent and store data.

###### Examples:

* Neo4j 
* InfoGrid 
* Sones 
* HyperGraphDB

| Relational Structure | Graph structure |
| :--- | :--- |
| Table | Vertex and Edge |
| Row | Vertex |
| Column | Vertex and Edge property |
| Relationship | Edge |

### Muli-model database

Some database has support for more than one kind of model storage this is the multi-model database.

###### Examples:

* OrientDB
* Couchbase

### Standard in SQL

Looking to Java application that uses a relational database. It's a good practice have a layer to be a bridge between a Java application and relationship database: a DAO, the data access object. Talking more about relational database there are APIs such as JPA and JDBC that have some advantages to a Java developer:

* There isn't a lock-in vendor, in other words, with the standard, a database change gonna happen easier and transparency because we just need to change a simple driver.

* There isn't necessary to learn a new API for each new database,  once there is a common database communication.

* There isn't impact in that change.

Currently in NoSQL database hasn't standard so a Java developer has some issues:

* Lock-in vendor

* To each new database is necessary to learn a new API, any change to another database there is a high impact, once all the communication layer gonna be lost once there isn't a standard API. This happens even with the same kind of NoSQL database, for example, a change in a column to another column.

There is a massive effort to create a common API to make the Java developers life easier, such as Spring Data, Hibernate ORM, and TopLink. The JPA is an API popular in Java world, this is why all solutions try to use it, however, this API is created to SQL and not to NoSQL, and it doesn't support all behavior in NoSQL database, many NoSQL hasn't a transaction, and many NoSQL database hasn't support to asynchronous insertion.

The solution for this case creates a specification that covers the four kinds of NoSQL database. The new API should look like the JPA, once the developer has familiarity with this API, besides adding new behavior and new exceptions, when a database has not support to a specific resource. Besides the API, another important point is integration with others Java specifications such as CDI and Bean Validation. 

