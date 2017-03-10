### Classes repositórios

As classes repositórios têm como principal objetivo converter a classe entidade, por exemplo, Person para o Diana, a API de nível de comunicação. Essas classes basicamente, são compostas de três componentes:

* **Converter**: Responsável por realizar conversão da entidade para a API de nível de comunicação

* **EntityManager**: Como falado anteriormente, o entity manager é a classe responsável por realizar toda a operação de inserção, edição, remoção e recuperação de dados a partir do Diana.

* **Workflow**: Ao salvar um objeto o Artemis envia 4 eventos, conforme mostra a figura abaixo, sendo que todo esse processo pode ser interceptado com o recurso oriundo do próprio CDI. Esses eventos são interessantes, por exemplo, para validar a informação da entidade antes dela ir para o banco de dados.

![](../../images/integration-artemis.png)

Os seis eventos são:

1. **firePreEntity**: O objeto recebido pelo Artemis
2. **firePreEntityDataBaseType**: Semelhante ao anterior, porém, específico para o tipo de banco de dados, ou seja, cada banco terão seu tipo específico de evento.
3. **firePreAPI**: o objeto convertido numa entidade de comunicação
4. **firePostAPI**: A entidade de comunicação enviada como resposta do banco de dados
5. **firePostEntity**: A entidade bean convertida oriunda do firePostAPIAs classes repositórios têm como principal objetivo converter a classe entidade, por exemplo, Person para o Diana, a API de nível de comunicação.
6. **firePostEntityDataBaseType**: Semelhante ao anterior, porém, específico para o tipo de banco de dados, ou seja, cada banco terão seu tipo específico de evento.



