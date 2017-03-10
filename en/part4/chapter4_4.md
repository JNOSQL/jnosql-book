## Persistence events

As mentioned previously, Artemis has support to persistence lifecycle to update and save an Entity. This lifecycle order is managed by a **WorkFlow** class. The default workflow is:

* **firePreEntity**: The Object received from Artemis.
* **firePreAPI**: The object converted to a communication layer.
* **firePostAPI**: The entity connection as a response from the database.
* **firePostEntity**: The entity model from the API low level from the firePostAPI.

Tha watch this event, just need to use an @**Observes**, a form of CDI itself.

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



