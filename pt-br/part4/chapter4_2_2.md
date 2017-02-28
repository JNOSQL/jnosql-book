## Repositório de Família de colunas

O repositório de família de coluna é responsável para realizar a comunicação da entidade para um banco de dados do tipo família de coluna. Ele é subdividido em `ColumnRepository` e `ColumnRepositoryAsync` para trabalhos síncronos e assíncronos respectivamente.

#### ColumnRepository

O ColumnRepository é responsável pela persistência de uma Entidade em um banco de dados do tipo coluna. Ele é composto, basicamente, por três componentes:

* **ColumnEntityConverter**: Responsável por converter da entidade, por exemplo, Person para ColumnEntity.

* **ColumnCollectionManager**: Entidade manager de documentos do Diana.

* **ColumnWorkflow**: Segue o fluxo de persistência durante os métodos de save e update.

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

Para a busca e a remoção da informação são utilizadas as mesmas classes do Diana para documentos, ou seja, ColumnQuery e **ColumnDeleteQuery** respectivamente.

```java
ColumnQuery query = DocumentQuery.of("Person");
query.and(ColumnCondition.eq(Column.of("address", "Olympus")));

List<Person> peopleWhoLiveOnOlympus = repository.find(query);
Optional<Person> artemis = repository.singleResult(ColumnQuery.of("Person")
                .and(ColumnCondition.eq(Column.of("nickname", "artemis"))));

ColumnDeleteQuery deleteQuery = query.toDeleteQuery();
repository.delete(deleteQuery);
```

Como o motor do Artemis é CDI para que se posso utilizar o ColumnRepository basta dar um @Inject num campo.

```java
@Inject
private ColumnRepository repository;
```

Para isso é necessário que a aplicação injete um ColumnFamilyManager**:**

```java
@Produces
public ColumnFamilyManager getManager() {
    ColumnFamilyManager manager = //instance
    return manager;
}
```

Para trabalhar com mais de um tipo de ColumnRepository existem duas opções:

1\) A primeira é com a utilização dos qualificadores:

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

2\) A segunda delas é a partir do  **ColumnRepositoryProducer**

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

O `ColumnRepositoryAsync` é responsável pela persistência de uma Entidade em um banco de dados do tipo família de colunas de forma assíncrona. Ele é composto, basicamente, por dois componentes:

* **DocumentEntityConverter:** Responsável por converter da entidade, por exemplo, Person para DocumentEntity.

* **DocumentCollectionManagerAsync:** Entidade manager de documentos do Diana de forma assíncrona.

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

Para a busca e a remoção da informação são utilizadas as mesmas classes do Diana para documentos, ou seja, **DocumentQuery** e **DocumentDeleteQuery** respectivamente também é possível o uso de callback.

```java
Consumer<List<Person>> callBackPeople = p -> {};
Consumer<Void> voidCallBack = v ->{};
repositoryAsync.find(query, callBackPeople);
repositoryAsync.delete(deleteQuery);
repositoryAsync.delete(deleteQuery, voidCallBack);
```

Como o motor do Artemis é CDI para que se posso utilizar o DocumentRepository basta dar um @Inject num campo.

```java
@Inject
private
DocumentRepositoryAsync repository;
```

Para isso é necessário que a aplicação injete um **DocumentCollectionManagerAsync:**

```
@Produces
public DocumentCollectionManagerAsync getManager() {
    DocumentCollectionManagerAsync managerAsync = //instance
    return manager;
}
```

Para trabalhar com mais de um tipo de DocumentRepository existem duas opções:

1\) A primeira é com a utilização dos qualificadores:

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
        DocumentCollectionManager manager = null;
        return manager;
    }

    @Produces
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
    public DocumentCollectionManagerAsync getManagerB() {
        DocumentCollectionManager manager = null;
        return manager;
    }
```

2\) A segunda delas é a partir do  **DocumentRepositoryAsyncProducer**

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



