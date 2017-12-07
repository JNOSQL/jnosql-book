## Introdução ao Diana

![](../../images/duke-diana-min.png)

Com o intuito de separar melhor as camadas de abstração e de comunicação entre os bancos não relacionais. Nasceu a API Diana, o seu objetivo é criar essa tal camada de comunicação de uma maneira simples, além de ser extensível. A extensibilidade é uma característica muito importante para os bancos não relacionais uma vez que, apesar de possuirem alguns comportamentos em comum, é de suma importância para o desenvolvedor que ele consiga utilizar os recursos específicos de cada banco. A vantagem que a Diana trás para o desenvolvedor é  que ela é uma API comum para todos os tipos de bancos não relacionais, assim caso seja necessário realizar uma mudança de banco de dados o impacto em aprender uma nova API é muito menor.

Com o intuito de abranger os quatro tipos de banco de dados, essa API é composta por quatro domínios, cada domínio abrange um tipo específico. 

* `org.jnosql.diana.column`
* `org.jnosql.diana.document`
* `org.jnosql.diana.graph`
* `org.jnosql.diana.key`

Caso um determinado banco de dados seja multi-model, ele usará duas APIs, uma para cada tipo. Além de possuir uma API para cada tipo, existe também o Technology Compatibility Kit, o TCK. Cada TCK tem como objetivo de realizar os testes para verificar se uma implementação está ou não compatível com a API. Mesmo com APIs diferentes, houve uma tentativa de se utilizar uma nomenclatura comum entre elas:

* Configuration
* Factory
* Manager
* Entity
* Value

#### Projetos do Diana

Diana é composta por quatro partes:

* **diana-core**: Camada de comunicação comum do JNoSQL API para todos os tipo.
* **diana-column**: Camada de comunicação do JNoSQL API para bancos de dados do tipo colunar.
* **diana-document**: Camada de comunicação do JNoSQL API para bancos de dados do tipo documento.
* **diana-key-value**: Camada de comunicação do JNoSQL API para bancos de dados do tipo chave-valor.
