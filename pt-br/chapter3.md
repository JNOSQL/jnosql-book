## Introdução ao Diana


Com o intuito de separar melhor as camadas de abstração e de comunicação entre os bancos não relacionais. Nasceu a API Diana, o seu objetivo é criar essa tal camada de comunicação de uma maneira simples, além de ser extensível. A extensibilidade é algo muito importante para os bancos não relacionais uma vez que, apesar de terem alguns comportamentos comuns, é muito importante que seja possível utilizar recurso específicos do banco de dados. A sua vantagem se encontra em ser uma API comum, assim caso seja necessário realizar a mudança o banco de dados o impacto em aprender uma nova API é muito menor.

Com o intuito de abranger os quatro tipos de banco de dados, essa API é comporta por quatro domínios, cada domínio abrange um tipo de banco de dados. 

* `org.jnosql.diana.column`
* `org.jnosql.diana.document`
* `org.jnosql.diana.graph`
* `org.jnosql.diana.key`