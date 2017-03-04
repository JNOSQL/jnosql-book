#### Document

The `Document` is a small piece of a Document entity. Each document has a tuple where the key is the document name, and the value is the information itself as `Value`.

```java
        Document document = Document.of("name", "value");
        Value value = document.getValue();
        String name = document.getName();
```

The document might an another document inside, the subdocument concept.

```java
Document subDocument = Document.of("subDocument", document);
```

The way to storage subdocuments will also depend on each driver implementation as all information as well.

To access the information from `Document` it has alias method to `Value`, in other words,  do a conversion directly from `Document` _interface_.

```java
Document age = Document.of("age", 29);
String ageString = age.get(String.class);
List<Integer> ages = age.get(new TypeReference<List<Integer>>() {});
Object ageObject = age.get();
```



