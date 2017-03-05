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

A forma de armazenar essa informação, em subdocumentos, dependerá da implementação de cada driver.

Para facilitar o acesso da informação, o `Document`, possui alias para os métodos do `Value`, ou seja, é possível realizar a conversão do valor diretamente na _interface_ `Document`.

```java
Document age = Document.of("age", 29);
String ageString = age.get(String.class);
List<Integer> ages = age.get(new TypeReference<List<Integer>>() {});
Object ageObject = age.get();
```



