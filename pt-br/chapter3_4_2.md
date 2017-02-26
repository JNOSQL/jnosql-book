#### Column Manager

A interação com o banco de dados do tipo família de coluna é dado por duas classes:

* **ColumnFamilyManager**: Para realizar operações no banco de dados de forma síncrona
* **ColumnFamilyManagerAsync**: Para realizar operações no banco de dados de forma assíncrona.

##### **ColumnFamilyManager**

O `ColumnFamilyManager` é classe que realiza as operações de forma síncrona, com ele é possível realizar a criação, editação, remoção e a recuperação dentro dos bancos de dados do tipo família de coluna.

```java
       ColumnEntity entity = ColumnEntity.of("columnFamily");
        Column diana = Column.of("name", "Diana");
        entity.add(diana);

        List<ColumnEntity> entities = Collections.singletonList(entity);
        ColumnFamilyManager manager = //instance;


        //saves operations
        manager.save(entity);
        manager.save(entity, Duration.ofHours(2L));//saves with 2 hours of TTL
        manager.save(entities, Duration.ofHours(2L));//saves with 2 hours of TTL
        //updates operations
        manager.update(entity);
        manager.update(entities);
```

##### ColumnFamilyManagerAsync

O `ColumnFamilyManagerAsync` é classe que realiza as operações de forma assíncrona, com ele é possível realizar a criação, editação, remoção e a recuperação dentro dos bancos de dados do tipo família de coluna.

```java
        DocumentEntity entity = DocumentEntity.of("collection");
        Document diana = Document.of("name", "Diana");
        entity.add(diana);

        List<DocumentEntity> entities = Collections.singletonList(entity);
         DocumentCollectionManagerAsync managerAsync = null;

        //saves operations
        managerAsync.save(entity);
        managerAsync.save(entity, Duration.ofHours(2L));//saves with 2 hours of TTL
        managerAsync.save(entities, Duration.ofHours(2L));//saves with 2 hours of TTL
        //updates operations
        managerAsync.update(entity);
        managerAsync.update(entities);
```

Em alguns momentos é necessário saber quando tal operação foi finalizada, mesmo quando é utilizado de forma assíncrona. Com esse objetivo, essa classe também vem com suporte a `callBack`, assim, tão logo a operação seja finalizada.

```java
        Consumer<DocumentEntity> callBack = e -> {};
        managerAsync.save(entity, callBack);
        managerAsync.update(entity, callBack);
```

##### Buscando as informações dentro de uma coleção de documentos:

##### 

No diana, as buscas tanto de forma síncrona e assíncrona são realizadas a partir da classe `DocumentQuery`, com essa classe é possível definir se alguns ou todos os apenas alguns documentos serão retornados, ordenação além da condição para a informação ser recuperada.

A condição dentro da query é formada por `DocumentCondition`, ele á composta por uma condição e um documento, por exemplo, o a condição abaixo buscará informação em que nome seja igual a “**Ada**”.

```java
DocumentCondition nameEqualsAda = DocumentCondition.eq(Document.of("name", “Ada”));
```

Também possível agrupar as informações da condição com operadores **AND**, **OR** e **NOT**.

```java
DocumentCondition nameEqualsAda = DocumentCondition.eq(Document.of("name", "Ada"));
DocumentCondition youngerThan2Years = DocumentCondition.lt(Document.of("age", 2));
DocumentCondition condition = nameEqualsAda.and(youngerThan2Years);
DocumentCondition nameNotEqualsAda = nameEqualsAda.negate();
```

Caso não seja informado uma condição significa que ele tentará trazer todas as informações no banco de dados, semelhante ao “`select * from database`” em um banco relacional, vale salientar que nem todos os bancos possuem suporte a tal recurso.

Dentro do DocumentQuery também é possível paginar as informações utilizando onde deve começar a busca e o limit máximo de retorno.

```java
DocumentCollectionManager manager = //instance;
DocumentCollectionManagerAsync managerAsync = //instance;
DocumentQuery query = DocumentQuery.of("collection");
DocumentCondition ageBiggerTen = DocumentCondition.gt(Document.of("age", 10));
query.and(ageBiggerTen);
query.addSort(Sort.of("name", Sort.SortType.ASC));
query.setLimit(10);
query.setStart(2);
List<DocumentEntity> entities = manager.find(query);
Optional<DocumentEntity> entity = manager.singleResult(query);
Consumer<List<DocumentEntity>> callback = e -> {};
managerAsync.find(query, callback);
```

##### Removendo as informações dentro de uma coleção de documentos:

Semelhante ao `DocumentQuery,`existe uma classe responsável por remover informações dentro da coleção de documentos: A classe `DocumentDeleteQuery`

Ela possui uma estrutura bem simples, sem paginação e ordenação, uma vez que o fogo será a remoção de informação dentro do banco de dados.

```java
        DocumentCollectionManager manager = //instance;
        DocumentCollectionManagerAsync managerAsync = //instance;

        DocumentDeleteQuery query = DocumentDeleteQuery.of("collection");
        DocumentCondition ageBiggerTen = DocumentCondition.gt(Document.of("age", 10));
        query.and(ageBiggerTen);


        manager.delete(query);

        managerAsync.delete(query);
        managerAsync.delete(query, v -> {});
```



