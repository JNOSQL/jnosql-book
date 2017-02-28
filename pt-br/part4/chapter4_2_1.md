## Repositório de Documentos

O repositório de documentos é responsável para realizar a comunicação da entidade para um banco de dados do tipo documentos, ele é subdividido em `DocumentRepository` e `DocumentRepositoryAsync`para trabalhos síncronos e assíncronos respectivamente.

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

        List<Person> persons = Collections.singletonList(person);

        Person personUpdated = repository.save(person);
        repository.save(persons);

        repository.update(person);
        repository.update(persons);

```

Para a busca e a remoção da informação são utilizadas as mesmas classes do Diana para documentos, ou seja,   
**DocumentQuery** **DocumentDeleteQuery** respectivamente.

