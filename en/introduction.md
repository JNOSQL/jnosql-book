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

### Comparando com as aplicações Java que utilizam bancos relacionais

É uma boa prática ter uma camada que é responsável por realizar a comunicação entre o banco de dados e o modelo, o bom e velho Data Acess Object ou DAO. Essa camada contém toda a API de comunicação com o banco de dados, olhando no mundo relacional, existem diversos vendors desse tipo de banco de dados, porém, com o padrão JPA o desenvolvedor Java tem algumas vantagens:

* Não existe lock-in vendor, ou seja, com o padrão a mudança acontece de maneira bem simples e transparente, apenas é necessário trocar o driver.
* Não é necessário aprender uma nova API para um novo banco de dados uma vez que a API é comum entre todos os bancos de dados.
* Impacto praticamente zero em realizar a mudança de banco de dados, em alguns momentos é necessário utilizar um recurso específico de um banco de dados.

  Nos bancos de dados NOSQL como não existe nenhum padrão pré estabelecido atualmente, assim os desenvolvedores Java enfrentam os seguintes problemas:

* Lock-in verdor

* Para um novo banco de dados é necessário aprender uma nova API.

* Para qualquer mudança de banco de dados o impacto é altíssimo, se perde praticamente toda a camada DAO uma vez que a API muda completamente. Isso acontece mesmo que a mudança seja para o mesmo tipo de banco NOSQL, família de coluna para família de coluna.

Com esse problema, existe um grande esforço ao criar uma API comuns entre esses bancos de dados. É o caso do Spring Data, Hibernate ORM e o TopLink. Como a API JPA já é uma camada muito conhecida entre os desenvolvedores Java, ele é comumente utilizada para facilitar o mapeamento, porém, o seu foco é para os bancos relacionais, assim ele não é suficiente para cobrir todos os casos desses bancos, por exemplo, muitos bancos NOSQL não tem transação ou com essa API não é possível realizar a inserção de forma assíncrona. Assim, infezlimente apesar de o JPA ser uma boa Api ela não contempla todos os comportamentos existentes nos bancos não relacionais.

Muitos bancos não relacionais vem surgindo no mundo do desenvolvimento de software, além do seu uso no mudno Java, por exemplo, na última pesquisa sobre Java EE o número de aplicações que usavam essa tecnolgia para armazenamento chegava a quase 50%. Permitir a criação do padrão facilitará a visa do desenvolvedor Java, uma vez que não será necessário aprender uma nova API ou a facilitar em realizar a mudança do banco de dados sem ser necessário aprender uma nova API. Porém, assim como nos bancos relacionais, utilizar recursos específicos dos bancos de dados não trará suporte para API, mas o que acontece nas aplicações normais é que boa parte do código é padronizável, ou seja, mesmo que o custo da migração não seja zero, será um número bem menor comparado o atualmente.

