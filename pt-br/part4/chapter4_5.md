## Bean Validation

O Artemis tem suporte o uso de bean validation, ele suporta como um plugin que, basicamente, escuta o evento de preEntity e executa o bean validation.



```java
@Entity
public class Person {

    @Key
    @NotNull
    @Column
    private String name;

    @Min(21)
    @NotNull
    @Column
    private Integer age;

    @DecimalMax("100")
    @NotNull
    @Column
    private BigDecimal salary;

    @Size(min = 1, max = 3)
    @NotNull
    @Column
    private List<String> phones;
}
```



