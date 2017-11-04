## Introdução ao Artemis

![](../../images/duke-artemis-min.png)

O projecto Eclipse JNoSQL Artemis é a camada de abstração, ou seja, o seu objetivo é ser algo semelhante ao JPA, realizar o mapeamento do Objeto e convertê-lo para o modelo do Diana.

É nessa camada que ficará toda a integração com outras tecnologias como Bean Validation, por exemplo. O Artemis é dotado de anotações o que facilita a vida de um desenvolvedor Java. Assim como o Diana, a camada de comunicação, ele precisa ser extensível, uma vez que essa característica é muito importante para os bancos não relacionais, ou seja, ela terá uma API comum, porém, será extensível para adicionar recursos específicos para um determinado banco de dados.

Com o intuito de abranger os quatro tipos de banco de dados, essa API é comporta por quatro domínios, cada domínio abrange um tipo de banco de dados.

* `org.jnosql.artemis.column`

* `org.jnosql.artemis.document`

* `org.jnosql.artemis.graph`

* `org.jnosql.artemis.key`

#### Projetos do Artemis

Artemis has six parts:

* *The **artemis-core**: a parte comum do projeto, é nela que contém, por exemplo, as anotaçes.
* The **artemis-configuration**: responsável pela leitura de configuração
* The **artemis-column**: The Eclipse JNoSQL mapping, Artemis, para bancos de dados do tipo colunar
* The **artemis-document**: The Eclipse JNoSQL mapping, Artemis, para banco de dados do tipo documento
* The **artemis-key-value**: The Eclipse JNoSQL mapping, Artemis, para o banco de dados do tipo chave-valor
* The **artemis-validation**: The Eclipse JNoSQL mapping, Artemis, que oferece suporte a bean validation.

* **Artemis-extension**: Assim como a camada de comunicação, Diana,existe suporte para a diversidade. Esse projeto tem para cada banco de dados para capturar comportamentos específicos, caso necessário.



