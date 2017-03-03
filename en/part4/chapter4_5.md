## Bean Validation

O Artemis tem suporte o uso de bean validation, ele suporta como um plugin que, basicamente, escuta o evento de preEntity e executa o bean validation. O artemis-validation tem suporte ao bean validation, mas ele não o implementa, em outras palavras, é necessário adicionar tanto a API quanto a uma implementação do bean validation no projeto.

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

  
Caso seja encontrado um problema na validação no projeto ele lançará uma exceção do tipo ArtemisValidationException.



```java
 Person person = Person.builder()
                .withAge(10)
                .withName("Ada")
                .withSalary(BigDecimal.ONE)
                .withPhones(singletonList("123131231"))
                .build();
repository.save(person);//throws an ArtemisValidationException 
```



