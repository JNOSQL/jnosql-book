### JNoSQL

The JNoSQL has several tools to make an easy integration between the Java Applications and NoSQL databases. To solve this problem the project has two layers:

* **Communication API: **An API just to communicate with the database, exactly what JDBC does to SQL. This API has four specializations, one for each kind of database.
* **Mapping API: **An API to do integration and do the best integration with the Java developer. It's annotation driven and integrated with other technologies like Bean Validation, etc, and to solve it this layer is CDI based.
