# Eclipse JNoSQL One API to many NoSQL databases

### JNoSQL

Eclipse JNoSQL is a Java framework that streamlines the integration of Java applications with NoSQL databases. It defines a set of APIs to interact with NoSQL databases and provides a standard implementation for most NoSQL databases. This clearly helps to achieve very low coupling with the underlying NoSQL technologies used.

The project has two layers:

* **Communication Layer**: This is a set of APIs that defines communication with NoSQL databases. In traditional SQL/RDBMS world, these can be compared with the JDBC APIs. This API set contains four modules, with each one representing a NoSQL database storage type: Key-Value, Column Family, Document and Graph.
* **Mapping Layer**: These are the APIs that help developers to map Java objects to NoSQL databases. This layer is annotation driven and uses technologies like CDI and Bean Validations to make it simple for the developer. In traditional SQL/RDBMS world, this layer can be compared to JPA and ORM frameworks.

## Key features

* Simple APIs supporting all well-known NoSQL storage types - Column Family, Key-Value Pair, Graph and Document databases.
* Use of Convention Over Configuration
* Support for Asynchronous Queries
* Support for Asynchronous Write operations
* Easy-to-implement API Specification and Test Compatibility Kit \(TCK\) for NoSQL Vendors
* The API's focus is on simplicity and ease of use. Developers should only have to know a minimal set of artifacts to work with JNoSQL. The API is built on Java 8 features like Lambdas and Streams and therefore fits perfectly with the functional features of Java 8+.

### Eclipse JNoSQL - Diana

The Eclipse JNoSQL - Diana project defines the standard APIs to communicate with NoSQL databases - this project works as a NoSQL Database jDriver.

Diana has four APIs, one for each NoSQL database storage type, and a TCK for each one. The Test Compatibility Kit \(TCK\) helps ensure that driver implementations adhere to API specifications. So if a key-value database driver implements and pass all its tests, it means that this database driver support the Diana key-value API.

### Eclipse JNoSQL - Artemis

The Eclipse JNoSQL - Artemis project is an integration and mapping layer that helps developers integrate applications and works with Diana. The Artemis layer uses technologies such as Bean Validations and incorporates CDI capabilities, making integrations very simple and effective. In other words:

Diana + CDI = Artemis

Similar to Diana, Artemis also has separate modules for all well known NoSQL Database Storage types. With CDI at its heart, Artemis is a very powerful, yet simple, framework.

Key features of Artemis:

Annotation Driven. Highly Customizable \(reflection, caching, persistence flow, etc.\) Observable events on the persistence flow Support for Interceptors, Injection and Validation

