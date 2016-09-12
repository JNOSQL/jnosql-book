#### Criando o seu próprio leitor


O real propósito da interface `Value`, como foi dito, é facilitar a comunicação entre o banco de dados e a aplicação. O Diana, por padrão, suporta os tipos comuns existentes na plataforma Java como os tipos primitivos, Wrappers, a nova API de time, etc. Além desses tipos nativamente suportados também é possível criar os seus próprios convetores de maneira transparente. Para isso ele possui duas interfaces:


* `ValueWriter`: Essa interface representa como uma instância de Value será escrita dentro do banco de dados.
* `ValueReader`: Essa interface representa como o valor será lido para ser lido dentro da aplicação Java. Por padrão, o método `<T> T get(Class<T> clazz)` e suas derivações (*getList*, *getSet*, *getMap*, *getStream*) utilizam essas implementações para realizar essa conversão.

Ambas as interfaces são carregadas a partir do ServiceLoader do Java SE. Assim, para fazer com que o Diana basta seguir o tal padrão. Para facilitar o entendimento, criará um converter para o seguinte tipo.

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

Com o intuito é criar um simples converter foi utilizado a criação dessa simples representação Monetário. Como se sabe que não é uma boa prática reinventar a roda, em sua aplicação utiliza APIs mais maduras como o [moneta](https://github.com/JavaMoney) que é a implementação de referência da money-api, [JSR 354](https://jcp.org/en/jsr/detail?id=354).


O primeiro passo é a criação da classe que realizará a conversão do tipo para o banco de dados, o `ValueWriter`. Ele possui dois métodos:

* `boolean isCompatible(Class clazz)`: Verifica se a implementação suporta a conversão para esse tipo de classe.
* `S write(T object)`: Uma vez definido que a implementação está apta para realizar a conversão, o próximo passo é realizar a conversão de uma instância T para o tipo desejado S.


[code]
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
[code]

Uma vez o valor definido dentro do banco de dados o próximo passo é realizar a leitura dessa informação para a aplicação. Para isso é necessário ter uma especialização do ValueReader. Assim, como o ValueWriter ele possui dois métodos:

boolean isCompatible(Class clazz); Verifica se a implementação está apta para realizar a leitura do tipo desejado.
<T> T read(Class<T> clazz, Object value); Uma vez compatível, o próximo passo é realizar a operação de leitura para a classe algo T a partir do objeto origem.

[code]
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
[code]




Uma vez criado as implementações o próximo passo é cadastrar as implementações de leitura e escrita. Para isso, é necessário criar dois arquivos:

META-INF/services/org.jnosql.diana.api.ValueReader 
META-INF/services/org.jnosql.diana.api.ValueWriter

Cada arquivo terá o caminho e a classe da respectiva implementação, Assim:

O arquivo `org.jnosql.diana.api.ValueReader ` terá o seguinte conteúdo:
my.company.MoneyValueReader

O arquivo `org.jnosql.diana.api.ValueWriter` terá o seguinte conteúdo:
my.company.MoneyValueWriter


[code]
Value value = Value.of("BRL 10.0"); 
Money money = value.get(Money.class); 
List<Money> list = value.getList(Money.class); 
Set<Money> set = value.getSet(Money.class);
[/code]
