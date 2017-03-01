## Lidando com os eventos da persistência

Como foi mencionado, o Artemis tem suporte a eventos e um ciclo de vida tanto para a inserção e a atualização dos dados. O ciclo de vida é gerenciado pela classe **WorkFlow** que por padrão tem o seguinte fluxo:

1. **firePreEntity**: O objeto recebido pelo Artemis
2. **firePreAPI**: o objeto convertido numa entidade de comunicação
3. **firePostAPI**: A entidade de comunicação enviada como resposta do banco de dados
4. **firePostEntity**: A entidade bean convertida oriunda do firePostAPIAs classes repositórios têm como principal objetivo converter a classe entidade, por exemplo, Person para o Diana, a API de nível de comunicação.



#### ColumnWorkFlow

```java
@ApplicationScoped
public class PersonEvent {

    private static final Logger LOGGER = Logger.getLogger(PersonEvent.class.getName());

    public void preEntity(@Observes EntityPostPersit event) {
        LOGGER.info("Event to pre persistence" + event.getValue());
    }

    public void postEntity(@Observes EntityPostPersit event) {
        LOGGER.info("Event to post persistence" + event.getValue());
    }

    public void preColumn(@Observes ColumnEntityPrePersist event) {
        LOGGER.info("Event to pre Column entity" + event.getEntity());
    }

    public void postColumn(@Observes ColumnEntityPostPersist event) {
        LOGGER.info("Event to post column entity" + event.getEntity());
    }
}
```

#### DocumentWorkFlow

```java
@ApplicationScoped
public class PersonEvent {

    private static final Logger LOGGER = Logger.getLogger(PersonEvent.class.getName());

    public void preEntity(@Observes EntityPostPersit event) {
        LOGGER.info("Event to pre persistence" + event.getValue());
    }

    public void postEntity(@Observes EntityPostPersit event) {
        LOGGER.info("Event to post persistence" + event.getValue());
    }

    public void preColumn(@Observes DocumentEntityPrePersist event) {
        LOGGER.info("Event to pre document entity" + event.getEntity());
    }

    public void postColumn(@Observes DocumentEntityPostPersist event) {
        LOGGER.info("Event to post document entity" + event.getEntity());
    }
}
```

#### KeyValueWorkFlow

```java
@ApplicationScoped
public class UserEvent {

    private static final Logger LOGGER = Logger.getLogger(UserEvent.class.getName());

    public void preEntity(@Observes EntityPostPersit event) {
        LOGGER.info("Event to pre persistence" + event.getValue());
    }

    public void postEntity(@Observes EntityPostPersit event) {
        LOGGER.info("Event to post persistence" + event.getValue());
    }

    public void preColumn(@Observes KeyValueEntityPrePersist event) {
        LOGGER.info("Event to pre key entity" + event.getEntity());
    }

    public void postColumn(@Observes KeyValueEntityPostPersist event) {
        LOGGER.info("Event to post key entity" + event.getEntity());
    }
}
```



