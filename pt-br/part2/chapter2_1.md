### Value

This interface represents the value that will store, that is a wrapper to be a bridge between the database and the application. Eg. If a database does not support a Java type, it may do the conversion with easily. 

```java
Value value = Value.of(12);
```

Os métodos dentro do Value são:

* `Object get();` Retorna o valor como Object

* `<T> T get(Class<T> clazz);` Realiza a conversão do valor para o tipo desejado, essa é a maneira mais segura para realizar a conversão. Caso conversão desejada não seja suportada será lançada uma exceção. Porém, é possível criar os seus próprios leitores.

* `<T> T get(TypeSupplier<T> typeSupplier);` Semelhante ao método anterior, realiza a conversão desejado em estruturas que utilizam generics como List, Set e Stream, por exemplo.

```java
        Value value = Value.of(12);
        String string = value.get(String.class);
        List<Integer> list = value.get(new TypeReference<List<Integer>>() {});
        Set<Long> set = value.get(new TypeReference<Set<Long>>() {});
        Stream<Integer> stream = value.get(new TypeReference<Stream<Integer>>() {});
        Object integer = value.get();
```



