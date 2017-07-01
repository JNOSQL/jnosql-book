## Document Template

This template has the duty to be a bridge between the entity model and Diana to document collection. It has two classes `DocumentTemplate` and `DocumentTemplateAsync`, one for the synchronous and the other for the asynchronous work.

#### `DocumentTemplate`

The `DocumentTemplate` is the document template for the synchronous tasks. It has three components:

* **DocumentEntityConverter**: That converts an entity to communication API, e.g., The Person to DocumentEntity.

* **DocumentCollectionManager**: The Diana document collection entity manager.

* **DocumentWorkflow**: The workflow to update and save methods.

```java
DocumentTemplate template = //instance

Person person = new Person();
person.setAddress("Olympus");
person.setName("Artemis Good");
person.setPhones(Arrays.asList("55 11 94320121", "55 11 94320121"));
person.setNickname("artemis");

List<Person> people = Collections.singletonList(person);

Person personUpdated = template.insert(person);
template.insert(people);
template.insert(person, Duration.ofHours(1L));

template.update(person);
template.update(people);
```

To do both remove and retrieve information from document collection that uses the same Diana classes, namely,  **DocumentQuery** and **DocumentDeleteQuery**.

```java
DocumentQuery query = DocumentQuery.of("Person");
query.and(DocumentCondition.eq(Document.of("address", "Olympus")));

List<Person> peopleWhoLiveOnOlympus = template.find(query);
Optional<Person> artemis = template.singleResult(DocumentQuery.of("Person")
                .and(DocumentCondition.eq(Document.of("nickname", "artemis"))));

DocumentDeleteQuery deleteQuery = query.toDeleteQuery();
template.delete(deleteQuery);
```

To use a document template just follow the CDI style and put an `@Inject` on the field.

```java
@Inject
private DocumentTemplate template;
```

The next step is to produce a **DocumentCollectionManager:**

```java
@Produces
public DocumentCollectionManager getManager() {
    DocumentCollectionManager manager = //instance
    return manager;
}
```

To work with more than one Document Template, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseA")
    private DocumentTemplate templateA;

    @Inject
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
    private DocumentTemplate templateB;


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

2\) Using the **DocumentTemplateProducer** class

```java
@Inject
private DocumentTemplateProducer producer;

public void sample() {
   DocumentCollectionManager managerA = //instance;
   DocumentCollectionManager managerB = //instance
   DocumentTemplate templateA = producer.get(managerA);
   DocumentTemplate templateB = producer.get(managerB);
}
```

#### `DocumentTemplateAsync`

The `DocumentTemplateAsync` is the document template for the asynchronous tasks. It has two components:

* **DocumentEntityConverter:** That converts an entity to communication API, e.g., The Person to DocumentEntity.

* **DocumentCollectionManagerAsync:** The Diana document collection entity manager asynchronous.

```java
DocumentTemplateAsync templateAsync = //instance

Person person = new Person();
person.setAddress("Olympus");
person.setName("Artemis Good");
person.setPhones(Arrays.asList("55 11 94320121", "55 11 94320121"));
person.setNickname("artemis");

List<Person> people = Collections.singletonList(person);

Consumer<Person> callback = p -> {};
templateAsync.insert(person);
templateAsync.insert(person, Duration.ofHours(1L));
templateAsync.insert(person, callback);
templateAsync.insert(people);

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

To use a document template just follow the CDI style and put an `@Inject` on the field.

```java
@Inject
private
DocumentTemplateAsync template;
```

The next step is produced a **DocumentCollectionManagerAsync:**

```
@Produces
public DocumentCollectionManagerAsync getManager() {
    DocumentCollectionManagerAsync managerAsync = //instance
    return manager;
}
```

To work with more than one Document Template, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseA")
    private DocumentTemplateAsync templateA;

    @Inject
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
    private DocumentTemplateAsync templateB;


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

2\) Using the **DocumentTemplateAsyncProducer**

```java
@Inject
private DocumentTemplateAsyncProducer producer;

public void sample() {
   DocumentCollectionManagerAsync managerA = //instance;
   DocumentCollectionManagerAsync managerB = //instance
   DocumentTemplateAsync templateA = producer.get(managerA);
   DocumentTemplateAsync templateB = producer.get(managerB);
}
```
