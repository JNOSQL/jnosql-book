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
        Column diana = Column.of("name", "Diana");
        entity.add(diana);

        List<ColumnEntity> entities = Collections.singletonList(entity);

        ColumnFamilyManagerAsync managerAsync = null;


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
        Consumer<ColumnEntity> callBack = e -> {};
        managerAsync.save(entity, callBack);
        managerAsync.update(entity, callBack);
```

##### Buscando as informações dentro de uma família de coluna:

##### 

No diana, as buscas tanto de forma síncrona e assíncrona são realizadas a partir da classe Column`Query`, com essa classe é possível definir se alguns ou todos os apenas algumas colunas serão retornados, ordenação além da condição para a informação a ser recuperada.

A condição dentro da query é formada por `ColumnCondition`, ele á composta por uma condição e um documento, por exemplo, o a condição abaixo buscará informação em que nome seja igual a “**Ada**”.

```java
ColumnCondition nameEqualsAda = ColumnCondition.eq(Column.of("name", “Ada”));
```

Também possível agrupar as informações da condição com operadores **AND**, **OR** e **NOT**.

```java
ColumnCondition nameEqualsAda = ColumnCondition.eq(Column.of("name", "Ada"));
ColumnCondition youngerThan2Years = ColumnCondition.lt(Column.of("age", 2));
ColumnCondition condition = nameEqualsAda.and(youngerThan2Years);
ColumnCondition nameNotEqualsAda = nameEqualsAda.negate();
```

Caso não seja informado uma condição significa que ele tentará trazer todas as informações no banco de dados, semelhante ao “`select * from database`” em um banco relacional, vale salientar que nem todos os bancos possuem suporte a tal recurso.

Dentro do ColumnQuery também é possível paginar as informações utilizando onde deve começar a busca e o limit máximo de retorno.

```java
        ColumnFamilyManager manager = //instance;

        ColumnFamilyManagerAsync managerAsync = //instance;

        ColumnQuery query = ColumnQuery.of("collection");
        ColumnCondition ageBiggerTen = ColumnCondition.gt(Column.of("age", 10));
        query.and(ageBiggerTen);
        query.addSort(Sort.of("name", Sort.SortType.ASC));

        query.withMaxResults(10);
        query.withFirstResult(2);

        List<ColumnEntity> entities = manager.find(query);
        Optional<ColumnEntity> entity = manager.singleResult(query);

        Consumer<List<ColumnEntity>> callback = e -> {};
        managerAsync.find(query, callback);
```

##### Removendo as informações dentro de uma família de colunas:

Semelhante ao `ColumnQuery,`existe uma classe responsável por remover informações dentro da coleção de documentos: A classe `ColumnDeleteQuery`

Ela possui uma estrutura bem simples, sem paginação e ordenação, uma vez que o fogo será a remoção de informação dentro do banco de dados.

```java
        ColumnFamilyManager manager = //instance;
        ColumnFamilyManagerAsync managerAsync = //instance;

        ColumnDeleteQuery query = ColumnDeleteQuery.of("collection");
        ColumnCondition ageBiggerTen = ColumnCondition.gt(Column.of("age", 10));
        query.and(ageBiggerTen);


        manager.delete(query);

        managerAsync.delete(query);
        managerAsync.delete(query, v -> {});
```



