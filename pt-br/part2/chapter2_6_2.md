# Column Configuration

Dentro da configuração para os bancos de dados do tipo família de colunas existem o `ColumnConfiguration` e `ColumnConfigurationAsync` para a criação do Manager factory síncrono e assíncrono respectivamente.

```java
ColumnConfiguration configuration = //instance
ColumnConfigurationAsync configurationAsync = //instance
ColumnFamilyManagerFactory managerFactory = configuration.get();
ColumnFamilyManagerAsyncFactory managerAsyncFactory = configurationAsync.getAsync();
```

O motivo da separação de duas configurações é que nem todos os bancos de dados suportam as operações síncronas e assíncronas. Caso o banco de dados suporte as duas operações o provedor pode optar por utilizar o `UnaryColumnConfiguration` no qual é uma configuração que implementa as duas classes anteriores.

```java
UnaryColumnConfiguration unaryDocumentConfiguration = //instance
ColumnFamilyManagerFactory managerFactory = unaryDocumentConfiguration.get();
ColumnFamilyManagerAsyncFactory managerAsyncFactory = unaryDocumentConfiguration.getAsync();
```

