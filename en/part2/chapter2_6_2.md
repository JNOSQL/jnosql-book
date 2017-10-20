## Column Configuration

On the column family configuration, there are two classes, ColumnConfiguration and ColumnConfigurationAsync to ColumnFamilyManagerFactory and ColumnFamilyManagerAsyncFactory respective.



```java
ColumnConfiguration configuration = //instance
ColumnConfigurationAsync configurationAsync = //instance
ColumnFamilyManagerFactory managerFactory = configuration.get();
ColumnFamilyManagerAsyncFactory managerAsyncFactory = configurationAsync.getAsync();
```

If a database has support to both synchronous and asynchronous, it may use `UnaryColumnConfiguration` that implement both document configuration.


```java
UnaryColumnConfiguration unaryDocumentConfiguration = //instance
ColumnFamilyManagerFactory managerFactory = unaryDocumentConfiguration.get();
ColumnFamilyManagerAsyncFactory managerAsyncFactory = unaryDocumentConfiguration.getAsync();
```
