#### Document Collection Factory

The factories classes have the duty to create the document collection manager.

* **DocumentCollectionManagerFactory**
* **DocumentCollectionManagerAsyncFactory**

The `DocumentCollectionManagerAsyncFactory` and `DocumentCollectionManagerFactory` is who create the manager synchronous and asynchronous respective.

```java
DocumentCollectionManagerFactory factory = //instance
DocumentCollectionManagerAsyncFactory asyncFactory = //instance
DocumentCollectionManager manager = factory.get("database");
DocumentCollectionManagerAsync managerAsync = asyncFactory.getAsync("database");

```

There are two factories because there is a database that just supports either synchronous or asynchronous.
