##

## CrudRepisotry

In addition to repositories class, Artemis has the CRUDRepository. This interface helps the Entity repository to save, update, delete and retrieve information.

To use CrudRepository, just need to create a new interface that extends the **CrudRepository**.

```java
    interface PersonRepository extends CrudRepository<Person> {

    }
```

The qualifier is mandatory to define the database type whose will use at the injection point moment.

```java
@Inject
@Database(DatabaseType.DOCUMENT)
private PersonRepository documentRepository;
@Inject
@Database(DatabaseType.COLUMN)
private PersonRepository columnRepository;
```

And then, as the repository class, create either a **ColumnFamilyManager** or a **DocumentCollectionManager** with produces method:

```java
@Produces
public DocumentCollectionManager getManager() {
DocumentCollectionManager manager = //instance
return manager;
}

@Produces
public ColumnFamilyManager getManager() {
ColumnFamilyManager manager = //instance
return manager;
}
```

To work with multiple database you can use qualifiers:

```java
@Inject
@Database(value = DatabaseType.DOCUMENT , provider = "databaseA")
private PersonRepository documentRepositoryA;

@Inject
@Database(value = DatabaseType.DOCUMENT , provider = "databaseB")
private PersonRepository documentRepositoryB;

@Inject
@Database(value = DatabaseType.COLUMN, provider = "databaseA")
private PersonRepository columnRepositoryA;

@Inject
@Database(value = DatabaseType.COLUMN, provider = "databaseB")
private PersonRepository columnRepositoryB;


//producers methods
@Produces
@Database(value = DatabaseType.COLUMN, provider = "databaseA")
public ColumnFamilyManager getColumnFamilyManagerA() {
ColumnFamilyManager manager =//instance
return manager;
}

@Produces
@Database(value = DatabaseType.COLUMN, provider = "databaseB")
public ColumnFamilyManager getColumnFamilyManagerB() {
ColumnFamilyManager manager = //instance
return manager;
}

@Produces
@Database(value = DatabaseType.DOCUMENT, provider = "databaseA")
public DocumentCollectionManager getDocumentCollectionManagerA() {
DocumentCollectionManager manager = //instance
return manager;
}

@Produces
@Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
public DocumentCollectionManager DocumentCollectionManagerB() {
DocumentCollectionManager manager = //instance
return manager;
}
```

So, Artemis will inject automatically.

```java
PersonRepository repository = //instance

Person person = new Person();
person.setNickname("diana");
person.setName("Diana Goodness");

List<Person> people = Collections.singletonList(person);

repository.save(person);
repository.save(people);
repository.save(people, Duration.ofHours(2));
repository.update(person);
repository.update(people);
repository.update(people);
```

#### Search information from CrudRepository

The CRUDRepository also has a dynamic query from the method name. These are the keywords:

* **findBy**: The prefix to find some information
* **deleteBy**: The prefix to delete some information

Also the operators:

* AND
* OR
* Between
* LessThan
* GreaterThan
* LessThanEqual
* GreaterThanEqual
* Like
* OrderBy
* OrderBy\_\_\_\_Desc
* OrderBy\_\_\_\_\_ASC

```java
interface PersonRepository extends CrudRepository<Person> {

    List<Person> findByAddress(String address);

    Stream<Person> findByName(String name);

    Stream<Person> findByNameOrderByNameAsc(String name);

    Optional<Person> findByNickname(String nickname);

    void deleteByNickName(String nickname);
}
```

Using these keywords, Artemis will create the queries.

#### Using CrudRepository as asynchronous way

The CrudRepositoryAsync interface works similarly as CrudRepository but with asynchronous work.


```java
@Inject
@Database(DatabaseType.DOCUMENT)
private PersonRepositoryAsync documentRepositoryAsync;

@Inject
@Database(DatabaseType.COLUMN)
private PersonRepositoryAsync columnRepositoryAsync;
```

In other words, just inject and then create an Entity Manager async with producers method.

```java
PersonRepositoryAsync repositoryAsync = //instance

Person person = new Person();
person.setNickname("diana");
person.setName("Diana Goodness");

List<Person> people = Collections.singletonList(person);


repositoryAsync.save(person);
repositoryAsync.save(people);
repositoryAsync.save(person, p -> {});
repositoryAsync.save(people, Duration.ofHours(2));
repositoryAsync.update(person);
repositoryAsync.update(person, p -> {});
repositoryAsync.update(people);
repositoryAsync.update(people);
```

Also, delete and retrieve information with a callback.

```java
    interface PersonRepositoryAsync extends CrudRepositoryAsync<Person> {

        void findByNickname(String nickname, Consumer<List<Person>> callback);

        void deleteByNickName(String nickname);

        void deleteByNickName(String nickname, Consumer<Void> callback);
    }
```

#### KeyValueCrudRepository

The KeyValueCrudRepository is a CRUDRepository to key-value type.

If the same way of CrudRepository, just extends **KeyValueCrudRepository**.

```java
public interface UserRepository extends KeyValueCrudRepository<User> {
}
```

And inject the resource.

```java
@Inject
private UserRepository userRepository;
```

Then use a producer to BucketManager

```java
@Produces
public BucketManager getManager() {
BucketManager manager =//instance
return manager;
}
```

You can use qualifier when you want to use a different database in the same application.

```java
@Inject
@Database(value = DatabaseType.KEY_VALUE, provider = "databaseA")
private UserRepository userRepositoryA;
@Inject
@Database(value = DatabaseType.KEY_VALUE, provider = "databaseB")
private UserRepository userRepositoryB;
//producers methods
@Produces
@Database(value = DatabaseType.KEY_VALUE, provider = "databaseA")
public BucketManager getManager() {
    BucketManager manager =//instance
    return manager;
}
@Produces
@Database(value = DatabaseType.KEY_VALUE, provider = "databaseB")
public BucketManager getManagerB() {
    BucketManager manager = //instance
    return manager;
}
```

Once there is not another to both delete and find information, there isn't dynamic query.

```java
UserRepository userRepository = null;
User user = new User("ada", "Ada Lovelace", 30);
List<User> users = Collections.singletonList(user);
userRepository.put(user);
userRepository.put(user, Duration.ofHours(1));
userRepository.put(users);
userRepository.put(users, Duration.ofHours(1));

Optional<User> userOptional = userRepository.get("ada");
Iterable<User> usersFound = userRepository.get(Collections.singletonList("ada"));
```
