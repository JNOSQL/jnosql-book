## Diana Introduction

With the strategy to divide and conquer on JNoSQL. The Diana was born, it has the goal to be the communication layer easy and extensible. The extensibility is more than important, that is entirely necessary once the API must support specific feature at each database. Nonetheless, the advantage of a common API is a change to another database provider has lesser than using the specific API.

Com o intuito de abranger os quatro tipos de banco de dados, essa API é comporta por quatro domínios, cada domínio abrange um tipo de banco de dados.

* `org.jnosql.diana.column`
* `org.jnosql.diana.document`
* `org.jnosql.diana.graph`
* `org.jnosql.diana.key`

Ou seja, caso um determinado banco de dados seja multi-model, ele usará duas APIs, uma para cada tipo de banco de dados. Além de possuir cada API, para cada tipo, existe também o Technology Compatibility Kit, o TCK. Cada TCK tem como objetivo de realizar os testes para verificar se uma implementação está ou não compatível com a API. Mesmo com APIs diferentes, houve uma tentativa de se utilizar uma nomenclatura comum entre elas:

* Configuration
* Factory
* Manager
* Entity
* Value



