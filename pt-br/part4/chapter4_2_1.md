## Repositório de Documentos

O repositório de documentos é responsável para realizar a comunicação da entidade para um banco de dados do tipo documentos. Ele é subdividido em `DocumentRepository` e `DocumentRepositoryAsync`para trabalhos síncronos e assíncronos respectivamente.

#### `DocumentRepository`

O `DocumentRepository` é responsável pela persistência de uma Entidade em um banco de dados do tipo documento. Ele é composto, basicamente, por três componentes:

* **DocumentEntityConverter**: Responsável por converter da entidade, por exemplo, Person para DocumentEntity.

* **DocumentCollectionManager**: Entidade manager de documentos do Diana.

* **DocumentWorkflow**: Segue o fluxo de persistência durante os métodos de save e update.

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

repository.update(person);
repository.update(people);
```

Para a busca e a remoção da informação são utilizadas as mesmas classes do Diana para documentos, ou seja,  
**DocumentQuery** e **DocumentDeleteQuery** respectivamente.

```java
DocumentQuery query = DocumentQuery.of("Person");
query.and(DocumentCondition.eq(Document.of("address", "Olympus")));

List<Person> peopleWhoLiveOnOlympus = repository.find(query);
Optional<Person> artemis = repository.singleResult(DocumentQuery.of("Person")
                .and(DocumentCondition.eq(Document.of("nickname", "artemis"))));

DocumentDeleteQuery deleteQuery = query.toDeleteQuery();
repository.delete(deleteQuery);
```

Como o motor do Artemis é CDI para que se posso utilizar o DocumentRepository basta dar um @Inject num campo.

```java
@Inject
private DocumentRepository repository;
```

Para isso é necessário que a aplicação injete um **DocumentCollectionManager:**

```java
@Produces
public DocumentCollectionManager getManager() {
    DocumentCollectionManager manager = //instance
    return manager;
}
```



  
Para trabalhar com mais de um tipo de DocumentRepository existem duas opções:



1\) A primeira é com a utilização dos qualificadores:



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
        DocumentCollectionManager manager = null;
        return manager;
    }

    @Produces
    @Database(value = DatabaseType.DOCUMENT, provider = "databaseB")
    public DocumentCollectionManager getManagerB() {
        DocumentCollectionManager manager = null;
        return manager;
    }
```



