## Key-Value repository

The key value is a bridge between the entity and the key-value database.

#### `KeyValueRepository`

The `KeyValuRepository` is the column repository to synchronous tasks. It has three components:

The KeyValuRepository is responsible for persistency of an entity in a key-value database. It is composed basically for three components.

* **KeyValueEntityConverter**: That converts an entity to communication API, e.g., The Person to KeyValueEntity.

* **BucketManager**: The Diana column key-value entity manager.

* **KeyValueWorkflow**: The workflow to update and save methods.

```java
KeyValueRepository repository = null;
User user = new User();
user.setNickname("ada");
user.setAge(10);
user.setName("Ada Lovelace");
List<User> users = Collections.singletonList(user);

repository.put(user);
repository.put(users);

Optional<Person> ada = repository.get("ada", Person.class);
Iterable<Person> usersFound = repository.get(Collections.singletonList("ada"), Person.class);
```
To use a key-value repository just follows the CDI style and put an `@Inject` on the field.


```java
@Inject
private KeyValueRepository repository;
```

The next step is to produce a **BucketManager**:

```java
@Produces
public BucketManager getManager() {
    BucketManager manager = //instance
    return manager;
}
```

To work with more than one key-value Repository, there are two approaches:

1\) Using qualifieres:

```java
    @Inject
    @Database(value = DatabaseType.KEY_VALUE, provider = "databaseA")
    private KeyValueRepository repositorA;

    @Inject
    @Database(value = DatabaseType.KEY_VALUE, provider = "databaseB")
    private KeyValueRepository repositoryB;


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

2\)  Using the **KeyValueRepositoryProducer** class

```java
@Inject
private KeyValueRepositoryProducer producer;

public void sample() {
   BucketManager managerA = //instance;
   BucketManager managerB = //instance
   KeyValueRepository repositorA = producer.get(managerA);
   KeyValueRepository repositoryB = producer.get(managerB);
}
```
