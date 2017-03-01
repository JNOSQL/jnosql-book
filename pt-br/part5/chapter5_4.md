## As classes respositórios



Os “maestros” da persistência é dividida em forma síncrona e assíncrona com Repository e RepositoryAsync respectivamente. Esses recursos serão estendidos principalmente para adicionar recursos que existem especificamente em cada driver do diana, por exemplo, live query no OrientDB ou nível de consistência no Cassandra. Com o intuito de facilitar essa extensão existem as seguintes classes:



* AbstractKeyValueRepository

* AbstractColumnRepository

* AbstractColumnRepositoryAsync

* AbstractDocumentRepository

* AbstractDocumentRepositoryAsync



