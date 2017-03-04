#### KeyValueEntity

O `KeyValueEntity` é a estrutura mais simples, uma vez que ele representa uma tupla, uma chave para o seu respectivo valor. Sendo que o tipo não se restringe apenas para String. Assim, como os outros ele tem métodos para acessar os métodos do `Value`.

```java
KeyValueEntity<String> entity = KeyValueEntity.of("key", Value.of(123));
KeyValueEntity<Integer> entity2 = KeyValueEntity.of(12, "Text");
String key = entity.getKey();
Value value = entity.getValue();
Integer integer = entity.get(Integer.class);
```
