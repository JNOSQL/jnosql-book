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

A huge advantage of this strategy that one change, either JDBC or JPA, can happen quickly. When a developer changes a database, he just needs the switch to a respective driver by a database and done! Code ready to a new database changed.

In a NoSQL database, there isn't a strategy to save code or little impact for a change. All APIs are different and don't follow any one standard, so one change to new database results in a lot of work. There are some solutions such as Spring Data, Hibernate OGM, TopLink NoSQL but it's at a high level. In other words, if this high-level API hasn't support to a particular database the result going to be either changing a high-level API or use the API from NoSQL database directly, so lost a lot of code. This solution has several issues:

* The database vendor need to be worried about the high-level abstraction to Java world

* The solution provider needs to be concerned about the low level of communication with a particular database. \* The database vendor needs to “copy” this communication solutions to all Java vendors.

* To a Java developer there are two lock-in types: If a developer uses an API directly for a change, it will lose code. If a developer uses a high-level abstraction, this developer has lock-in in a Java solution because if this high level hasn't support to a particular NoSQL database, the developer needs to change to either Java solution or use an API NoSQL directly.

The solve this problem the API should have two layers: 

* The communication layer: the driver from a particular database that connects Java to an accurate database. This layer has four specializations, one to each NoSQL type. 
*  The abstraction level: its duty is to high concept to Java developers, this layer has annotations and integration to other specializations.



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

