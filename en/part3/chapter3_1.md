## Utilizando drivers para com o Diana

Um importante ponto sobre o Diana é que ele é apenas uma API subdivida em quatro, uma para cada banco de dados NoSQL. Ou seja, sem uma implementação essa API não é útil, Uma vez o um banco de dados implemente uma dessas APIs, e aprovado no TCK, é dito que o Diana possui o driver daquele banco de dados para um determinado tipo, por exemplo, o Cassandra possui o driver para colunas.

Caso o banco de dados seja multi-model, ou seja, suporte mais de um banco de dados, é necessário que ele implemente e rode os testes do TCK para cada banco que ele suporte, por exemplo, um provedor suporta chave-valor e também documentos, assim, é necessário que ele implemente a API de chave-valor e de documentos.

Dentro do diana-driver tem alguns drivers prontos para uso:

[https://github.com/JNOSQL/diana-driver](https://github.com/JNOSQL/diana-driver)



Dentro do diana-demo se tem alguns exemplos do seu uso:

[https://github.com/JNOSQL/diana-demos](https://github.com/JNOSQL/diana-demos)

