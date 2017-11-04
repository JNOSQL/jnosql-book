## Models Annotation

As mentioned previously, Artemis has annotations that make the Java developer life easier; these annotations have two categories:

* Annotation Models

* Qualifier annotation

#### Annotation Models

The annotation model is to convert the entity model to the entity on communication, the Diana entity:


* Entity
* Column
* MappedSuperclass
* Key
* Embeddable
* Convert

##### Entity

This annotation maps the class to Artemis. It has an unique attribute called `name` . This attribute is to inform either the column family name or the document collection name, etc. The default value is the simple name of a class, for example, given the org.jnosql.demo.Person class the default name will `Person`.

```java
@Entity
public class Person {
}
```

```java
@Entity("name")
public class Person {
}
```

##### Column

This annotation it to define which fields on an Entity will be persisted. It also has a unique attribute name to specify that name on Database, and the default value is the field name.

```java
@Entity
public class Person {
    @Column
    private String nickname;
    @Column
    private String name;
    @Column
    private List<String> phones;
    //ignored
    private String address;
//getter and setter
}
```

##### MappedSuperclass

If this annotation is on the parent class, Artemis will persist its information as well. So beyond the son class, Artemis will store any field that is in Parent class with Column annotation.


```java
@Entity
public class Dog extends Animal {

    @Column
    private String name;
    //getter and setter

}

@MappedSuperclass
public class Animal {

    @Column
    private String race;

    @Column
    private Integer age;

    //getter and setter

}
```

On this sample above, when saves a `Dog` instance, it saves the `Animal` case too, explicitly will save the fields `name`, `race` and `age`.


##### Id

Just for the key-value database, it shows which attribute is the key, thus the value will be the remaining information. The way of storing the class will depend on the database driver.

```java
@Entity
public class User implements Serializable {

    @Id
    private String userName;

    private String name;

    private List<String> phones;
    }
```
##### Embeddable

Defines a class whose instances are stored as an intrinsic part of an owning entity and share the identity of the object. So when converts an Embeddable instance to either save or update this is going to be either subdocument or subcolumn.

```java
@Entity
public class Book {

    @Column
    private String name;

    @Column
    private Author author;

//getter and setter

}

@Embeddable
public class Author {

    @Column
    private String name;

    @Column
    private Integer age;

//getter and setter

}
```

##### Convert

As Diana, Artemis has a converter at abstraction level. This feature is useful, e.g., to cipher a field, String to String, or just to do a converter to a custom type using annotation. The `Converter` annotation has a parameter, and an AttributeConverter implementation class can be used. Eg. The sample bellow shows how to create a converter to a custom Money class.

```java
@Entity
public class Worker {
    @Column
    private String name;
    @Column
    private Job job;
    @Column("money")
    @Convert(MoneyConverter.class)
    private Money salary;
//getter and setter
}

public class MoneyConverter implements AttributeConverter<Money, String>{
    @Override
    public String convertToDatabaseColumn(Money attribute) {
        return attribute.toString();
    }
    @Override
    public Money convertToEntityAttribute(String dbData) {
        return Money.parse(dbData);
    }
}
public class Money {
    private final String currency;

    private final BigDecimal value;

//....
}
```

#### Qualifier annotation

That is important to work with more than one type of the same application.

```java
@Inject
private DocumentRepository repositoryA;
@Inject
private DocumentRepository repositoryB;
```

Two injections with the same interface, CDI throws an ambiguous exception. There is the `Database` qualifier to fix this problem. It has two attributes:

* **DatabaseType**: The database type, key-value, document, column, graph.

* **provider**: The provider database name, eg. "cassandra, "hbase", "mongodb". So using the `Database` qualifier:


```java
@Inject
@Database(value = DatabaseType.DOCUMENT, provider = “databaseA”)
private DocumentRepository repositoryA;
@Inject
@Database(value = DatabaseType.DOCUMENT, provider = “databaseB”)
private DocumentRepository repositoryB;
```

Beyond this annotation, the producer method with the entity manager is required.

The benefits using this qualifier instead of creating a new one is that if the Manager Entity is produced using `Database` as a qualifier, Artemis will create classes such as DocumentRepository, ColumnRepository, etc. automatically.
