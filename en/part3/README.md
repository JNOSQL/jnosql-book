# The diversity on NoSQL databases

On NoSQL world beyond the several types, it's trivial a particular database has features that do exist on this provider.

When there is a change among the types, column family, and document collection, there is a considerable change. Notably, with there a switch to the same kind such as column family to column family, e.g., Cassandra to HBase, there is the same problem once Cassandra has featured such as Cassandra query language and consistency level.

The Diana allows looking the variety on NoSQL database. The configurations classes, and entity factory return specialist class from a provider.

```java
public interface ColumnFamilyManagerFactory<SYNC extends ColumnFamilyManager> extends AutoCloseable {
SYNC get(String database);
}
```

A `ColumnFamilyManagerFactory` return a class the implements `ColumnFamilyManager`.

E.g: Using a particular resource from Cassandra driver.

```java
CassandraConfiguration condition = new CassandraConfiguration();
try(CassandraDocumentEntityManagerFactory managerFactory = condition.get()) {
    CassandraColumnFamilyManager columnEntityManager = managerFactory.get(KEY_SPACE);
    ColumnEntity entity = ColumnEntity.of(COLUMN_FAMILY);
    Column id = Column.of("id", 10L);
    entity.add(id);
    entity.add(Column.of("version", 0.001));
    entity.add(Column.of("name", "Diana"));
    entity.add(Column.of("options", Arrays.asList(1, 2, 3)));
    columnEntityManager.save(entity);
    //common implementation
    ColumnQuery query = ColumnQuery.of(COLUMN_FAMILY);
    query.and(ColumnCondition.eq(id));
    Optional<ColumnEntity> result = columnEntityManager.singleResult(query);
    //cassandra implementation
    columnEntityManager.save(entity, ConsistencyLevel.THREE);
    List<ColumnEntity> entities = columnEntityManager.cql("select * from newKeySpace.newColumnFamily");
    System.out.println(entities);
}
```

