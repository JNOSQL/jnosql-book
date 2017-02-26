#### Document Manager

A interação com o banco de dados do tipo documento é dado por duas classes:

* **DocumentCollectionManager**: Para realizar operações no banco de dados de forma síncrona
* **DocumentCollectionManagerAsync**: Para realizar operações no banco de dados de forma assíncrona.





```java
        DocumentEntity entity = DocumentEntity.of("collection");
        Document diana = Document.of("name", "Diana");
        entity.add(diana);

        List<DocumentEntity> entities = Collections.singletonList(entity);
        DocumentCollectionManager manager = null;

        //saves operations
        manager.save(entity);
        manager.save(entity, Duration.ofHours(2L));//saves with 2 hours of TTL
        manager.save(entities, Duration.ofHours(2L));//saves with 2 hours of TTL
        //updates operations
        manager.update(entity);
        manager.update(entities);
```



