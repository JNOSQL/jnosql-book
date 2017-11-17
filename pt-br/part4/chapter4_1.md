## Anotações para o Modelo

Como mencionado anteriormente, o Artemis é orientado a anotações para que facilite a vida do desenvolvedor Java. Essas anotações têm algumas categorias:

* Anotação para modelo

* Anotação para qualificação

#### Anotações para o Modelo

As anotações para o Modelo tem como objetivo transformar o modelo, orientado a objetos, para a camada de comunicação, Diana. Para mapeamento do modelo existe:

* Entity
* Column
* MappedSuperclass
* Id
* Embeddable
* Convert

O JNoSQL Artemis não requer métodos getter e setter para os campos, no entanto, as classes do tipo entidade precisam ter um construtor padrão ou protegido por pacote.

##### Entity

Essa anotação tem como objetivo mapear a classe que será utilizada como entidade dentro do Artemis, ela possui um único atributo: name. Esse atributo tem como objetivo informar o nome da Família de coluna, Coleção de documentos, chave valor, etc. Caso ele não seja informado será utilizado o nome simples da classe, por exemplo, a Classe org.jnosql.demo.Person ele usará o Person como nome.

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

Essa anotação observada os atributos dentro da classe, anotada com Entity.

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

Faz com que o Artemis olhe para os atributos da classe pai, cujo os atributos sejam anotados com Column,

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



No exemplo citado, ao salvar a classe `Dog` será levado em consideração também os campos da classe `Animal`, ou seja, serão persistidos três campos, `name`, `race` e `age`.

##### Id

Ele indica qual dos atributos é o id, ou a chave para banco de dados do tipo chave-valor. A forma de armazenamento da classe vai depender do driver do banco de dados.

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

Indica que as classes com essa anotação serão persistidas de forma embarcada, ou seja, na conversão de uma entidade para o tipo documentos o tipo embarcado será um suddocumento.

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



Assim como o Diana, o Artemis também possui o recurso para realizar conversão de tipos. Esse tipo de recurso é interessante também para, por exemplo, criptografar uma informação texto, de String para String. Como parâmetro essa anotação precisa de uma classe que implemente AttributeConverter. No exemplo abaixo, foi criado a classe salário com um converter para a classe criada Money.

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

## Anotação para qualificação

Em alguns momentos é necessário trabalhar com o mesmo tipo de banco de dados, por exemplo, trabalhar com dois bancos do tipo documentos.

```java
@Inject
private DocumentRepository repositoryA;
@Inject
private DocumentRepository repositoryB;
```

Como nos dois casos ele implementa a mesma interface será retornado em tempo de execução pelo CDI um problema de ambiguidade de injeção. Para resolver esse problema existe o qualificador que a API já traz, o qualificador `Database`. Essa API possui dois atributos:

* **DatabaseType**: O tipo de banco de dados, por exemplo, chave-valor documentos, grafo ou família de coluna

* **provider**: O nome do provedor do banco de dados, por exemplo, “cassandra”, “hbase”, “mongoDB”, etc. Assim, para resolver o problema mencionado anteriormente existe é necessário anotar com o qualificador.

```java
@Inject
@Database(value = DatabaseType.DOCUMENT, provider = “databaseA”)
private DocumentRepository repositoryA;
@Inject
@Database(value = DatabaseType.DOCUMENT, provider = “databaseB”)
private DocumentRepository repositoryB;
```

Obviamente será necessário a criação de métodos produtores com os mesmos qualificadores que falaremos mais a frente.

Um ponto importante é da integração do Artemis com esse qualificador. Caso ele seja utilizado para a criação dos Entity Manager do Diana \(ColumnFamilyManager, DocumentCollectionManager, BucketManager, etc.\) o Artemis gerenciará todo o ciclo de vida de classes como DocumentRepository, ColumnRepository, dentro outros que será visto mais a frente.


## ConfigurationUnit

Armazenar as configurações do banco de dados como senha e usuário é algo muito importante para uma aplicação. O Eclipse JNoSQL Artemis tem a anotação ConfigurationUnit, ela lê a configuração a partir de arquivos como XML e JSON e o injeta criando uma fábrica. Esse arquivo de configuração pode estar na parta **META-INF** ou **WEB-INF** 

#### Estrutura de um arquivo JSON


```json
[
   {
      "description":"that is the description",
      "name":"name",
      "provider":"class",
      "settings":{
         "key":"value"
      }
   },
   {
      "description":"that is the description",
      "name":"name-2",
      "provider":"class",
      "settings":{
         "key":"value"
      }
   }
]

```

#### Estrutura de um arquivo XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configurations>
   <configuration>
      <description>that is the description</description>
      <name>name</name>
      <provider>class</provider>
      <settings>
         <entry>
            <key>key2</key>
            <value>value2</value>
         </entry>
         <entry>
            <key>key</key>
            <value>value</value>
         </entry>
      </settings>
   </configuration>
</configurations>
```

#### Injection the code 

Com o arquivo de configuração, o próximo passo é o injetar na aplicação, o Eclipse JNoSQL Artemis, por padrão, tem suporte para as seguintes classes:

* BucketManagerFactory
* DocumentCollectionManagerAsyncFactory
* DocumentCollectionManagerAsyncFactory
* ColumnFamilyManagerAsyncFactory
* ColumnFamilyManagerAsyncFactory

```java

    @Inject
    @ConfigurationUnit(fileName = "column.xml", name = "name")
    private ColumnFamilyManagerFactory<?> factoryA;

    @Inject
    @ConfigurationUnit(fileName = "document.json", name = "name-2")
    private DocumentCollectionManagerFactory factoryB;

    @Inject
    @ConfigurationUnit
    private BucketManagerFactory factoryB;

```

#### Dependência do ConfigurationUnit

Para utilizar a anotação o seu comportamento padrão de ConfigurationUnit, é necessário adicionar a  dependência de configuração.

```xml
   <dependency>
        <groupId>org.jnosql.artemis</groupId>
        <artifactId>artemis-configuration</artifactId>
        <version>0.0.4-SNAPSHOT</version>
    </<dependency>
    
```
