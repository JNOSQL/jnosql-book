	 	 	
   The NoSQL DB is a database that provides a mechanism for storage and retrieval of data which is modeled in means other than the tabular relations used in relational databases. These databases have speed and high scalability. This kind of database has becoming more popular in several applications, that include financial one. As result of increase the number of user the number of vendors are increasing too.


   The NoSQL database is defined basically by the its model of storage, those has four kind:
 	

### Key-value
   This database has a structure look like an java.util.Map API, where we can storage any value from a key.

* AmazonDynamo
* AmazonS3
* Redis
* Scalaris
* Voldemort  
* Couchbase

	 	 	
### Document

   This model can storage any document, without this model be defined previously their structure. This document may be composed by inumerous fields, with many kinds of data, that include a document inside another document. This model is look like either XML or JSON file.

* AmazonSimpleDb
* ApacheCouchdb
* MongoDb
* Riak
* Couchbase
* OrientDB

	 	 	
### Column
   This model became popular with the BigTable's paper by Google, with the goal of be a distributed system storage, projected to have either a high scalability and volume.

* Hbase	
* Cassandra
* Scylla
* Clouddata
* SimpleDb
* DynamoDB

	 	 	
### Graph

   In computing, a graph database is a database that uses graph structures for semantic queries with nodes, edges and properties to represent and store data.

* Neo4j
* InfoGrid
* Sones
* HyperGraphDB
* OrientDB


### Multi-model database

   Some database has support to more than one kind of model storage this is the multi model database.
   
* OrientDB
* Couchbase




Standard in SQL
	 	 	
   Looking to Java application that uses relational database. It's a good practice have a layer to be a bridge between a Java application and relationship database: a DAO, the data access object. Talking more about relational database there are APIs such as JPA and JDBC that have some advantages to a Java developer:

	 	 	
There isn't a lock-in vendor, in other words, with the standard, a database change gonna happen easier and transparency, because we 	just need to change a simple driver.
There isn't necessary to learn a new API for each new database, 	once there is a common database communication.	
There isn't impact in that change.





NoSQL Issues:	
 	 	
 Currently in NoSQL database hasn't standard so a Java developer has some issues:

Lock-in vendor
To each new database is necessary learn a new API.
Any change to another database there is a high impact, once all the communication layer gonna be lost once there isn't a common API. This happen even with the same kind of NoSQL database, for example, a change between a column to another column.

There is a huge effort to create a common API to make the Java developers life easier, such as Spring Data, Hibernate ORM and TopLink. The JPA is an API popular in Java world, this is why all solutions try to use it, however this API was not made to NoSQL and it doesn't support all behavior in NoSQL database, many NoSQL haven't transaction and many NoSQL database haven’t support to asynchronous insertion.

The solution for this case is create a specification that cover the four kind of NoSQL database. The new API should be look like the JPA, once the developer has familiarity with this API, beside add new behavior and new exceptions, when a database has not support to specific resource. Beside the API, another important point is an integration with others Java specifications such as CDI and bean validation. 

Roadmap

The road map to this API creation is:

The apache project creation (Diana, the Greek goddess of the hunt, is possible name).
Review in Java Community.
Development inside an Apache project.
A JSR proposal.


Conclusion

Many NoSQL databases are emerging and also its use by Java developers, in the last survey about Java EE developers, almost 50% of interviewers are already using NoSQL technology. Allow a standard to NoSQL will make easier the life of Java developer once will difficult the lock-in vendor and also is not necessary learn a new API to new database. 



