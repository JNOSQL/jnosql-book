# DocumentEntity

The `DocumentEntity` is an entity to document collection database type. It is composed of one or more document. As a result, the Document is a tuple of name and value.

```java
        DocumentEntity entity = DocumentEntity.of("documentFamily");
        String name = entity.getName();
        entity.add(Document.of("id", Value.of(10L)));
        entity.add(Document.of("version", 0.001));
        entity.add(Document.of("name", "Diana"));
        entity.add(Document.of("options", Arrays.asList(1, 2, 3)));

        List<Document> documents = entity.getDocuments();
        Optional<Document> id = entity.find("id");
        entity.remove("options");
```

