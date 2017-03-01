## Repositório de Chave Valor

O repositório de chave valor é responsável para realizar a comunicação da entidade para um banco de dados do tipochave valor.

#### `KeyValueRepository`

O KeyValuRepository é responsável pela persistência de uma Entidade em um banco de dados do tipo chave valor. Ele é composto, basicamente, por dois componentes:

* **KeyValueEntityConverter**: Responsável por converter uma entidade, por exemplo, User para KeyValueEntity

* **BucketManager**: entidade manager de chave valor do Diana.

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

Como o motor do Artemis é CDI para que se posso utilizar o DocumentRepository basta dar um @Inject num campo.

```java
@Inject
private KeyValueRepository repository;
```

Para isso é necessário que a aplicação injete um BucketManager:
```java
@Produces
public BucketManager getManager() {
    BucketManager manager = //instance
    return manager;
}
```

Para trabalhar com mais de um tipo de KeyValueRepository existem duas opções:

1) A primeira é com a utilização dos qualificadores:
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
