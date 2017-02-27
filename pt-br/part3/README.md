## Diversidade nos bancos não relacionaisDiversidade nos Bancos não relacionais





Dentro dos bancos não relacionais além das divisões dos tipos, já mencionado anteriormente. É muito comum que cada banco tenha comportamentos específicos e único, que muitas vezes, esse comportamento específico é o que faz com que escolha um banco dentro outro banco.

Uma mudança entre tipo de banco de dados, por exemplo, documentos para famílias de colunas exigirá algumas mudanças, dentre elas, a modelagem dos seus objetos, uma vez que cada tipo tem comportamento específico para resolver propósitos específicos. Porém, mesmo olhando entre os bancos do mesmo tipo, família de coluna, por exemplo, Cassandra e Hbase cada provedor tem alguns comportamentos específicos \(Cassandra Query Language, nível de consistência, dentre outros que apenas o Cassandra possuí\).



Com esse intuito a API do Diana tem como foco também permitir a diversidade nos bancos de dados não relacionais. As classes de configurações, fábricas de entidades retornam classes que especialistas, ou seja, eles tem métodos mínimos, mas não um limite de métodos ou mesmo uma quantidade que essas classes especialistas podem ter.





```java
public interface ColumnFamilyManagerFactory<SYNC extends ColumnFamilyManager> extends AutoCloseable {
SYNC get(String database);
}
```



Uma implementação do `ColumnFamilyManagerFactory` pode retornar uma classe desde que ela implemente `ColumnFamilyManager`.



Por exemplo, utilizando recursos específicos do Cassandra.



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









