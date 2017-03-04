## The repository class

The "maestro" persistence class has synchronous and asynchronous. These interfaces are extended mainly to add a feature that has a Diana driver, but the Artemis API does not support, eg., the live query at OrientDB, consistency level. To make secure an extension there is template method class.

* AbstractKeyValueRepository

* AbstractColumnRepository

* AbstractColumnRepositoryAsync

* AbstractDocumentRepository

* AbstractDocumentRepositoryAsync

To demonstrate, it will create a `ColumnRepository` extension to supports a Cassandra methods that do exists on diana-driver: Cassandra Query Language and consistency level.



```java
public class CassandraColumnRepository extends AbstractColumnRepository {

    @Inject
    private ColumnEntityConverter converter;
    @Inject
    private CassandraColumnFamilyManager manager;
    @Inject
    private ColumnWorkflow workflow;

    @Override
    protected ColumnEntityConverter getConverter() {
        return converter;
    }

    @Override
    protected ColumnFamilyManager getManager() {
        return manager;
    }

    @Override
    protected ColumnWorkflow getFlow() {
        return workflow;
    }

    public <T> T save(T entity, ConsistencyLevel level) {
        Objects.requireNonNull(entity, "entity is required");
        Objects.requireNonNull(level, "level is required");
        UnaryOperator<ColumnEntity> save = e -> manager.save(e, level);
        return workflow.flow(entity, save);
    }

    public <T> T save(T entity, Duration ttl, ConsistencyLevel level) throws NullPointerException {
        Objects.requireNonNull(entity, "entity is required");
        Objects.requireNonNull(level, "level is required");
        UnaryOperator<ColumnEntity> save = e -> manager.save(e, ttl, level);
        return workflow.flow(entity, save);
    }

    public <T> List<T> cql(String query) throws NullPointerException {
        Objects.requireNonNull(query, "query is required");
        List<ColumnEntity> entities = manager.cql(query);
        return entities.stream().map(converter::toEntity).map(c -> (T) c)
                .collect(Collectors.toList());
    }

    public void delete(ColumnDeleteQuery query, ConsistencyLevel level) throws NullPointerException {
        manager.delete(query, level);
    }

}

```

To conclude, extending the AbstractColumnRepository the CassandraColumnRepository will have support to the methods on Artemis interface and also append the Cassandra resources.

