## Document Repository

This repository has the duty to be a bridge between the entity model and Diana to document collection. It has two classes `DocumentRepository` and `DocumentRepositoryAsync`, one for the synchronous and the other for the asynchronous work.

#### `DocumentRepository`

The `DocumentRepository` is the document repository for the synchronous tasks. It has three components:

* **DocumentEntityConverter**: That converts an entity to communication API, e.g., The Person to DocumentEntity.

* **DocumentCollectionManager**: The Diana document collection entity manager.

* **DocumentWorkflow**: The workflow to update and save methods.

```java
DocumentRepository repository = //instance

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

To do both remove and retrieve information from document collection that uses the same Diana classes, namely,  **DocumentQuery** and **DocumentDeleteQuery**.

```java
DocumentQuery query = DocumentQuery.of("Person");
query.and(DocumentCondition.eq(Document.of("address", "Olympus")));

List<Person> peopleWhoLiveOnOlympus = repository.find(query);
Optional<Person> artemis = repository.singleResult(DocumentQuery.of("Person")
                .and(DocumentCondition.eq(Document.of("nickname", "artemis"))));

DocumentDeleteQuery deleteQuery = query.toDeleteQuery();
repository.delete(deleteQuery);
```

To use a document repository just follow the CDI style and put an `@Inject` on the field.

```java
@Inject
private DocumentRepository repository;
```

The next step is to produce a **DocumentCollectionManager:**

```java
@Produces
public DocumentCollectionManager getManager() {
    DocumentCollectionManager manager = //instance
    return manager;
}
```

To work with more than one Document Repository, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseA")
    private DocumentRepository repositorA;

    @Inject
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
    private DocumentRepository repositoryB;


    //producers methods
    @Produces
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseA")
    public DocumentCollectionManager getManagerA() {
        DocumentCollectionManager manager = //instance
        return manager;
    }

    @Produces
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
    public DocumentCollectionManager getManagerB() {
        DocumentCollectionManager manager = //instance
        return manager;
    }
```

2\) Using the **DocumentRepositoryProducer** class

```java
@Inject
private DocumentRepositoryProducer producer;

public void sample() {
   DocumentCollectionManager managerA = //instance;
   DocumentCollectionManager managerB = //instance
   DocumentRepository repositorA = producer.get(managerA);
   DocumentRepository repositoryB = producer.get(managerB);
}
```

#### `DocumentRepositoryAsync`

The `DocumentRepositoryAsync` is the document repository for the asynchronous tasks. It has two components:

* **DocumentEntityConverter:** That converts an entity to communication API, e.g., The Person to DocumentEntity.

* **DocumentCollectionManagerAsync:** The Diana document collection entity manager asynchronous.

```java
DocumentRepositoryAsync repositoryAsync = //instance

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

To use a document repository just follow the CDI style and put an `@Inject` on the field.

```java
@Inject
private
DocumentRepositoryAsync repository;
```

The next step is produced a **DocumentCollectionManagerAsync:**

```
@Produces
public DocumentCollectionManagerAsync getManager() {
    DocumentCollectionManagerAsync managerAsync = //instance
    return manager;
}
```

To work with more than one Document Repository, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseA")
    private DocumentRepositoryAsync repositorA;

    @Inject
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
    private DocumentRepositoryAsync repositoryB;


    //producers methods
    @Produces
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseA")
    public DocumentCollectionManagerAsync getManagerA() {
        DocumentCollectionManager manager = //instance
        return manager;
    }

    @Produces
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
    public DocumentCollectionManagerAsync getManagerB() {
        DocumentCollectionManager manager = //instance
        return manager;
    }
```

2\) Using the **DocumentRepositoryAsyncProducer**

```java
@Inject
private DocumentRepositoryAsyncProducer producer;

public void sample() {
   DocumentCollectionManagerAsync managerA = //instance;
   DocumentCollectionManagerAsync managerB = //instance
   DocumentRepositoryAsync repositorA = producer.get(managerA);
   DocumentRepositoryAsync repositoryB = producer.get(managerB);
}
```

####
