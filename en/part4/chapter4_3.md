# Repository

In addition to template class, Artemis has the Repository. This interface helps the Entity repository to save, update, delete and retrieve information.

To use Repository, just need to create a new interface that extends the **Repository**.

```java
    interface PersonRepository extends Repository<Person, String> {

    }
```

The qualifier is mandatory to define the database type that will be used at the injection point moment.

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
```

#### Search information from Repository

The Repository also has a method query from the method name. These are the keywords:

* **findBy**: The prefix to find some information
* **deleteBy**: The prefix to delete some information

Also the operators:

* And
* Or
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
interface PersonRepository extends Repository<Person, Long> {

    List<Person> findByAddress(String address);

    Stream<Person> findByName(String name);

    Stream<Person> findByNameOrderByNameAsc(String name);

    Optional<Person> findByNickname(String nickname);

    void deleteByNickName(String nickname);
}
```

Using these keywords, Artemis will create the queries.

#### Using Repository as asynchronous way

The RepositoryAsync interface works similarly as Repository but with asynchronous work.

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
```

Also, delete and retrieve information with a callback.

```java
    interface PersonRepositoryAsync extends RepositoryAsync<Person, Long> {

        void findByNickname(String nickname, Consumer<List<Person>> callback);

        void deleteByNickName(String nickname);

        void deleteByNickName(String nickname, Consumer<Void> callback);
    }
```

#### Repository at KeyValue

Key-value database has support to **Repository**.

```java
public interface UserRepository extends Repository<User> {
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
public BucketManager getManager() {
BucketManager manager =//instance
return manager;
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
UserRepository userRepository = //instance
User user = new User("ada", "Ada Lovelace", 30);
List<User> users = Collections.singletonList(user);
userRepository.save(user);
userRepository.save(users);


Optional<User> userOptional = userRepository.findById("ada");
Iterable<User> usersFound = userRepository.findById(Collections.singletonList("ada"));
```

In the key-value resource, the **Repository** does not support method query resource.

