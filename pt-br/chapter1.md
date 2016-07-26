## A principal ideia atrás da API



  Uma vez discutido da importância da padronização das API dos bancos não relacionais, o próximo passo é discutir mais detalhes sobre a API. Porém, para facilitar a explicação da solução da nova API, primeiro é importante falar das camadas do software seja camada física ou lógica. Essas camadas facilitam a organização, manutenção além de dividir as responsabilidades nos softwares, principalmente os mais complexos. A proposta dessa nova API seria responsável por realizar a comunicação entre a camada lógica e a de dados, para isso, será criado duas novas API uma para comunicação entre o banco e outra responsável pela alta abstração na aplicação Java.


![Camada Física](../images/01.png)


  No mundo de software é muito comum que a aplicação tenha camadas, sejam elas lógicas ou físicas. Aplicações de multi-camada física é muito comum, principalmente com três: 

* **Camada de apresentação**: A sua principal responsabilidade é traduzir as atividades de uma forma que o usuário possa entender.
* **Camada lógica**: A camada lógica é onde fica localizada toda a lógica de negócio e processamento, condições, salva informação, essa a camada que move e processa informações entre as camadas.
* **Camada de dados**: Essa camada é responsável por armazenar e recuperar as informações dentro de um banco de dados ou sistema de arquivo.


 Falando precisamente da camada física, tier, lógica, para separar as responsabilidades, existem as camadas, layer, lógicas. Essas camadas contêm a ponte entre a camada física de apresentarão e a camada de dados além da lógica de negócio. Indiferente do seu padrão de arquitetura (MVC, HMVC, PAC, MVA, MVP, MVVM) eles possuem no mínimo quatro camadas:
 
* **Camada de aplicação**: A ponte para a camada física de apresentação, por exemplo, responsável por transformar o objeto em JSON ou XML.
* **Camada de serviço**: A camada de serviço, a depender da tecnologia utilizada pode ser um Controller ou um Resource.
* **Camada de negócio**: Local onde contém toda a lógica de negócio e o modelo.
* **Camada de persistência**: A ponte entre a camada física de acesso aos dados e a camada física lógica.
