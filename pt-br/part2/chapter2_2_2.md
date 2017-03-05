#### Column

O Column, é a menor parte de uma entidade de uma entidade de família de coluna. Cada coluna possui uma tupla em que a chave é o nome do documento e o valor é a informação representado pela classe `Value`.

```java
        Column document = Column.of("name", "value");
        Value value = document.getValue();
        String name = document.getName();
```

Com essa interface também é possível ter uma coluna dentro de outra coluna.

```java
Column subColumn = Column.of("subColumn", column);
```

A forma de armazenar essa informação, em subcolunas, dependerá da implementação de cada driver, assim como toda a informação.

Para facilitar o acesso da informação, o Column, possui alias para os métodos do `Value`, ou seja, é possível realizar a conversão do valor diretamente na _interface_ `Document`.

```java
Column age = Column.of("age", 29);
String ageString = age.get(String.class);
List<Integer> ages = age.get(new TypeReference<List<Integer>>() {});
Object ageObject = age.get();
```





