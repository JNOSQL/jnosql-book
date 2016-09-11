## Introdução ao Diana.


Com o intuito de separar melhor as camadas de abstração e de comunicação entre os bancos não relacionais. Nasceu a API Diana, o seu objetivo é criar essa tal camada de comunicação de uma maneira simples, além de ser extensível. A extensibilidade é algo muito importante para os bancos não relacionais uma vez que, apesar de terem alguns comportamentos comuns, é muito importante que seja possível utilizar recurso específicos do banco de dados. A sua vantagem se encontra em ser uma API comum, assim caso seja necessário realizar a mudança o banco de dados o impacto em aprender uma nova API é muito menor.


Com o intuito de abranger os quatro tipos de banco de dados, essa API é comporta por quatro domínios, cada domínio abrange um tipo de banco de dados. 

* org.jnosql.diana.column
* org.jnosql.diana.document
* org.jnosql.diana.graph
* org.jnosql.diana.key

Ou seja, caso um determinado banco de dados seja multi-model, ele usará duas APIs, uma para cada tipo de banco de dados. Além de possuir cada API, para cada tipo, existe também o Technology Compatibility Kit, o TCK. Cada TCK tem como objetivo de realizar os testes para verificar se uma implementação está ou não compatível com a API. Mesmo com APIs diferentes, houve uma tentativa de se utilizar uma nomenclatura comum entre elas:

* Configuration
* Factory
* Manager
* Entity
* Value
