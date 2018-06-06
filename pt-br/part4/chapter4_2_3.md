# Template de Chave valor

O template de chave valor é responsável para realizar a comunicação da entidade para um banco de dados do tipochave valor.

## `KeyValueTemplate`

O KeyValuTemplate é responsável pela persistência de uma Entidade em um banco de dados do tipo chave valor. Ele é composto, basicamente, por três componentes:

* **KeyValueEntityConverter**: Responsável por converter uma entidade, por exemplo, User para KeyValueEntity
* **BucketManager**: entidade manager de chave valor do Diana.
* **KeyValueWorkflow**: O workflow para os bancos do tipo chave valor.

```java
KeyValueTemplate template = null;
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

Como o motor do Artemis é CDI para que se posso utilizar o KeyValueTemplate basta dar um @Inject num campo.

```java
@Inject
private KeyValueTemplate template;
```

Para isso é necessário que a aplicação injete um BucketManager:

```java
@Produces
public BucketManager getManager() {
    BucketManager manager = //instance
    return manager;
}
```

Para trabalhar com mais de um tipo de KeyValueTemplate existem duas opções:

1\) A primeira é com a utilização dos qualificadores:

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

2\) A segunda delas é a partir do KeyValueTemplateProducer

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

