#### Document

O Document, é a menor parte de uma entidade de uma entidade de documento. Cada documento possui uma tupla em que a chave é o nome do documento e o valor é a informação representado pela classe `Value`.



```java
        Document document = Document.of("name", "value");
        Value value = Value.of(10);
        Document document1 = Document.of("name", value);
```



