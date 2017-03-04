#### Column Family Manager Factory

The factories classes have the duty to create the column family manager.

* **ColumnFamilyManagerAsyncFactory**
* **ColumnFamilyManagerFactory**

The `ColumnFamilyManagerAsyncFactory` and `ColumnFamilyManagerFactory` is who create the manager synchronous and asynchronous respective.

```java
ColumnFamilyManagerFactory factory = //instance
ColumnFamilyManagerAsyncFactory asyncFactory = //instance
ColumnFamilyManager manager = factory.get("database");
ColumnFamilyManagerAsync managerAsync = asyncFactory.getAsync("database");
```

There are two factories because there is a database that just supports either synchronous or asynchronous.

