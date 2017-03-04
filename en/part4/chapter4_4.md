## Persistence events

As mentioned previously, Artemis has support to persistence lifecycle to update and save an Entity. This lifecycle order is managed by a **WorkFlow** class. The default workflow is:

* **firePreEntity**: The Object received from Artemis.
* **firePreAPI**: The object converted to a communication layer.
* **firePostAPI**: The entity connection as a response from the database.
* **firePostEntity**: The entity model from the API low level from the firePostAPI.

Tha watches this event, just need to use an @**Observes**, a form of CDI itself.

#### ColumnWorkFlow

```java
@ApplicationScoped
public class PersonEvent {

    private static final Logger LOGGER = Logger.getLogger(PersonEvent.class.getName());

    public void preEntity(@Observes EntityPrePersist event) {
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

    public void preEntity(@Observes EntityPrePersist event) {
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

    public void preEntity(@Observes EntityPrePersist event) {
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



