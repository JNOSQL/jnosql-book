## Artemis introduction

![](../../images/duke-artemis-min.png)

The Eclipse JNoSQL Artemis project is the mapping level, to put it differently, it has the same goals of the either JPA or ORM to NoSQL world, which converts the entity object to Diana model.

This level is in charge to do integration among technologies such as Bean Validation. The  Artemis has annotations that make the Java developer life easier. As Diana project, it must be extensible and configurable to keep the diversity on NoSQL database.

To go straight and cover the four NoSQL types, this API has four domains:

* `org.jnosql.artemis.column`

* `org.jnosql.artemis.document`

* `org.jnosql.artemis.graph`

* `org.jnosql.artemis.key`

#### The Artemis project

Artemis has six parts:

* *The **artemis-core**: The Eclipse JNoSQL mapping, Artemis, commons project.
* The **artemis-configuration**: The Eclipse JNoSQL reader to Artemis project.
* The **artemis-column**: The Eclipse JNoSQL mapping, Artemis, to column NoSQL database.
* The **artemis-document**: The Eclipse JNoSQL mapping, Artemis, to document NoSQL database.
* The **artemis-key-value**: The Eclipse JNoSQL mapping, Artemis, to key-value NoSQL database.
* The **artemis-validation**: The Eclipse JNoSQL mapping, Artemis, that offers support to Bean Validation

* **Artemis-extension**: Like Diana, there is a support for database diversity. This project has extensions to the each database type on the database mapping level.



