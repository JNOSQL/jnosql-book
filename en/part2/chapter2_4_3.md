#### BucketManager

The `BucketManager`is the class who save the KeyValueEntity on the synchronous way on a key-value database.

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

###### 

##### Removing and retrieve information from a key-value database

##### 

With a simple structure, the bucket needs a key to both retrieve and delete information from the database.

```java
Optional<Value> value = bucketManager.get("key");
Iterable<Value> values = bucketManager.get(Collections.singletonList("key"));
bucketManager.remove("key");
bucketManager.remove(Collections.singletonList("key"));
```



