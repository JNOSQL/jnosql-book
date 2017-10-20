#### Column Manager

The Manager class for the column family type can be synchronous or asynchronous:

* **ColumnFamilyManager**: To do synchronous operations.
* **ColumnFamilyManagerAsync**: To do asynchronous operations.

##### **ColumnFamilyManager**

The `ColumnFamilyManager` is the class that manages the persistence on the synchronous way to column family.

```java
       ColumnEntity entity = ColumnEntity.of("columnFamily");
        Column diana = Column.of("name", "Diana");
        entity.add(diana);

        List<ColumnEntity> entities = Collections.singletonList(entity);
        ColumnFamilyManager manager = //instance;


        //inserts operations
        manager.insert(entity);
        manager.insert(entity, Duration.ofHours(2L));//inserts with 2 hours of TTL
        manager.insert(entities, Duration.ofHours(2L));//inserts with 2 hours of TTL
        //updates operations
        manager.update(entity);
        manager.update(entities);
```

##### ColumnFamilyManagerAsync

The `ColumnFamilyManagerAsync` is the class that manages the persistence on the asynchronous way to column family.

```java
        Column diana = Column.of("name", "Diana");
        entity.add(diana);

        List<ColumnEntity> entities = Collections.singletonList(entity);

        ColumnFamilyManagerAsync managerAsync = null;


        //inserts operations
        managerAsync.insert(entity);
        managerAsync.insert(entity, Duration.ofHours(2L));//inserts with 2 hours of TTL
        managerAsync.insert(entities, Duration.ofHours(2L));//inserts with 2 hours of TTL
        //updates operations
        managerAsync.update(entity);
        managerAsync.update(entities);
```

Sometimes on an asynchronous process, is important to know when this process is over, so the `ColumnFamilyManagerAsync` also has callback support.

```java
        Consumer<ColumnEntity> callBack = e -> {};
        managerAsync.insert(entity, callBack);
        managerAsync.update(entity, callBack);
```

##### Search information on a column family

#### 

Diana has support to retrieve information from both ways synchronous and asynchronous from the `ColumnQuery` class. The `ColumnQuery`  has information such as sort type, document and also the condition to retrieve information.

The condition on `ColumnQuery` is given from `ColumnCondition`, whose it has the status and the column. Eg. The condition behind is to find a name equal "**Ada**".

```java
ColumnCondition nameEqualsAda = ColumnCondition.eq(Column.of("name", “Ada”));
```

Also, the developer can use the aggregators such as **AND**, **OR** e **NOT**.

```java
ColumnCondition nameEqualsAda = ColumnCondition.eq(Column.of("name", "Ada"));
ColumnCondition youngerThan2Years = ColumnCondition.lt(Column.of("age", 2));
ColumnCondition condition = nameEqualsAda.and(youngerThan2Years);
ColumnCondition nameNotEqualsAda = nameEqualsAda.negate();
```

If there isn't condition at the query that means the query will try to retrieve all information from the database, look like a “`select * from database`” in a relational database, just to remember the return depends on from driver. It is important to say that not all NoSQL databases have support for this resource.

ColumnQuery also has pagination feature to define where the data start, and it limits.

```java
        ColumnFamilyManager manager = //instance;

        ColumnFamilyManagerAsync managerAsync = //instance;

        ColumnQuery query = ColumnQuery.of("collection");
        ColumnCondition ageBiggerTen = ColumnCondition.gt(Column.of("age", 10));
        query.and(ageBiggerTen);
        query.addSort(Sort.of("name", Sort.SortType.ASC));

        query.withMaxResults(10);
        query.withFirstResult(2);

        List<ColumnEntity> entities = manager.select(query);
        Optional<ColumnEntity> entity = manager.singleResult(query);

        Consumer<List<ColumnEntity>> callback = e -> {};
        managerAsync.select(query, callback);
```

##### Removing information from Column Family

Such as `ColumnQuery` there is a class to remove information from the column database type: A `ColumnDeleteQuery` type.

It is smoother than `ColumnQuery` because there isn't pagination and sort feature, once this information is unnecessary to remove information from database.

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



