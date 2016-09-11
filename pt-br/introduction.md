## Está na hora de falar de padrões para o NOSQL em Java



   Os bancos NoSQL são bancos de dados que realizam operação de inserção e recuperação de dados utilizando outro modelo que não seja o relacional. Esses tipos de bancos se caracterizam pela velocidade e alta taxa de escalabilidade e vem sendo utilizado com maior frequência em diversos tipos de aplicações, inclusive, aplicações para as instituições financeiras. Como consequência, cresce também o número de vendors ou distribuidores para esse tipo de banco de dados.


Os bancos de dados NOSQL são definidos basicamente pelo seu modelo de armazenamento que são  quatro:

### Chave-valor

Possui uma estrutura muito semelhante à do java.util.Map, onde podemos armazenar uma chave e seu valor. Normalmente esse valor pode ser qualquer informação.

* AmazonDynamo 
* AmazonS3 
* Redis 
* Scalaris 
* Voldemort 

### Orientado a documentos

   Este modelo permite armazenar qualquer documento, sem ter a necessidade de definir previamente sua estrutura. O documento e composto por inúmeros campos, com tipos de dados diversos, inclusive um campo pode conter um outro documento, possui uma estrutura semelhante a um arquivo XML. 

* AmazonSimpleDb 
* ApacheCouchdb 
* MongoDb 
* Riak 

### Família de colunas

  Esse modelo se tornou popular através do paper BigTable do Google, com o objetivo de montar um sistema de armazenamento de dados distribuído, projetado para ter um alto grau de escalabilidade e de volume de dados. 

* Hbase
* Cassandra
* Scylla
* Clouddata
* SimpleDb
* DynamoDB

### Grafos

   É uma estrutura de dados que conecta um conjunto de vértices através de um conjunto de arestas. Os bancos de dados de grafo modernos suportam estruturas de grafo multi-relacionais, onde existem tipos diferentes vértices (representando pessoas, lugares, itens) e diferentes tipos de arestas.
   
* Neo4j 
* InfoGrid 
* Sones 
* HyperGraphDB

### Muli-model database

Alguns bancos de dados possuem a comum característica de ter suporte de um ou mais modelos apresentados anteriormente.

* OrientDB
* Couchbase

###  Comparando com as aplicações Java que utilizam bancos relacionais
  
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