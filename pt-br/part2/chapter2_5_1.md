# chapter2\_5\_1

As classes fábricas são as responsáveis pela criação de uma classe Manager numa família de colunas.

* **ColumnFamilyManagerAsyncFactory**
* **ColumnFamilyManagerFactory**

As classes `ColumnFamilyManagerAsyncFactory` e `ColumnFamilyManagerFactory` são responsáveis pela criação das classes gerentes de forma síncrona e assíncrona respectivamente, para isso, basta passar informar o nome do banco de dados.

```java
ColumnFamilyManagerFactory factory = //instance
ColumnFamilyManagerAsyncFactory asyncFactory = //instance
ColumnFamilyManager manager = factory.get("database");
ColumnFamilyManagerAsync managerAsync = asyncFactory.getAsync("database");
```

As fábricas foram separadas itencionamente, uma vez, que nem todos os bancos de dados suportam operações síncronas ou assíncronas.

