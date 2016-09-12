#### KeyValueEntity

O `KeyValueEntity` é a estrutura mais simples, uma vez que ele representa uma tupla, uma chave para o seu respectivo valor. Sendo que o tipo não se restringe apenas para String.

[code]
KeyValueEntity<String> entity = KeyValueEntity.of("key", Value.of(123)); 
KeyValueEntity<Integer> entity2 = KeyValueEntity.of(12, "Text");
[code]