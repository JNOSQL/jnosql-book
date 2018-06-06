# chapter2\_3\_3

The `KeyValueEntity` is the simplest structure; it has a tuple, a key-value structure. As the previous entity, it has direct access to information using alias method to `Value`

```java
KeyValueEntity<String> entity = KeyValueEntity.of("key", Value.of(123));
KeyValueEntity<Integer> entity2 = KeyValueEntity.of(12, "Text");
String key = entity.getKey();
Value value = entity.getValue();
Integer integer = entity.get(Integer.class);
```

