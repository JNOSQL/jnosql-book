#### Document Collection Factory

The factory classes have the duty to create the document collection manager.

* **DocumentCollectionManagerFactory**
* **DocumentCollectionManagerAsyncFactory**

The `DocumentCollectionManagerAsyncFactory` and `DocumentCollectionManagerFactory` creates the manager synchronously and asynchronously respectively.

```java
DocumentCollectionManagerFactory factory = //instance
DocumentCollectionManagerAsyncFactory asyncFactory = //instance
DocumentCollectionManager manager = factory.get("database");
DocumentCollectionManagerAsync managerAsync = asyncFactory.getAsync("database");

```

The factories were separated intentionally, as not all databases support synchronous and asynchronous operations.
