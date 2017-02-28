## Repositório de chave valor

  


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



