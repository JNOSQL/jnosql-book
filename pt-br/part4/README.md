## Introdução ao Artemis



![](../../images/duke-artemis-min.png)



O Artemis é a camada de abstração, ou seja, o seu objetivo é ser algo semelhante ao JPA, realizar o mapeamento do Objeto e convertê-lo para o modelo do Diana.

É nessa camada que ficará toda a integração com outras tecnologias como Bean Validation, por exemplo. O Artemis é dotado de anotações o que facilita a vida de um desenvolvedor Java. Assim como o Diana, a camada de comunicação, ele precisa ser extensível, uma vez que essa característica é muito importante para os bancos não relacionais, ou seja, ela terá uma API comum, porém, será extensível para adicionar recursos específicos para um determinado banco de dados.

Com o intuito de abranger os quatro tipos de banco de dados, essa API é comporta por quatro domínios, cada domínio abrange um tipo de banco de dados.

* `org.jnosql.artemis.column`

* `org.jnosql.artemis.document`

* `org.jnosql.artemis.graph`

* `org.jnosql.artemis.key`



