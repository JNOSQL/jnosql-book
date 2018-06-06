# Eclipse JNoSQL one API to many NoSQL databases

O JNoSQL possui várias ferramentas para facilitar a integração entre as aplicações Java e os bancos de dados NoSQL. Para resolver este problema, o projeto tem duas camadas:

* **API de Comunicação:** Uma API para comunicar apenas com o banco de dados, da mesma forma que o JDBC faz com o SQL. Esta API possui quatro tipos, um para cada tipo de banco de dados.
* **API de Mapeamento:** Uma API para realizar a melhor integração com o desenvolvedor Java, a qual é orientado a anotações e realiza integrações com outras tecnologias, como o Bean Validation, etc., além de utilizar CDI para resolver esta camada.



