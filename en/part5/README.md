## Artemis components

Everything is configurable on Artemis, to solve the massive number of databases in NoSQL world. The components are replaceable, just defining the Alternative, from CDI. Also, a developer can wrapper without a conflict without change the source core.



* **Workflow**: That defines the workflow when either save or update an entity. As show the picture bellow. These events are useful when you, eg., want to validate data before be saved.

* **EventManager**: The component that **Workflow** uses to send an event, so this element does not define the order just the event way. So, a developer can replace the fire synchronous to asynchronous using an EventManager implementation.

* **Converter**: That converts the Entity to a communication level API.

* **Repository**: The "regent" of the synchronous persistence

* **RepositoryAsync**: The regent of the asynchronous persistence
