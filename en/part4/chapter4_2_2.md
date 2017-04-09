## Column Family Repository

This repository has the duty to be a bridge between the entity model and Diana to a column family. It has two classes `ColumnRepository` and `ColumnRepositoryAsync`, one for the synchronous and the other for the asynchronous work.

#### ColumnRepository

The `ColumnRepository` is the column repository for the synchronous tasks. It has three components:

* **ColumnEntityConverter**: That converts an entity to communication API, e.g., The Person to ColumnFamilyEntity.

* **ColumnCollectionManager**: The Diana column famiy entity manager.

* **ColumnWorkflow**: The workflow to update and save methods.

```java
ColumnRepository repository = //instance

Person person = new Person();
person.setAddress("Olympus");
person.setName("Artemis Good");
person.setPhones(Arrays.asList("55 11 94320121", "55 11 94320121"));
person.setNickname("artemis");

List<Person> people = Collections.singletonList(person);

Person personUpdated = repository.save(person);
repository.save(people);
repository.save(person, Duration.ofHours(1L));

repository.update(person);
repository.update(people);
```

For information removal and retrieval are used the same classes from Diana for documents,  **DocumentQuery** and **DocumentDeleteQuery**, respectively, also the callback method can be used.


```java
ColumnQuery query = DocumentQuery.of("Person");
query.and(ColumnCondition.eq(Column.of("address", "Olympus")));

List<Person> peopleWhoLiveOnOlympus = repository.find(query);
Optional<Person> artemis = repository.singleResult(ColumnQuery.of("Person")
                .and(ColumnCondition.eq(Column.of("nickname", "artemis"))));

ColumnDeleteQuery deleteQuery = query.toDeleteQuery();
repository.delete(deleteQuery);
```

To use a column repository just follow the CDI style and put an `@Inject` on the field.

```java
@Inject
private ColumnRepository repository;
```

The next step is produced a **ColumnFamilyManager**:

```java
@Produces
public ColumnFamilyManager getManager() {
    ColumnFamilyManager manager = //instance
    return manager;
}
```

To work with more than one Column Repository, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.COLUMN, provider = "databaseA")
    private ColumnRepository repositorA;

    @Inject
    @Database(value = DatabaseType.COLUMN, provider = "databaseB")
    private ColumnRepository repositoryB;


    //producers methods
    @Produces
    @Database(value = DatabaseType.COLUMN, provider = "databaseA")
    public ColumnFamilyManager getManagerA() {
        ColumnFamilyManager manager =//instance
        return manager;
    }

    @Produces
    @Database(value = DatabaseType.COLUMN, provider = "databaseB")
    public ColumnFamilyManager getManagerB() {
        ColumnFamilyManager manager = //instance
        return manager;
    }
```

2\)  Using the **ColumnRepositoryProducer** class

```java
@Inject
private ColumnRepositoryProducer producer;

public void sample() {
   ColumnFamilyManager managerA = //instance;
   ColumnFamilyManager managerB = //instance
   ColumnRepository repositorA = producer.get(managerA);
   ColumnRepository repositoryB = producer.get(managerB);
}
```

#### ColumnRepositoryAsync


The `ColumnRepositoryAsync` is the document repository for the asynchronous tasks. It has two components:

* **ColumnEntityConverter:** That converts an entity to communication API, e.g., The Person to ColumnFamilyEntity.

* **ColumnFamilyManagerAsync:** The Diana column family entity manager asynchronous.


```java
ColumnRepositoryAsync repositoryAsync = //instance

Person person = new Person();
person.setAddress("Olympus");
person.setName("Artemis Good");
person.setPhones(Arrays.asList("55 11 94320121", "55 11 94320121"));
person.setNickname("artemis");

List<Person> people = Collections.singletonList(person);

Consumer<Person> callback = p -> {};
repositoryAsync.save(person);
repositoryAsync.save(person, Duration.ofHours(1L));
repositoryAsync.save(person, callback);
repositoryAsync.save(people);

repositoryAsync.update(person);
repositoryAsync.update(person, callback);
repositoryAsync.update(people);
```

For information removal and retrieval are used the same classes from Diana for documents,  **DocumentQuery** and **DocumentDeleteQuery**, respectively, also the callback method can be used.


```java
Consumer<List<Person>> callBackPeople = p -> {};
Consumer<Void> voidCallBack = v ->{};
repositoryAsync.find(query, callBackPeople);
repositoryAsync.delete(deleteQuery);
repositoryAsync.delete(deleteQuery, voidCallBack);
```

To use a column repository just follow the CDI style and put an `@Inject` on the field.

```java
@Inject
private ColumnRepositoryAsync repository;
```


The next step is to produce a **ColumnFamilyManagerAsync:**

```
@Produces
public ColumnFamilyManagerAsync getManager() {
    ColumnFamilyManagerAsync managerAsync = //instance
    return manager;
}
```

To work with more than one Column Repository, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.COLUMN, provider = "databaseA")
    private ColumnRepositoryAsync repositorA;

    @Inject
    @Database(value = DatabaseType.COLUMN, provider = "databaseB")
    private ColumnRepositoryAsync repositoryB;


    //producers methods
    @Produces
    @Database(value = DatabaseType.COLUMN, provider = "databaseA")
    public ColumnFamilyManagerAsync getManagerA() {
        ColumnFamilyManagerAsync manager = //instance
        return manager;
    }

    @Produces
    @Database(value = DatabaseType.COLUMN, provider = "databaseB")
    public ColumnFamilyManagerAsync getManagerB() {
        ColumnFamilyManagerAsync manager = //instance
        return manager;
    }
```

2\) Using the  **ColumnRepositoryAsyncProducer**

```java
@Inject
private DocumentRepositoryAsyncProducer producer;

public void sample() {
   ColumnFamilyManagerAsync managerA = //instance;
   ColumnFamilyManagerAsync managerB = //instance
   ColumnRepositoryAsync repositorA = producer.get(managerA);
   ColumnRepositoryAsync repositoryB = producer.get(managerB);
}
```

####
