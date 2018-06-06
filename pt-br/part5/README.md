# Componentes do Artemis

Um ponto importante dentro do Artemis é que ele é configurável focando principalmente na variedade que existe no mundo dos bancos de dados. Todos os seus componentes são facilmente trocados, para isso, basta implementar um recurso e utilizar o Alternative, recurso oriundo do próprio CDI, além de pode ser adicionado ou decorado sem que haja conflito com o recurso já existente. Os componentes são:

* **Workflow**: Define a ordem quando uma entidade será salva ou atualizada.
* **EventManager**: Componente responsável em enviar o evento, lembrando que ele não define a ordem apenas o envia. A sua implementação atual é enviar eventos de forma síncrona por CDI. Assim, é possível, por exemplo, substitui esse componente com o envio de eventos de forma assíncrona ou decorar esse componente para que ele envie outros tipos de eventos.
* **Converter**: Responsável por converter uma entidade, Person, para as classes do domínio do Diana.
* **Repository**: O “maestro” da persistência síncrona, responsável por enviar uma entidade para o banco de dados.
* **RepositoryAsync**: O “maestro” da persistência assíncrona, responsável por enviar uma entidade para o banco de dados.

