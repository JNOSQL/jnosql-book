#### Document

O Document, é a menor parte de uma entidade de uma entidade de documento. Cada documento possui uma tupla em que a chave é o nome do documento e o valor é a informação representado pela classe `Value`.

```java
        Document document = Document.of("name", "value");
        Value value = document.getValue();
        String name = document.getName();
```

Com essa interface também é possível ter um documento dentro de outro documento, o conceito de subdocumento.

```java
Document subDocument = Document.of("subDocument", document);
```

A forma de armazenar essa informação, em subdocumentos, dependerá da implementação de cada driver, assim como toda a informação.

