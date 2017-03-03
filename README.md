## JNOSQL

## 

The JNoSQL is a several tools to make easy an integration between the Java Application with the NoSQL. To solve this problem the project gonna have two layers:

* **Communication API:**
  An API just to communicate with the database, exactly what JDBC does to SQL. This API gonna have four specializations, one for each kind of database.
* **Abstraction API:**
  An API to do integration and do the best integration with the Java developer. That gonna be annotation drive and gonna have integration with other technologies like Bean Validation, etc. To solve it this layer gonna be a CDI based.

The basic building blocks hereby are:

* A simple API to support Column NoSQL Database
* A simple API to support Key-value NoSQL Database
* A simple API to support Graph NoSQL Database
* A simple API to support Document Database
* Convention over configuration
* Support for asynchronous queries
* Support for asynchronous write operations
* An easy API to implement, so that NoSQL vendors can comply with it and test by themselves.

The API's focus is on simplicity and ease of use. Developers should only have to know a minimal set of artifacts to work with the solution. The API is built on latest Java 8 features and therefore fit perfectly with the functional features of Java 8.

## Diana

The Diana project has as goal just be the low layer, in other words, just the communication layer to NoSQL database. Basically this project gonna work as a database driver. Diana will have four APIs, one for each database type, and its TCK respective. The test compatibility kit affirms if a driver implements an API correctively. So a X database of key-value implements and run all tests correctively that means this X database has support to key-value Diana API.

The main reason to Diana just works in communication layer are:

* A developer doesn't want to learn a new API beyond JPA.
* The abstraction layer makes sense as a JPA extension and not a new one.
* Be a communication API is good enough to be a project.

Furthermore Diana **will not**be:

* A new API to replace JPA
* A new API to abstraction layer
* Just one API communication to solve all kind of NoSQL database
* Be responsible for doing integrations with other technologies such as CDI, EJB, Bean Validation, Spring, etc.

## Artemis

Artemis is an integration layer, in other words, it has the goal to communicate with the communication layer, Diana, and it does integrations with other technologies such as Bean Validation. The Artemis engine has CDI. So it formula is really simple:

#### Diana plus CDI equals to Artemis

Look like Diana, it has a different package to each NoSQL database. Using CDI as heart, Artemis is highly customizable also observe events on the persistence flow. Artemis has a nice feature such as:

* Annotation based
* Highly customizable \\(reflection component, how to save the cache, persistence workflow, etc.\\)
* Listen to an event to each kind of persistence workflow \\(it has differents kind of events to each database\\).
* Use interceptors

Using CDI events to add a new feature on Artemis is so easy and you also can it on transparency way, you don't need to change the repositories. E.G: Using events can add bean validation on the workflow, without the DAO layer know.

