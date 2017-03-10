## Lidando com os eventos da persistência

Como foi mencionado, o Artemis tem suporte a eventos e um ciclo de vida tanto para a inserção e a atualização dos dados. O ciclo de vida é gerenciado pela classe **WorkFlow** que por padrão tem o seguinte fluxo:

1. **firePreEntity**: O objeto recebido pelo Artemis.
2. **firePreEntityDataBaseType**: Semelhante ao anterior, porém, específico para o tipo de banco de dados, ou seja, cada banco terão seu tipo específico de evento.
3. **firePreAPI**: o objeto convertido numa entidade de comunicação.
4. **firePostAPI**: A entidade de comunicação enviada como resposta do banco de dados.
5. **firePostEntity**: A entidade bean convertida oriunda do firePostAPIAs classes repositórios têm como principal objetivo converter a classe entidade, por exemplo, Person para o Diana, a API de nível de comunicação.
6. **firePostEntityDataBaseType**: Semelhante ao anterior, porém, específico para o tipo de banco de dados, ou seja, cada banco terão seu tipo específico de evento.

Para observar esses eventos, basta utilizar o @**Observes**, recurso oriundo do próprio CDI.

#### ColumnWorkFlow

```java
@ApplicationScoped
public class PersonEvent {

    private static final Logger LOGGER = Logger.getLogger(PersonEvent.class.getName());

    public void observer(@Observes EntityPrePersist event) {
        LOGGER.info("Event to pre persistence" + event.getValue());
    }

    public void observer(@Observes EntityColumnPrePersist event) {
        LOGGER.info("Event to pre document persistence" + event.getValue());
    }

    public void observer(@Observes ColumnEntityPrePersist event) {
        LOGGER.info("Event to pre document entity" + event.getEntity());
    }

    public void observer(@Observes ColumnEntityPostPersist event) {
        LOGGER.info("Event to post document entity" + event.getEntity());
    }

    public void observer(@Observes EntityPostPersit event) {
        LOGGER.info("Event to post persistence" + event.getValue());
    }

    public void observer(@Observes EntityColumnPostPersist event) {
        LOGGER.info("Event to post document entity" + event.getValue());
    }

}
```

#### DocumentWorkFlow

```java
@ApplicationScoped
public class PersonEvent {

    private static final Logger LOGGER = Logger.getLogger(PersonEvent.class.getName());

    public void observer(@Observes EntityPrePersist event) {
        LOGGER.info("Event to pre persistence" + event.getValue());
    }

    public void observer(@Observes EntityDocumentPrePersist event) {
        LOGGER.info("Event to pre document persistence" + event.getValue());
    }

    public void observer(@Observes DocumentEntityPrePersist event) {
        LOGGER.info("Event to pre document entity" + event.getEntity());
    }

    public void observer(@Observes DocumentEntityPostPersist event) {
        LOGGER.info("Event to post document entity" + event.getEntity());
    }

    public void observer(@Observes EntityPostPersit event) {
        LOGGER.info("Event to post persistence" + event.getValue());
    }

    public void observer(@Observes EntityDocumentPostPersist event) {
        LOGGER.info("Event to post document entity" + event.getValue());
    }
}
```

#### KeyValueWorkFlow

```java
@ApplicationScoped
public class UserEvent {

    private static final Logger LOGGER = Logger.getLogger(UserEvent.class.getName());

    public void observer(@Observes EntityPrePersist event) {
        LOGGER.info("Event to pre persistence" + event.getValue());
    }

    public void observer(@Observes EntityKeyValuePrePersist event) {
        LOGGER.info("Event to pre document persistence" + event.getValue());
    }

    public void observer(@Observes KeyValueEntityPrePersist event) {
        LOGGER.info("Event to pre document entity" + event.getEntity());
    }

    public void observer(@Observes KeyValueEntityPostPersist event) {
        LOGGER.info("Event to post document entity" + event.getEntity());
    }

    public void observer(@Observes EntityPostPersit event) {
        LOGGER.info("Event to post persistence" + event.getValue());
    }

    public void observer(@Observes EntityKeyValuePostPersist event) {
        LOGGER.info("Event to post document entity" + event.getValue());
    }

}
```

### Eventos para buscar e deletar informações

Além dos eventos de inserção de atualização, dentro do das APIs de colunas e documentos, o Artemis tem um evento específico para quando uma query de busca ou para remover é lançada.

```java
public class ColumnQueryEvent {

    private static final Logger LOGGER = Logger.getLogger(ColumnQueryEvent.class.getName());

    public void observer(@Observes ColumnQueryExecute event) {
        ColumnQuery query = event.getQuery();
        LOGGER.info("Event to pre persistence" + query);
    }

    public void observer(@Observes ColumnDeleteQueryExecute event) {
        ColumnDeleteQuery query = event.getQuery();
        LOGGER.info("Event to pre persistence" + query);
    }
}


public class DocumentQueryEvent {

    private static final Logger LOGGER = Logger.getLogger(DocumentQueryEvent.class.getName());

    public void observer(@Observes DocumentQueryExecute event) {
        DocumentQuery query = event.getQuery();
        LOGGER.info("Event to pre persistence" + query);
    }

    public void observer(@Observes DocumentDeleteQueryExecute event) {
        DocumentDeleteQuery query = event.getQuery();
        LOGGER.info("Event to pre persistence" + query);
    }
}
```



