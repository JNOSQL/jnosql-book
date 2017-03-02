## Anotações para o Modelo

Como mencionado anteriormente, o Artemis é orientado a anotações para que facilite a vida do desenvolvedor Java. Essas anotações têm algumas categorias:

* Anotação para modelo

* Anotação para qualificação

#### Anotações para o Modelo

As anotações para o Modelo tem como objetivo transformar o modelo, orientado a objetos, para a camada de comunicação, Diana. Para mapeamento do modelo existe:

* Entity

* Column

* MappedSuperclass

* Key

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

##### Key

Apenas para o banco de dados do tipo chave-valor, ele indica qual dos atributos é a chave o valor será toda a informação restante. A forma de armazenamento da classe vai depender do driver do banco de dados.

```java
@Entity
public class User implements Serializable {

    @Key
    private String userName;

    private String name;

    private List<String> phones;
    }
```

#### Anotação para qualificação

Em alguns momentos é necessário trabalhar com o mesmo tipo de banco de dados, por exemplo, trabalhar com dois bancos do tipo documentos.

```java
@Inject
private DocumentRepository repositoryA;
@Inject
private DocumentRepository repositoryB;
```

Como nos dois casos ele implementa a mesma interface será retornado em tempo de execução pelo CDI um problema de ambiguidade de injeção. Para resolver esse problema existe o qualificador que a API já traz, o qualificador `Database`. Essa API possui dois atributos:

* **DatabaseType**: O tipo de banco de dados, por exemplo, chave-valor documentos, grafo ou família de coluna

* **privider**: O nome do provedor do banco de dados, por exemplo, “cassandra”, “hbase”, “mongoDB”, etc. Assim, para resolver o problema mencionado anteriormente existe é necessário anotar com o qualificador.

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

