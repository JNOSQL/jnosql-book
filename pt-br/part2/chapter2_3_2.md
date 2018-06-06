# DocumentEntity

O `DocumentEntity` é a representação da entidade que será presentado dentro de um banco de dados do tipo documentos. Ela é composta por um ou mais documentos, o documento por sua vez, assim como a coluna, é composta por um valor, representado Value, e o seu respectivo nome.

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

