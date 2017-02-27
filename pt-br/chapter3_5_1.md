#### Column Family Manager Factory





As classes fábricas são as responsáveis pela criação de uma classe Manager numa coleção de documentos. 



* **ColumnFamilyManagerAsyncFactory**
* **ColumnFamilyManagerFactory**
* **UnaryColumnConfiguration**



As classes `ColumnFamilyManagerAsyncFactory` e `ColumnFamilyManagerFactory` são responsáveis pela criação das classes gerentes de forma síncrona e assíncrona respectivamente, para isso, basta passar informar o nome do banco de dados.



```java
ColumnFamilyManagerFactory factory = //instance
ColumnFamilyManagerAsyncFactory asyncFactory = //instance
ColumnFamilyManager manager = factory.get("database");
ColumnFamilyManagerAsync managerAsync = asyncFactory.getAsync("database");

```

s fábricas foram separadas itencionamente, uma vez, que nem todos os bancos de dados suportam operações síncronas ou assíncronas. Caso ele suporte as duas operações existe a classe `UnaryColumnConfiguration`, no qual o provedor de banco de dados pode utilizar. Essa classe implementa as duas fábricas.



```java
UnaryColumnConfiguration unary = //instance
ColumnFamilyManager manager = unary.get("database");
ColumnFamilyManagerAsync managerAsync = unary.getAsync("database");
```



