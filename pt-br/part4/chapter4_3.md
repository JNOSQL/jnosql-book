## 

## CrudRepisotry

Além dos repositórios de família de colunas e também de documentos o Artemis também possui o CRUDRepository. Essa interface tem como objetivo auxiliar na criação de classes repositórios específicas paraas entidadesalém de facilitar na criação de uma query.

Para utilizar esse recurso é necessário apenas criar uma interface que extenda de **CrudRepository**.

```java
    interface PersonRepository extends CrudRepository<Person> {

    }
```

E injete o recurso, é necessário definir também o qualificador Database que será responsável por dizer para qual tipo de banco a informação será enviada.

```java
@Inject
@Database(DatabaseType.DOCUMENT)
private PersonRepository documentRepository;
@Inject
@Database(DatabaseType.COLUMN)
private PersonRepository columnRepository;
```

Com isso o Artemis se encarregará de utilizar o apropriado banco de dados e cuidará de implementar os métodos.

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

#### Criando Queries com o CrudRepository

Além de salvar e atualizar a informação também é possível recuperar e deletar a informação utilizando queries dinâmicas no método. Com esse intuito o CrudRepository vem com algumas palavras reservadas:

* **findBy**: Como prefixo para encontrar alguma informação
* **deleteBy**: Como prefixo, para deletar alguma informação

Além dos Operadores:

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

Com isso o artemis cuidará de implementar esses métodos.

#### Utilizando o CrudRepository de forma assíncrona

Para trabalhar de forma assíncrona existe a interface CrudRepositoryAsync, seu funcionamento é semelhante ao CrudRepository.

```java
@Inject
@Database(DatabaseType.DOCUMENT)
private PersonRepositoryAsync documentRepositoryAsync;

@Inject
@Database(DatabaseType.COLUMN)
private PersonRepositoryAsync columnRepositoryAsync;
```

Ou seja, basta injetá-lo que o Artemis cuidará de implementar os métodos.

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

Também é possível recuperar e deletar a informação de forma assíncrona, a diferença é que na recuperação um callback é obrigatório no fim do método enquanto para deletar ou remover informação o callback é opcional.

```java
    interface PersonRepositoryAsync extends CrudRepositoryAsync<Person> {
        void findByNickname(String nickname, Consumer<List<Person>> callback);

        void deleteByNickName(String nickname);

        void deleteByNickName(String nickname, Consumer<Void> callback);
    }
```



