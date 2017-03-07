#### ColumnFamilyEntity

The `ColumnFamilyEntity` is an entity to column family database type. It is composed of one or more columns. As a result, the Column is a tuple of name and value.


```java
ColumnFamilyEntity entity = ColumnFamilyEntity.of("columnFamily"); 
entity.add(Column.of("id", Value.of(10L))); 
entity.add(Column.of("version", 0.001)); 
entity.add(Column.of("name", "Diana")); 
entity.add(Column.of("options", Arrays.asList(1, 2, 3))); 

List<Column> columns = entity.getColumns(); 
Optional<Column> id = entity.find("id");
```
