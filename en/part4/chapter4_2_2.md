## Column Family Template

This template has the duty to be a bridge between the entity model and Diana to a column family. It has two classes `ColumnTemplate` and `ColumnTemplateAsync`, one for the synchronous and the other for the asynchronous work.

#### ColumnTemplate

The `ColumnTemplate` is the column template for the synchronous tasks. It has three components:

* **ColumnEntityConverter**: That converts an entity to communication API, e.g., The Person to ColumnFamilyEntity.

* **ColumnCollectionManager**: The Diana column famiy entity manager.

* **ColumnWorkflow**: The workflow to update and save methods.

```java
ColumnTemplate template = //instance

Person person = new Person();
person.setAddress("Olympus");
person.setName("Artemis Good");
person.setPhones(Arrays.asList("55 11 94320121", "55 11 94320121"));
person.setNickname("artemis");

List<Person> people = Collections.singletonList(person);

Person personUpdated = template.save(person);
template.save(people);
template.save(person, Duration.ofHours(1L));

template.update(person);
template.update(people);
```

For information removal and retrieval are used the same classes from Diana for documents,  **DocumentQuery** and **DocumentDeleteQuery**, respectively, also the callback method can be used.


```java
ColumnQuery query = DocumentQuery.of("Person");
query.and(ColumnCondition.eq(Column.of("address", "Olympus")));

List<Person> peopleWhoLiveOnOlympus = template.find(query);
Optional<Person> artemis = template.singleResult(ColumnQuery.of("Person")
                .and(ColumnCondition.eq(Column.of("nickname", "artemis"))));

ColumnDeleteQuery deleteQuery = query.toDeleteQuery();
template.delete(deleteQuery);
```

To use a column template just follow the CDI style and put an `@Inject` on the field.

```java
@Inject
private ColumnTemplate template;
```

The next step is produced a **ColumnFamilyManager**:

```java
@Produces
public ColumnFamilyManager getManager() {
    ColumnFamilyManager manager = //instance
    return manager;
}
```

To work with more than one Column Template, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.COLUMN, provider = "databaseA")
    private ColumnTemplate templateA;

    @Inject
    @Database(value = DatabaseType.COLUMN, provider = "databaseB")
    private ColumnTemplate templateB;


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

2\)  Using the **ColumnTemplateProducer** class

```java
@Inject
private ColumnTemplateProducer producer;

public void sample() {
   ColumnFamilyManager managerA = //instance;
   ColumnFamilyManager managerB = //instance
   ColumnTemplate templateA = producer.get(managerA);
   ColumnTemplate templateB = producer.get(managerB);
}
```

#### ColumnTemplateAsync


The `ColumnTemplateAsync` is the document template for the asynchronous tasks. It has two components:

* **ColumnEntityConverter:** That converts an entity to communication API, e.g., The Person to ColumnFamilyEntity.

* **ColumnFamilyManagerAsync:** The Diana column family entity manager asynchronous.


```java
ColumnTemplateAsync templateAsync = //instance

Person person = new Person();
person.setAddress("Olympus");
person.setName("Artemis Good");
person.setPhones(Arrays.asList("55 11 94320121", "55 11 94320121"));
person.setNickname("artemis");

List<Person> people = Collections.singletonList(person);

Consumer<Person> callback = p -> {};
templateAsync.save(person);
templateAsync.save(person, Duration.ofHours(1L));
templateAsync.save(person, callback);
templateAsync.save(people);

templateAsync.update(person);
templateAsync.update(person, callback);
templateAsync.update(people);
```

For information removal and retrieval are used the same classes from Diana for documents,  **DocumentQuery** and **DocumentDeleteQuery**, respectively, also the callback method can be used.


```java
Consumer<List<Person>> callBackPeople = p -> {};
Consumer<Void> voidCallBack = v ->{};
templateAsync.find(query, callBackPeople);
templateAsync.delete(deleteQuery);
templateAsync.delete(deleteQuery, voidCallBack);
```

To use a column template just follow the CDI style and put an `@Inject` on the field.

```java
@Inject
private ColumnTemplateAsync template;
```


The next step is to produce a **ColumnFamilyManagerAsync:**

```
@Produces
public ColumnFamilyManagerAsync getManager() {
    ColumnFamilyManagerAsync managerAsync = //instance
    return manager;
}
```

To work with more than one Column Template, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.COLUMN, provider = "databaseA")
    private ColumnTemplateAsync templateA;

    @Inject
    @Database(value = DatabaseType.COLUMN, provider = "databaseB")
    private ColumnTemplateAsync templateB;


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

2\) Using the  **ColumnTemplateAsyncProducer**

```java
@Inject
private ColumnTemplateAsyncProducer producer;

public void sample() {
   ColumnFamilyManagerAsync managerA = //instance;
   ColumnFamilyManagerAsync managerB = //instance
   ColumnTemplateAsync templateA = producer.get(managerA);
   ColumnTemplateAsync templateB = producer.get(managerB);
}
```
