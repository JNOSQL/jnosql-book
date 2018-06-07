# Document Configuration

On the document collection configuration, there are two classes, `DocumentConfiguration` and `DocumentConfigurationAsync` to `DocumentCollectionManagerFactory` and `DocumentCollectionManagerAsyncFactory` respectively.

```java
DocumentConfiguration configuration = //instance
DocumentConfigurationAsync configurationAsync = //instance
DocumentCollectionManagerFactory managerFactory = configuration.get();
DocumentCollectionManagerAsyncFactory managerAsyncFactory = configurationAsync.getAsync();
```

If a database has support to both synchronous and asynchronous, it may use `UnaryDocumentConfiguration` that implement both document configuration.

```java
UnaryDocumentConfiguration unaryDocumentConfiguration = //instance
DocumentCollectionManagerFactory managerFactory = unaryDocumentConfiguration.get();
DocumentCollectionManagerAsyncFactory managerAsyncFactory = unaryDocumentConfiguration.getAsync();
```

