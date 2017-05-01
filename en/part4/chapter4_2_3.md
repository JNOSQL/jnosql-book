## Key-Value template

The key value is a bridge between the entity and the key-value database.

#### `KeyValueTemplate`

The `KeyValuTemplate` is the column template to synchronous tasks. It has three components:

The KeyValuTemplate is responsible for persistency of an entity in a key-value database. It is composed basically for three components.

* **KeyValueEntityConverter**: That converts an entity to communication API, e.g., The Person to KeyValueEntity.

* **BucketManager**: The Diana column key-value entity manager.

* **KeyValueWorkflow**: The workflow to update and save methods.

```java
KeyValuTemplate template = null;
User user = new User();
user.setNickname("ada");
user.setAge(10);
user.setName("Ada Lovelace");
List<User> users = Collections.singletonList(user);

template.put(user);
template.put(users);

Optional<Person> ada = template.get("ada", Person.class);
Iterable<Person> usersFound = template.get(Collections.singletonList("ada"), Person.class);
```
To use a key-value template just follows the CDI style and put an `@Inject` on the field.


```java
@Inject
private KeyValuTemplate template;
```

The next step is to produce a **BucketManager**:

```java
@Produces
public BucketManager getManager() {
    BucketManager manager = //instance
    return manager;
}
```

To work with more than one key-value Template, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.KEY_VALUE, provider = "databaseA")
    private KeyValueTemplate templateA;

    @Inject
    @Database(value = DatabaseType.KEY_VALUE, provider = "databaseB")
    private KeyValueTemplate templateB;


    //producers methods
    @Produces
    @Database(value = DatabaseType.KEY_VALUE, provider = "databaseA")
    public BucketManager getManagerA() {
        DocumentCollectionManager manager =//instance
        return manager;
    }

    @Produces
    @Database(value = DatabaseType.KEY_VALUE, provider = "databaseB")
    public DocumentCollectionManager getManagerB() {
        BucketManager manager = //instance
        return manager;
    }
```

2\)  Using the **KeyValueTemplateProducer** class

```java
@Inject
private KeyValueTemplateProducer producer;

public void sample() {
   BucketManager managerA = //instance;
   BucketManager managerB = //instance
   KeyValueTemplate templateA = producer.get(managerA);
   KeyValueTemplate templateB = producer.get(managerB);
}
```
