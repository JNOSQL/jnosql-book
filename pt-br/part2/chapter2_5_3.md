# Bucket Manager Factory

O Bucket Manager Factory é a classe responsável pela criação do BucketManager.

```java
BucketManagerFactory bucketManager= //instance
BucketManager bucket = bucketManager.getBucketManager("bucket");
```

Além da criação do BucketManager, dentro dos bancos chave valor é possível também a crianção de estruturas de dados especiais que serão representados no mundo Java com `List`, `Set`, `Queue` e `Map`.

```java
List<String> list = bucketManager.getList("list", String.class);
Set<String> set = bucketManager.getSet("set", String.class);
Queue<String> queue = bucketManager.getQueue("queue", String.class);
Map<String, String> map = bucketManager.getMap("map", String.class, String.class);
```

Vale salientar que todas essas operações de que retornam estrutura de dados especiais podem retornar um `UnsupportedoperationException`, caso o banco de dados não suporte um ou mais tipos de dados.

