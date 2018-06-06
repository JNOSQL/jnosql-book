# chapter2\_3\_1

O `ColumnFamilyEntity` é a representação da entidade que será persistida dentro de um banco de dados família de coluna. Ela é composta por uma ou mais colunas, a coluna por sua vez é um tupla composta por um valor, representado por Value, e o seu respectivo nome.

```java
ColumnFamilyEntity entity = ColumnFamilyEntity.of("columnFamily"); 
entity.add(Column.of("id", Value.of(10L))); 
entity.add(Column.of("version", 0.001)); 
entity.add(Column.of("name", "Diana")); 
entity.add(Column.of("options", Arrays.asList(1, 2, 3))); 

List<Column> columns = entity.getColumns(); 
Optional<Column> id = entity.find("id");
```

