## As classes respositórios

Os “maestros” da persistência é dividida em forma síncrona e assíncrona com Repository e RepositoryAsync respectivamente. Esses recursos serão estendidos principalmente para adicionar recursos que existem especificamente em cada driver do diana, por exemplo, live query no OrientDB ou nível de consistência no Cassandra. Com o intuito de facilitar essa extensão existem as seguintes classes:

* AbstractKeyValueRepository

* AbstractColumnRepository

* AbstractColumnRepositoryAsync

* AbstractDocumentRepository

* AbstractDocumentRepositoryAsync

Para exemplificar, será criado uma extensão do ColumnRepository para suportar alguns recursos específicos que tem apenas no Cassandra: O Cassandra Query Language e a persistência com nível de consistência.



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



Assim, herdando a classe AbstractColumnRepository o CassandraColumnRepository terá suporte a todos os recursos já existentes na API do Artemis e pode adicionar os recursos específicos do Cassandra.

