### Value

Essa interface representa o valor que será armazenado no banco de dados, um simples Wrapper. Esse Wrapper tem como principal objetivo de realizar a ponte entre o banco e a aplicação. Por exemplo, ao se utilizar um tipo no qual o banco de dados não suporte, é possível realizar a conversão entre a comunicação e seu banco de dados de maneira transparente. 

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



