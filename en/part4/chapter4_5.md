## Bean Validation

Artemis has support to use Bean Validation (BV), which supports a plugin that, basically, listens an event from preEntity and executes the BV. The artemis-validation has the support for BV, but does not implement it, thus, is necessary to add either the API and an implementation of BV in the project.

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


In case of a validation problem in the project, an ArtemisValidationException will be thrown.


```java
 Person person = Person.builder()
                .withAge(10)
                .withName("Ada")
                .withSalary(BigDecimal.ONE)
                .withPhones(singletonList("123131231"))
                .build();
repository.save(person);//throws an ArtemisValidationException
```
