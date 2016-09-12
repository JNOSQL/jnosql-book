### Value


Essa interface representa o valor que será armazenado no banco de dados, um simples Wrapper. Esse Wrapper tem como principal objetivo de realizar a ponte entre o banco e a aplicação. Por exemplo, ao se utilizar um tipo no qual o banco de dados não suporte, é possível realizar a conversão entre a comunicação e seu banco de dados de maneira transparente. Por padrão, o Diana suporta uma implementação default a partir da própria API.

```java
Value value = Value.of(12);
```

Os métodos dentro do Value são:

* `Object get();` Retorna o valor como Object
* `<T> T cast();` Retorna o valor realizando a conversão utilizando o cast, ou seja, isso é um modo não seguro, é preciso saber o formato previamente, para que não ocorra o ClassCastException.
* `<T> T get(Class<T> clazz);` Realiza a conversão do valor para o tipo desejado, essa é a maneira mais segura para realizar a conversão. Caso conversão desejada não seja suportada será lançada uma exceção. Porém, é possível criar os seus próprios leitores.
* `<T> List<T> getList(Class<T> clazz);` Semelhante ao método anterior, realiza a conversão desejado, porém, retorna uma lista. Caso objeto seja uma especialização de Iterable, todos os elementos nele contido serão convertidos, do contrário, retornará uma lista única com o elemento convertido.
* `<T> Set<T> getSet(Class<T> clazz);` Semelhante ao método getList, porém, é retornado um Set, no qual não permite elementos não duplicados.
* `<T> Stream<T> getStream(Class<T> clazz);` Semelhante aos métodos anteiores, porém é retornado um Stream.
* `<K, V> Map<K, V> getMap(Class<K> keyClass, Class<V> valueClass;` Caso o valor contido dentro do Value seja uma instância do java.util.Map ele converterá a chave e o valor para o tipo desejado, do contrário, a operação não será suportada.

```java
        Value value = Value.of(12); 
        String string = value.get(String.class); 
        List<Integer> list = value.getList(Integer.class); 
        Set<Long> set = value.getSet(Long.class); 
        Stream<Integer> stream = value.getStream(Integer.class); 
        Integer integer = value.cast();
```