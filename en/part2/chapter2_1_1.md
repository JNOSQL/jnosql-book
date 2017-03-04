#### Make custom Writer and Reader

As mentioned before, the `Value` interface is to storage the cost information into a database. Diana already has support to the Java type such as primitives types, wrappers types, new Java 8 date time. Furthermore, the developer can create custom converter easily and quickly. It has two interfaces:

* `ValueWriter`: This interface represents a `Value` instance to write in a database.
* `ValueReader`: This interface represents how the `Value` will convert to Java application. This interface will use on the `<T> T get(Class<T> clazz)` and `<T> T get(TypeSupplier<T> typeSupplier)`.

Both class implementations load from  Java SE ServiceLoader resource. So, to Diana learn a new type just register on ServiceLoader, e.g., Given a Money type:

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

* `boolean isCompatible(Class clazz)`: Verifica se a implementação suporta a conversão para esse tipo de classe.
* `S write(T object)`: Uma vez definido que a implementação está apta para realizar a conversão, o próximo passo é realizar a conversão de uma instância `T` para o tipo desejado `S`.

```java
public class MoneyValueWriter implements ValueWriter<Money, String> { 

    @Override 
    public boolean isCompatible(Class clazz) { 
        return Money.class.equals(clazz); 
    } 

    @Override 
    public String write(Money object) { 
        return object.toString(); 
    } 
}
```

Uma vez o valor definido dentro do banco de dados o próximo passo é realizar a leitura dessa informação para a aplicação. Para isso é necessário ter uma especialização do ValueReader. Assim, como o `ValueWriter` ele possui dois métodos:

* `boolean isCompatible(Class clazz)`; Verifica se a implementação está apta para realizar a leitura do tipo desejado.
* `<T> T read(Class<T> clazz, Object value)`; Uma vez compatível, o próximo passo é realizar a operação de leitura para a classe algo T a partir do objeto origem.

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

Uma vez criado as implementações o próximo passo é cadastrar as implementações de leitura e escrita. Para isso, é necessário criar dois arquivos:

* `META-INF/services/org.jnosql.diana.api.ValueReader`
* `META-INF/services/org.jnosql.diana.api.ValueWriter`

Cada arquivo terá o caminho e a classe da respectiva implementação, Assim:

O arquivo `org.jnosql.diana.api.ValueReader` terá o seguinte conteúdo:

```
my.company.MoneyValueReader
```

O arquivo `org.jnosql.diana.api.ValueWriter` terá o seguinte conteúdo:

```
my.company.MoneyValueWriter
```

```java
        Value value = Value.of("BRL 10.0");
        Money money = value.get(Money.class);
        List<Money> list = value.get(new TypeReference<List<Money>>() {});
        Set<Money> set = value.get(new TypeReference<Set<Money>>() {});;
```



