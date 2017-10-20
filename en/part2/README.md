## Diana Introduction

With the strategy to divide and conquer on JNoSQL. The Diana was born, it has the goal to be the communication layer easy and extensible. The extensibility is more than important, that is entirely necessary once the API must support specific feature at each database. Nonetheless, the advantage of a common API is a change to another database provider has lesser than using the specific API.

To cover the four kinds of database, this API has four packages, one for each data bank.

* `org.jnosql.diana.column`
* `org.jnosql.diana.document`
* `org.jnosql.diana.graph`
* `org.jnosql.diana.key`

So, if a database is multi-model, has support to more than one database, it will implement an API to each database which it supports. Also, each API has the TCK to prove if the database is compatible with the API. Even from differents JSR it tries to use the same nomenclature:

* Configuration
* Factory
* Manager
* Entity
* Value



