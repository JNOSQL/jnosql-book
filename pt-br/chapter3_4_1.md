#### Document Manager

A interação com o banco de dados do tipo documento é dado por duas classes:

* **DocumentCollectionManager**: Para realizar operações no banco de dados de forma síncrona
* **DocumentCollectionManagerAsync**: Para realizar operações no banco de dados de forma assíncrona.



##### **DocumentCollectionManager**



 O `DocumentCollectionManager` é classe que realiza as operações de forma síncrona, com ele é possível realizar a criação, editação, remoção e a recuperação dentro dos bancos de dados do tipo documento.

```java
        DocumentEntity entity = DocumentEntity.of("collection");
        Document diana = Document.of("name", "Diana");
        entity.add(diana);

        List<DocumentEntity> entities = Collections.singletonList(entity);
        DocumentCollectionManager manager = //instance;

        //saves operations
        manager.save(entity);
        manager.save(entity, Duration.ofHours(2L));//saves with 2 hours of TTL
        manager.save(entities, Duration.ofHours(2L));//saves with 2 hours of TTL
        //updates operations
        manager.update(entity);
        manager.update(entities);
```



##### **DocumentCollectionManagerAsync**



 O `DocumentCollectionManagerAsync` é classe que realiza as operações de forma assíncrona, com ele é possível realizar a criação, editação, remoção e a recuperação dentro dos bancos de dados do tipo documento.

```java
        DocumentEntity entity = DocumentEntity.of("collection");
        Document diana = Document.of("name", "Diana");
        entity.add(diana);

        List<DocumentEntity> entities = Collections.singletonList(entity);
         DocumentCollectionManagerAsync managerAsync = null;

        //saves operations
        managerAsync.save(entity);
        managerAsync.save(entity, Duration.ofHours(2L));//saves with 2 hours of TTL
        managerAsync.save(entities, Duration.ofHours(2L));//saves with 2 hours of TTL
        //updates operations
        managerAsync.update(entity);
        managerAsync.update(entities);
```



