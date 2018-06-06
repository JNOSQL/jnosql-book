# chapter2\_6

As classes de configurações são as responsáveis pela criação das fábricas de Manager. É dentro dessa classe que ficará toda a configuração inicial para a conexão com o banco de dados.

Uma vez que as informações a e comunicação entre os bancos variam bastante, P2P ou master/slave número de clientes necessários, query inicial, configuração via thrift ou http dentre outras possíveis configurações. O número de métodos e configurações necessárias variarão bastante, porém, eles terão um método responsável pela criação do Manager Factory.

Uma recomendação para os provedores de banco de dados é que essas informações de configuração sejam totalmente ou parcialmente realizadas também via um arquivo properties dentro do resource. Com o formato: diana-provider.properties

