# Value

This interface represents the value that will store, that is a wrapper to be a bridge between the database and the application. Eg. If a database does not support a Java type, it may do the conversion with easily.

```java
Value value = Value.of(12);
```

The Value interface has the methods:

* `Object get();` Returns the value as Object
* `<T> T get(Class<T> clazz);` Does the conversion process to the required type that is the safer way to do it. If the type required doesn't have support it will throw an exception, although, the API allows to create custom converters.
* `<T> T get(TypeSupplier<T> typeSupplier);` Similar to the previous method, it does the conversion process but using a structure that uses generics such as List, Map, Stream and Set.

```java
        Value value = Value.of(12);
        String string = value.get(String.class);
        List<Integer> list = value.get(new TypeReference<List<Integer>>() {});
        Set<Long> set = value.get(new TypeReference<Set<Long>>() {});
        Stream<Integer> stream = value.get(new TypeReference<Stream<Integer>>() {});
        Object integer = value.get();
```

