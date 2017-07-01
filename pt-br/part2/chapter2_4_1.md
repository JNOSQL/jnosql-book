#### Document Manager

A interação com o banco de dados do tipo documento é dado por duas classes:

* **DocumentCollectionManager**: Para realizar operações no banco de dados de forma síncrona
* **DocumentCollectionManagerAsync**: Para realizar operações no banco de dados de forma assíncrona.

##### **DocumentCollectionManager**

O `DocumentCollectionManager` é classe que realiza as operações de forma síncrona, com ele é possível realizar a criação, editação, remoção e a recuperação dentro dos bancos de dados do tipo documento.

```java
        DocumentEntity entity = DocumentEntity.of("collection");
        Document diana = Document.of("name", "Diana");
        entity.add(diana);

        List<DocumentEntity> entities = Collections.singletonList(entity);
        DocumentCollectionManager manager = //instance;

        //insert operations
        manager.insert(entity);
        manager.insert(entity, Duration.ofHours(2L));//inserts with 2 hours of TTL
        manager.insert(entities, Duration.ofHours(2L));//inserts with 2 hours of TTL
        //updates operations
        manager.update(entity);
        manager.update(entities);
```

##### **DocumentCollectionManagerAsync**

O `DocumentCollectionManagerAsync` é classe que realiza as operações de forma assíncrona, com ele é possível realizar a criação, editação, remoção e a recuperação dentro dos bancos de dados do tipo documento.

```java
        DocumentEntity entity = DocumentEntity.of("collection");
        Document diana = Document.of("name", "Diana");
        entity.add(diana);

        List<DocumentEntity> entities = Collections.singletonList(entity);
         DocumentCollectionManagerAsync managerAsync = //instance

        //insert operations
        managerAsync.insert(entity);
        managerAsync.insert(entity, Duration.ofHours(2L));//inserts with 2 hours of TTL
        managerAsync.insert(entities, Duration.ofHours(2L));//inserts with 2 hours of TTL
        //updates operations
        managerAsync.update(entity);
        managerAsync.update(entities);
```

Em alguns momentos é necessário saber quando tal operação foi finalizada, mesmo quando é utilizado de forma assíncrona. Com esse objetivo, essa classe também vem com suporte a `callBack`, assim, tão logo a operação seja finalizada.

```java
        Consumer<DocumentEntity> callBack = e -> {};
        managerAsync.insert(entity, callBack);
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
query.withMaxResults(10);
query.withFirstResult(2);
List<DocumentEntity> entities = manager.select(query);
Optional<DocumentEntity> entity = manager.singleResult(query);
Consumer<List<DocumentEntity>> callback = e -> {};
managerAsync.select(query, callback);
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



