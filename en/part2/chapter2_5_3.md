# Bucket Manager Factory

The factory classes have the duty to create the bucket manager.

```java
BucketManagerFactory bucketManager= //instance
BucketManager bucket = bucketManager.getBucketManager("bucket");
```

Beyond the BucketManager, some databases have support for particular structure represented in the Java world such as `List`, `Set`, `Queue` e `Map`.

```java
List<String> list = bucketManager.getList("list", String.class);
Set<String> set = bucketManager.getSet("set", String.class);
Queue<String> queue = bucketManager.getQueue("queue", String.class);
Map<String, String> map = bucketManager.getMap("map", String.class, String.class);
```

These methods may return an `UnsupportedoperationException` if the database does not support any of structures.

