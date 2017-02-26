#### BucketManager

O `BucketManager` é responsável por realizar as operações de forma síncrona dentro do banco de dados de chave-valor. Com ele é possível realizar as operações de criação, edição, remação e recuperação dentro desse tipo de banco de dados.

```java
BucketManager bucketManager= null;
KeyValueEntity<String> entity = KeyValueEntity.of("key", 1201);
Set<KeyValueEntity<String>> entities = Collections.singleton(entity);
bucketManager.put("key", "value");
bucketManager.put(entity);
bucketManager.put(entities);
bucketManager.put(entities, Duration.ofHours(2));//two hours TTL
bucketManager.put(entity, Duration.ofHours(2));//two hours TTL
```

###### Remoção e recuperação da sua informação



Em sua estrutura simples, o bucket precisa apenas da chave para recuperar ou remover uma informação no banco de dados.

```java
Optional<Value> value = bucketManager.get("key");
Iterable<Value> values = bucketManager.get(Collections.singletonList("key"));
bucketManager.remove("key");
bucketManager.remove(Collections.singletonList("key"));
```



