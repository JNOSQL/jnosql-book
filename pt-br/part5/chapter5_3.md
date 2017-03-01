## Converter

Responsável por converter uma entidade, Person, para as classes do domínio do Diana e vise e versa. Ela é tem especializações a partir do tipo de banco de dados:

* ColumnEntityConverter

* DocumentEntityConverter

* KeyValueEntityConverter



Esse recurso é muito importante, por exemplo, como extensão para um banco de dados específico. É possível imaginar a criação de uma anotação \(índice, UDT para o Cassandra, para indicar que um determinado campo será criptografado, etc.\) e levá-lo em consideração durante a conversão e a desconversão.



