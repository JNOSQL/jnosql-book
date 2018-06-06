# chapter5\_2

Componente responsável em enviar o evento, lembrando que ele não define a ordem apenas o envia. A sua implementação atual é enviar eventos de forma síncrona por CDI. Assim, é possível, por exemplo, substitui esse componente com o envio de eventos de forma assíncrona ou decorar esse componente para que ele envie outros tipos de eventos. Ele é dividido em especializações a partir dos tipos de bancos de dados.

* DocumentEventPersistManager
* ColumnEventPersistManager
* KeyValueEventPersistManager

