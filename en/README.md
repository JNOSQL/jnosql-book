### JNoSQL

The JNoSQL has several tools to make an easy integration between the Java Applications and NoSQL databases. To solve this problem the project will have two layers:

* **Communication API: **An API just to communicate with the database, exactly what JDBC does to SQL. This API will have four specializations, one for each kind of database.
* **Abstraction API: **An API to do integration and do the best integration with the Java developer. That will be annotation driven and will have integration with other technologies like Bean Validation, etc, and to solve it this layer will be CDI based.
