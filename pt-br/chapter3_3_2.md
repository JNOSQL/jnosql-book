#### DocumentCollectionEntity

O `DocumentCollectionEntity` é a representação da entidade que será presentado dentro de um banco de dados do tipo documentos. Ela é composta por um ou mais documentos, o documento por sua vez, assim como a coluna, é composta por um valor, representado Value, e o seu respectivo nome.

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



