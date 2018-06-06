# chapter2\_6\_1

Dentro da configuração para os bancos de dados do tipo documentos existem o `DocumentConfiguration` e `DocumentConfigurationAsync` para a criação do Manager factory síncrono e assíncrono respectivamente.

```java
DocumentConfiguration configuration = //instance
DocumentConfigurationAsync configurationAsync = //instance
DocumentCollectionManagerFactory managerFactory = configuration.get();
DocumentCollectionManagerAsyncFactory managerAsyncFactory = configurationAsync.getAsync();
```

O motivo da separação de duas configurações é que nem todos os bancos de dados suportam as operações síncronas e assíncronas. Caso o banco de dados suporte as duas operações o provedor pode optar por utilizar o `UnaryDocumentConfiguration` no qual é uma configuração que implementa as duas classes anteriores.

```java
UnaryDocumentConfiguration unaryDocumentConfiguration = //instance
DocumentCollectionManagerFactory managerFactory = unaryDocumentConfiguration.get();
DocumentCollectionManagerAsyncFactory managerAsyncFactory = unaryDocumentConfiguration.getAsync();
```

