## Repositório de Documentos

O repositório de documentos é responsável para realizar a comunicação da entidade para um banco de dados do tipo documentos, ele é subdividido em `DocumentRepository` e `DocumentRepositoryAsync`para trabalhos síncronos e assíncronos respectivamente. 



#### `DocumentRepository`

  
O `DocumentRepository` é responsável pela persistência de uma Entidade em um banco de dados do tipo documento. Ele é composto, basicamente, por três componentes:



* **DocumentEntityConverter**: Responsável por converter da entidade, por exemplo, Person para DocumentEntity.

* **DocumentCollectionManager**: Entidade manager de documentos do Diana.

* **DocumentWorkflow**: Segue o fluxo de persistência durante os métodos de save e update.



