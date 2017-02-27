## Anotações para o Modelo

Como mencionado anteriormente, o Artemis é orientado a anotações para que facilite a vida do desenvolvedor Java. Essas anotações têm algumas categorias:

* Anotação para modelo

* Anotação para interceptor

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
    private long id;
    @Column
    private String name;
    @Column
    private List<String> phones;
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

    @Column
    @Key
    private String userName;

    private String name;

    private List<String> phones;
    }
```



