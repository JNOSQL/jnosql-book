## Está na hora de falar de padrões para o NOSQL em Java

Os bancos de dados NoSQL realizam operação de inserção e recuperação de dados utilizando outro modelo que não seja o relacional. Esses bancos tem como principais características velocidade e alta taxa de escalabilidade, eles estão sendo adotados com maior frequência em diversos tipos de aplicações, inclusive, aplicações para as instituições financeiras. Como consequência, cresce também o número de fornecedores para esse tipo de banco de dados.

Basicamente os bancos de dados NOSQL são classificados em quatro grupos que são definidos pelo seu modelo de armazenamento:

### Chave-valor

Possui uma estrutura muito semelhante à do java.util.Map, onde podemos armazenar uma chave e seu valor. Normalmente esse valor pode ser qualquer informação.

###### Exemplos:

* ###### AmazonDynamo
* AmazonS3 
* Redis 
* Scalaris 
* Voldemort 



| Estrutura relacional | Estrutura chave-valor |
| :--- | :--- |
| Table | Bucket |
| Row | Key/value pair |
| Column | ---- |
| Relationship | ---- |

### Orientado a documentos

Este modelo permite armazenar qualquer documento, sem ter a necessidade de definir previamente sua estrutura. O documento é composto por inúmeros campos, com tipos de dados diversos, inclusive um campo pode conter um outro documento, um banco de dados NoSQL orientado a documentos possui uma estrutura semelhante a de um arquivo XML.

###### Exemplos:

* AmazonSimpleDb 
* ApacheCouchdb 
* MongoDb 
* Riak 



| Estrutura relacional | Estrutura de documentos |
| :--- | :--- |
| Table | Collection |
| Row | Document |
| Column | Key/value pair |
| Relationship | Link |

### Família de colunas

Esse modelo se tornou popular através do paper BigTable do Google, com o objetivo de montar um sistema de armazenamento de dados distribuído, projetado para ter um alto grau de escalabilidade e de volume de dados.

###### Exemplos:

* Hbase
* Cassandra
* Scylla
* Clouddata
* SimpleDb
* DynamoDB



| Estrutura relacional | Estrutura de família de colunas |
| :--- | :--- |
| Table | Column Family |
| Row | Column |
| Column | Key/value pair |
| Relationship | not supported |

### Grafos

É uma estrutura de dados que conecta um conjunto de vértices através de um conjunto de arestas. Os bancos modernos dessa categoria suportam estruturas de grafo multi-relacionais, onde existem diferentes tipos de vértices \(representando pessoas, lugares, itens\) e diferentes tipos de arestas.

###### Exemplos:

* Neo4j 
* InfoGrid 
* Sones 
* HyperGraphDB

| Estrutura relacional | Estrutura de grafos |
| :--- | :--- |
| Table | Vertex and Edge |
| Row | Vertex |
| Column | Vertex and Edge property |
| Relationship | Edge |

### Multi-model database

Alguns bancos de dados possuem a comum característica de ter suporte de um ou mais modelos apresentados anteriormente.

###### Exemplos:

* OrientDB
* Couchbase

### Comparando com as aplicações Java que utilizam bancos relacionais

É uma boa prática ter uma camada que é responsável por realizar a comunicação entre o banco de dados e o modelo, o bom e velho Data Acess Object ou DAO. Essa camada contém toda a API de comunicação com o banco de dados, olhando para o paradigma dos bancos relacionais, existem diversos vendors, porém, com o padrão JPA o desenvolvedor Java tem algumas vantagens:

* Não existe lock-in vendor, ou seja, com o padrão a mudança acontece de maneira bem simples e transparente, apenas é necessário trocar o driver.
* Não é necessário aprender uma nova API para um novo banco de dados uma vez que a API é comum entre todos os bancos de dados.
* Impacto praticamente zero ao mudar de vendor para outro, em alguns momentos é necessário utilizar um recurso específico de um banco de dados, mas mesmo nesses casos não se perte toda a camada DAO.

  Nos bancos de dados NOSQL não existe nenhum padrão pré estabelecido atualmente, assim os desenvolvedores Java enfrentam os seguintes problemas:

* Lock-in verdor

* Para um novo banco de dados é necessário aprender uma nova API.

* Para qualquer mudança de banco de dados o impacto é altíssimo, se perde praticamente toda a camada DAO uma vez que a API muda completamente. Isso acontece mesmo que a mudança ocorra no mesmo grupo de banco NOSQL, família de coluna para família de coluna.

Com esse problema, existe um grande esforço ao criar uma API comuns entre esses bancos de dados. É o caso do Spring Data, Hibernate ORM e o TopLink. Como a API JPA já é uma camada muito conhecida entre os desenvolvedores Java, ela é comumente utilizada para facilitar o mapeamento, porém, o seu foco é para os bancos relacionais, por este motivo a JPA não é suficiente para cobrir todas as necessidades dos bancos NOSQL, por exemplo, muitos bancos NOSQL não possuem transação e com a API JPA não é possível realizar a inserção de forma assíncrona. Assim, infelizmente apesar de a JPA ser uma boa API ela não contempla todos os comportamentos existentes nos bancos não relacionais.

Muitos bancos não relacionais vem surgindo no mundo do desenvolvimento de software, além do seu uso no mundo Java, por exemplo, na última pesquisa sobre Java EE o número de aplicações que usavam essa tecnolgia para armazenamento chegava a quase 50%. Permitir a criação do padrão facilitará o trabalho do desenvolvedor Java, uma vez que não será necessário aprender uma nova API ou irá facilitar na mudança de vendor para outro porque não sera necessário aprender uma nova API. Porém, assim como nos bancos relacionais, utilizar recursos específicos de um banco de dados não trará suporte para API, mas geralmente as aplicações costumam utilizar código padronizável, ou seja, mesmo que o custo da migração não seja zero, será em uma escala bem menor comparado o atualmente.
