#### DocumentCollectionEntity

The `DocumentCollectionEntity` is an entity to document collection database type. It is composed of one or more document. As a result, the Document is a tuple of name and value.

```java
        ColumnEntity entity = ColumnEntity.of("columnFamily");
        String name = entity.getName();
        entity.add(Column.of("id", Value.of(10L)));
        entity.add(Column.of("version", 0.001));
        entity.add(Column.of("name", "Diana"));
        entity.add(Column.of("options", Arrays.asList(1, 2, 3)));

        List<Column> columns = entity.getColumns();
        Optional<Column> id = entity.find("id");
        entity.remove("options");
```



