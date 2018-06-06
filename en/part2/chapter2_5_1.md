# chapter2\_5\_1

The factory classes have the duty to create the column family manager.

* **ColumnFamilyManagerAsyncFactory**
* **ColumnFamilyManagerFactory**

The `ColumnFamilyManagerAsyncFactory` and `ColumnFamilyManagerFactory` creates the manager synchronously and asynchronously respectively.

```java
ColumnFamilyManagerFactory factory = //instance
ColumnFamilyManagerAsyncFactory asyncFactory = //instance
ColumnFamilyManager manager = factory.get("database");
ColumnFamilyManagerAsync managerAsync = asyncFactory.getAsync("database");
```

The factories were separated intentionally, as not all databases support synchronous and asynchronous operations.

