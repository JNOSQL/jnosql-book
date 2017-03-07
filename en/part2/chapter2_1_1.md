#### Make custom Writer and Reader

As mentioned before, the `Value` interface is to storage the cost information into a database. Diana already has support to the Java type such as primitives types, wrappers types, new Java 8 date time. Furthermore, the developer can create custom converter easily and quickly. It has two interfaces:

* `ValueWriter`: This interface represents a `Value` instance to write in a database.
* `ValueReader`: This interface represents how the `Value` will convert to Java application. This interface will use on the `<T> T get(Class<T> clazz)` and `<T> T get(TypeSupplier<T> typeSupplier)`.

Both class implementations load from Java SE ServiceLoader resource. So, to Diana learn a new type just register on ServiceLoader, e.g., Given a Money type:

```java
public class Money {

    private final String currency;

    private final BigDecimal value;

    Money(String currency, BigDecimal value) {
        this.currency = currency;
        this.value = value;
    }

    public String getCurrency() {
        return currency;
    }

    public BigDecimal getValue() {
        return value;
    }

    @Override
    public String toString() {
        return currency + ' ' + value;
    }

    public static Money parse(String text) {
        String[] texts = text.split(" ");
        return new Money(texts[0], BigDecimal.valueOf(Double.valueOf(texts[1])));
    }
}
```

Just to be more didactic the book creates a simple money representation. As everyone knows that is not a good practice reinventing the wheel, so in production the Java Developer must use mature Money APIS such as [moneta](https://github.com/JavaMoney) that is the reference implementation of [JSR 354](https://jcp.org/en/jsr/detail?id=354).

The first step is to create the converter to a custom type to a database, the `ValueWriter`. It has two methods:

* `boolean isCompatible(Class clazz)`: Check if the given class has support for this implementation.
* `S write(T object)`: Once the implementation supports the type, the next step converts a `T` instance to `S` type.

```java
public class MoneyValueWriter implements ValueWriter<Money, String> {

    @Override
    public boolean isCompatible(Class clazz) {
        return Money.class.equals(clazz);
    }

    @Override
    public String write(Money money) {
        return money.toString();
    }
}
```

With the `MoneyValueWriter` created and the Money type will save as `String`, then the next step is read information to Java application. As can be seen, a `ValueReader` implementation. This interface has two methods:

* `boolean isCompatible(Class clazz)`; Check if the given class has support for this implementation.
* `<T> T read(Class<T> clazz, Object value)`; Converts to the `T` type from Object instance.

```java
public class MoneyValueReader implements ValueReader {

    @Override
    public boolean isCompatible(Class clazz) {
        return Money.class.equals(clazz);
    }

    @Override
    public <T> T read(Class<T> clazz, Object value) {
        return (T) Money.parse(value.toString());
    }
}
```

After all, the both implementation were done, the last step is to register them into two files:

* `META-INF/services/org.jnosql.diana.api.ValueReader`
* `META-INF/services/org.jnosql.diana.api.ValueWriter`

Each file will have the qualified of this respective implementation:

The file `org.jnosql.diana.api.ValueReader` will have:

```
my.company.MoneyValueReader
```

The file `org.jnosql.diana.api.ValueWriter` will have:

```
my.company.MoneyValueWriter
```

```java
        Value value = Value.of("BRL 10.0");
        Money money = value.get(Money.class);
        List<Money> list = value.get(new TypeReference<List<Money>>() {});
        Set<Money> set = value.get(new TypeReference<Set<Money>>() {});;
```
