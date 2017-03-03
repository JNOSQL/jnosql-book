## The main idea behind the API

Once, we talked about the importance of the standard of a NoSQL database API; the next step is to discuss, in more details, about API. However, to make a natural explanation, first going to talk about both layer and tier. These structures level make the communication, maintenance, split the responsibility clearer. The new API proposal  is going to be responsible for being a bridge between the logic tier and data tier, to do this, we need to create two APIs one to communication to a database and another one to be a high abstraction to Java application.

![Camada Física](../../images/01.png)

In software, the world is common that application has structures: tier, physical structure, and layer, logic one. The multi-tier application has three levels:

* **Presentation tier**: That has as primary duty translate the result, from below tiers, to the user a can understand.

* **Logic tier**: The tier where has all business rules, process, conditions, save the information, etc. This level moves and processes information between other levels.

* **Data tier**: Storage and retrieve information either database or a system file.

Falando precisamente da camada física, tier, lógica, para separar as responsabilidades, existem as camadas, layer, lógicas. Essas camadas contêm a ponte entre a camada física de apresentarão e a camada de dados além da lógica de negócio. Indiferente do seu padrão de arquitetura \(MVC, HMVC, PAC, MVA, MVP, MVVM\) eles possuem no mínimo quatro camadas:

* **Camada de aplicação**: A ponte para a camada física de apresentação, por exemplo, responsável por transformar o objeto em JSON ou XML.
* **Camada de serviço**: A camada de serviço, a depender da tecnologia utilizada pode ser um Controller ou um Resource.
* **Camada de negócio**: Local onde contém toda a lógica de negócio e o modelo.
* **Camada de persistência**: A ponte entre a camada física de acesso aos dados e a camada física lógica.

Within a persistence layer, it has its layers: A Data Access Object, DAO, this structure connect business layer and persistence layer. Inside it has an API that does database. Currently, there is a difference between SQL and NoSQL database:

In the relational database there are two mechanisms, beyond DAO, JDBC, and JPA:



* **JDBC**: a deep layer with a database that has communications, basic transactions, basically it's a driver to a particular database.

* **JPA**: A high layer that has communication either JDBC and JPA. This layer has high abstraction to Java; this place has annotations and an EntityManager. In general, a JPA has integration with other specifications such as CDI and Bean Validation.



A vantagem dessas camadas é que a mudança, seja do driver ou do JPA, acontece de forma transparente. Por exemplo, ao realizar a mudança do banco de dados, basta apenas trocar pelo seu respectivo driver e a camada de JPA e o DAO ficam intactas. Assim, caso exista um novo banco de dados, basta que o distribuidor crie apenas o respectivo JDBC sem se preocupar com as outras camadas. O mesmo acontece com o JPA, caso um destruidor de solução técnica para Java queria criar o seu próprio JPA, ele não precisa se preocupar com os detalhes de cada banco de dados, apenas focar na solução de alto nível com o JPA.

No mundo dos bancos NoSQL, infelizmente, isso não acontece. Como todas as APIs são diferentes toda mudança de banco resulta na troca de API, assim uma grande perda de código. A solução feita atualmente \(Spring Data, Hibernate OGM, TopLink NOSQL, etc.\) é que essa camada de alto nível seja responsável por realizar essa comunicação entre o banco de dados e a aplicação Java, assim temos alguns problemas:

* O distribuidor de banco de dados, precisa se preocupar com o código de alta abstração de acesso de Java.

* O distribuidor da solução Java, precisa se preocupar com o código de baixo nível para realizar o acesso ao banco de dados.

* O distribuidor do banco de dados, precisa replicar a solução para todos provedores de solução Java.

* O desenvolvedor Java, fica preso a uma solução de alto nível para facilitar o seu código.

* Caso a camada de alto nível não tenha suporte para o banco de dados o desenvolvedor terá que ou mudar de solução Java ou mesmo fazer a chamada diretamente pela API do banco de dados, ou seja, haverá uma grande perda de código.

A solução para esse problema é , assim como no mundo relacional, ter duas camadas de API:

* Uma camada de baixo nível ou camada de comunicação: que seria o driver de comunicação entre o banco e o Java. Essa camada teria quatro especializações \(uma para cada tipo de banco de dados\).
* Uma camada de alto nível ou camada de abstração: Responsável pela alta abstração para o desenvolvedor Java. É nessa camada que ficar as anotações, o EntityManager, etc.

Com essa abordagem temos algumas vantagens:

* O distribuidor pelo banco de dados, precisa se preocupar com a comunicação e o parser para o Java.
* O distribuidor de solução Java, precisa se preocupar apenas com a API de abstração.
* O desenvolvedor Java não fica preso nem ao banco de dados e nem a API de abstração.

Essas APIs serão opcional uma da outra, em outras palavras, um vendor só precisa se preocupar com a camada do seu interesse.

### Nasce o Projeto JNoSQL

O JNoSQL é uma API flexível e extensiva Java cujo o objetivo é realizar comunicações entre a aplicação java e o banco de dados NoSQL. Assim, o seu foco será criar uma API comum, porém, olhando para a diversidade que existe dentro do mundo NoSQL, mesmo para os bancos do mesmo tipo. Se tem ciência que mesmo um banco de dados sendo do mesmo tipo, família de colunas, existem recursos que são específicos de um determinado banco de dados é o caso, por exemplo, do Cassandra Query Language ou mesmo nível de consistência que existe apenas no Cassandra. A API terá como missão ser extensível e configurável sendo apta para utilizar recursos específicos de um banco de dados. Para atacar esse problema o projeto será composto de duas camadas core:

* **A camada de comunicação:** A API que realiza a comunicação com o banco de dados, em analogia, seria o que o JDBC faz para o SQL. Essa API será subcomposta de inicialmente de quatro, uma para cada tipo de banco de dados NoSQL.

* **A camada de abstração:** Uma API que realiza a integração entre outras ferramentas, assim será o melhor amigo para o desenvolvedor Java. Essa API será focada em anotações e integração com outras tecnologias, por exemplo, Bean validation. O seu coração será baseado em CDI.

#### Diana

![](../../images/duke-diana-min.png)

O projeto Diana tem como objetivo tratar apenas da camada de baixo nível, ou seja, a camada de comunicação com os bancos não relacionais. A ideia que esse projeto funcione como um driver para os bancos de dados não relacionais. De modo geral ela possuirá quatro APIs, uma para cada tipo de banco de dados, além do seu respectivo TCK. O Kit de teste de compatibilidade tem como objetivo afirmar que um determinado banco de dados implementa uma das APIs corretamente, por exemplo, caso o banco X implemente a API de chave valor e passar nos testes de compatibilidade quer dizer que ele está compatível com o a API de chave-valor. O motivo que o projeto abrangerá apenas a API de comunicação são:

* O desenvolvedor não quer aprender uma nova API além do JPA.
* A camada de abstração, faz sentido como extensão do JPA e não criar uma nova.
* O objetivo da camada de comunicação é um escopo bem grande uma vez que se tem diversos tipos de bancos de dados de diversas implementações.
* Facilitar a implementação com as camadas de abstração já existentes, além de iniciar as conversas sobre esse tipo de padronização.

Com isso o projeto Diana **não** será:

* Uma nova API para substituir o JPA
* Uma nova API para camada de abstração
* Apenas uma única API, ignorando as especializações de cada tipo de banco
* Camada responsável por realizar a integração entre as outras especificações como CDI, EJB, Bean Validation, Spring, etc.

  Assim, mesmo com a não responsabilidade de realizar o papel das camadas de abstração, essa camada de comunicação facilitará a entrada do suporte ao banco de dados, assim, por exemplo, caso uma das APIs de abstração como Spring Data, Hibernate OGM, etc. Suporte essa API, basta que o distribuidor do banco de dados implemente o seu respectivo tipo de banco de dados e essa API terá suporte para o novo banco.

  Diana não é valioso apenas utilizado em conjunto com uma camada de abstração, caso o desenvolvedor opte por não utilizar tal API de abstração, ao utilizar a camada de comunicação, a mudança entre os bancos de dados do mesmo tipo será transparente, por exemplo, a mudança de um banco de grafos para outro, será necessário apenas trocar o driver, implementação, do outro banco.

#### Artemis

![](../../images/artemis-integration.png)

O Artemis é a camada de integração, ou seja, ele será responsável por se comunicar com a camada de comunicação, o diana, e realizar a integração com outras tecnologias, como Bean Validation, por exemplo. O seu coração é composto com CDI. Assim, sua fórmula é simples:

Diana mais CDI igual ao Artemis.

Assim como o Diana, ela também possui, de maneira geral, um pacote para cada tipo de banco de dados. Utilizando como base o CDI ele se caracteriza por ser compartimentalizável além de ser possível criar eventos para cada momento da persistência do objeto. Com Artemis será possível:

* Persistir seu objeto utilizando simples anotações

* Substituir o grande número de componentes \(reflections, a conversão da entidade para o banco de dados, como armazena o cache, ciclo de eventos ao persistir uma informação, dentre outros\).

* Ouvir eventos para cada ciclo de persistência no banco de dados \(Diferentes eventos para cada tipo de banco de dados\).

* Criar classes interceptadoras para as classes repositório.

Um ponto importante sobre os eventos no CDI é a facilidade de adicionar novo recursos ou funcionalidades de maneira transparente ao código. Por exemplo, é possível injetar a validação das entidades, bean validation, sem ser necessário mudar nenhum fluxo do código atual. Por padrão, o fluxo de evento segue o seguinte fluxo:

![](../../images/integration-artemis.png)

Cada tipo de banco de dados, possui os seus próprios interceptors e eventos, ou seja, é possível realizar a mudança apenas dos eventos do chave-valor sem modificar escutar os eventos do tipo grafo.

