## Diana Introduction

![](../../images/duke-diana-min.png)

With the strategy to divide and conquer on JNoSQL. The Diana was born, it has the goal to be the communication layer easy and extensible. The extensibility is more than important, that is entirely necessary once the API must support specific feature in each database. Nonetheless, the advantage of a common API is a change to another database provider has lesser than using the specific API.

To cover the four kinds of database, this API has four packages, one for each data bank.

* `org.jnosql.diana.column`
* `org.jnosql.diana.document`
* `org.jnosql.diana.key`

There isn't communication API because of the Graph API already does exist, that is Apache TinkerPop. 

So, if a database is multi-model, has support to more than one database, it will implement an API to each database which it supports. Also, each API has the TCK to prove if the database is compatible with the API. Even from different JSR it tries to use the same nomenclature:

* Configuration
* Factory
* Manager
* Entity
* Value

#### The Diana project

Diana has four parts:

* The **diana-core:** The JNoSQL API communication commons to all types.
* The **diana-key-value:** The JNoSQL communication API layer to key-value database.
* The **diana-column:** The JNoSQL communication API layer to column database.
* The **diana-document:** The JNoSQL communication API layer to document database.


To the Graph communication API, there is the [Apache TinkerPop](http://tinkerpop.apache.org/) that won't be covered in this documentation.
