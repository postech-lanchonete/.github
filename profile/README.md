# PosTech - Lanchonete

<p align="center">
  <img src="https://github.com/postech-lanchonete/.github/blob/main/profile/logo.png?raw=true" />
</p>

<p align="justify">
  O projeto Lanchonete do Bairro tem como objetivo desenvolver um sistema de gerenciamento para uma lanchonete familiar. O sistema será desenvolvido utilizando a arquitetura limpa e seguirá os princípios do <b>Domain-Driven Design</b> (DDD).
</p>
<details>
  <summary>Saber mais sobre o projeto</summary>
  <p align="justify">
  Através desse sistema, os clientes terão acesso a uma interface intuitiva onde poderão realizar pedidos e efetuar pagamentos de forma prática. Será possível montar o combo de lanches com opções de lanche, acompanhamento e bebida. O sistema também permitirá que os clientes acompanhem o progresso do seu pedido, desde a confirmação até a entrega ou retirada.
  </p>
  <p align="justify">
    Além das funcionalidades voltadas para os clientes, o sistema contará com um painel administrativo que permitirá o gerenciamento de clientes, produtos e categorias. O estabelecimento poderá cadastrar novos clientes, gerenciar campanhas promocionais, adicionar, editar e remover produtos, definindo nome, categoria, preço, descrição e imagens. Também será possível acompanhar os pedidos em andamento e verificar o tempo de espera de cada pedido.
  </p>
  <p align="justify">
    O projeto será desenvolvido utilizando a linguagem de programação Java 17 e o framework Spring Boot. Será integrado ao banco de dados MariaDB para armazenar as informações dos clientes, produtos e pedidos. Além disso, o projeto incluirá a documentação do sistema utilizando a linguagem ubíqua (DDD) e a implementação de *endpoints* RESTful para as funcionalidades descritas.
  </p>
  <p align="justify">
    Com o projeto Lanchonete do Bairro, pretendemos criar um sistema eficiente e intuitivo que facilite o processo de pedido e pagamento, proporcionando uma experiência agradável aos clientes e auxiliando o estabelecimento.
  </p>
</details>

## Monolito para microservicos

<p align="center">
  <img src="https://github.com/postech-lanchonete/.github/assets/20681811/2409cb8c-d26b-4ec3-9763-a2a0cdc9d57b" />
</p>

<p align="justify">
  Optamos pela transição do serviço monolítico para cinco microserviços distintos visando vantagens como escalabilidade independente, facilitação de manutenção e evolução, desacoplamento de responsabilidades, resiliência a falhas e a possibilidade de utilizar tecnologias específicas para cada contexto. Essa abordagem permite melhorias na eficiência operacional, agilidade no desenvolvimento e maior flexibilidade para adaptação às demandas específicas de cada serviço.
</p>

| Projeto                   | Cobertura de Código SonarCloud |
|---------------------------|--------------------------------|
| [postech-produtos](https://github.com/postech-lanchonete/postech-produtos) | [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=postech-lanchonete_postech-produtos&metric=coverage)](https://sonarcloud.io/summary/new_code?id=postech-lanchonete_postech-produtos) |
| [postech-clientes](https://github.com/postech-lanchonete/postech-clientes) | [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=postech-lanchonete_postech-clientes&metric=coverage)](https://sonarcloud.io/summary/new_code?id=postech-lanchonete_postech-clientes) |
| [postech-pedidos](https://github.com/postech-lanchonete/postech-pedidos) | [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=postech-lanchonete_postech-pedidos&metric=coverage)](https://sonarcloud.io/summary/new_code?id=postech-lanchonete_postech-pedidos) |
| [postech-pagamento](https://github.com/postech-lanchonete/postech-pagamento) | [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=postech-lanchonete_postech-pagamento&metric=coverage)](https://sonarcloud.io/summary/new_code?id=postech-lanchonete_postech-pagamento) |
| [postech-producao](https://github.com/postech-lanchonete/postech-producao) | [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=postech-lanchonete_postech-producao&metric=coverage)](https://sonarcloud.io/summary/new_code?id=postech-lanchonete_postech-producao) |

## Padrão Saga

<p align="justify">
  Este projeto implementa o padrão Saga para processar o fluxo de pagamento de uma aplicação utilizando o Apache Kafka como gerenciador de mensageria. O padrão Saga é utilizado para garantir a consistência e disponibilidade da aplicação, especialmente em cenários de transações distribuídas.
</p>

<p align="center">
  <img src="https://github.com/postech-lanchonete/.github/assets/20681811/ca72bd9b-d630-48cd-8654-8aeca107682c" />
</p>

### Fluxo de Execução

1. O processo se inicia com o serviço postech-pedido, responsável por receber e processar os pedidos dos clientes.
2. Quando um pedido é recebido, o serviço postech-pedido publica uma mensagem no tópico do Kafka para iniciar o fluxo de pagamento.
3. O serviço de pagamento, ao consumir a mensagem do tópico, processa o pagamento do pedido.
4. Após a conclusão do pagamento com sucesso, o serviço de pagamento publica uma mensagem em outro tópico do Kafka.
5. O serviço postech-pedido consome essa mensagem e, em seguida, envia o pedido para a produção.

### Justificativa da Utilização do Tipo Orquestração

<p align="justify">
  Optei por utilizar o tipo de orquestração no padrão Saga devido à complexidade e dependência entre as etapas do processo de pagamento. A orquestração permite coordenar as diversas operações necessárias para concluir o fluxo de pagamento, garantindo a consistência e o controle do processo como um todo.
</p>

### Justificativa da Utilização do Apache Kafka
A escolha do Apache Kafka como gerenciador de mensageria se baseia em diversos fatores:

1. Escalabilidade e Desempenho: O Apache Kafka é altamente escalável e pode processar milhares de requisições simultâneas, garantindo a disponibilidade e performance da aplicação.
2. Desacoplamento: O uso do Kafka promove um alto nível de desacoplamento entre os serviços, permitindo que operem de forma independente.
3. Tolerância a Falhas: O Kafka oferece resiliência e tolerância a falhas, garantindo a integridade das mensagens mesmo em caso de falhas no sistema.
4. Event Sourcing: O Kafka é adequado para implementar o padrão de Event Sourcing, mantendo um registro de todas as transações e garantindo a consistência do sistema.
5. Integração com Spring Boot: A integração do Kafka com o Spring Boot simplifica o desenvolvimento e implantação de aplicativos Java baseados em microserviços.

## Infraestrutura

A infraestrutura do projeto será implementada utilizando soluções disponíveis na AWS, como EKS, DocumentDB e RDS. O diagrama do fluxo da infraestrutura pode ser visualizado abaixo:

<p align="center">
  <img src="https://github.com/postech-lanchonete/.github/assets/20681811/90f03df1-b30e-4639-b176-d43b7b7ca343?raw=true" />
</p>

<p align="justify">
O Amazon EKS será responsável pelo gerenciamento das APIs, cada uma delas executando em uma instância EC2 dedicada. Além disso, considerando a necessidade de armazenamento de dados em múltiplos bancos, optaremos por diferentes soluções. O serviço DocumentDB será utilizado para armazenar os dados da aplicação de pagamentos, uma vez que a estrutura dessas informações não é crítica para o projeto e não envolve consultas complexas com múltiplas tabelas.
</p>
<p align="justify">
Quanto aos demais projetos, o Amazon RDS foi escolhido devido à sua simplicidade e facilidade de manutenção, proporcionando uma evolução mais ágil das APIs. Optamos por uma única instância de RDS para armazenar os dados, com uma divisão lógica necessária entre os contextos das aplicações. Por fim, o Amazon DynamoDB pode ser necessário para realizar o cache de informações, como tipos de lanches, dados dos clientes, entre outros.
</p>

## Domain-Driven Design (DDD)

Como anteriormente falado, o projeto utilizou DDD para guiar seu desenvolvimento. A seguir estão alguns dos materiais coletados. 

<details>
  <summary>Linguagem Ubíqua</summary>

1. Lanchonete: Estabelecimento que oferece uma variedade de alimentos e bebidas.
2. Cliente: Pessoa que faz um pedido na lanchonete.
3. Pedido: Solicitação de alimentos e/ou bebidas feita por um cliente.
4. Produto: Produtos que compõem um pedido
5. Acompanhamento: Opção adicional selecionada pelo cliente para acompanhar seu lanche.
6. Lanche: Alimento principal do pedido, como hamburguês, pizza, etc.
7. Acompanhamento: Alimento secundário do pedido, como batata frita, salada, etc.
8. Bebida: Opção de bebida selecionada pelo cliente.
9. Sobremesa: Complemento da alimentação.
10. Pagamento: Processo de efetuar o pagamento do pedido.
11. Sistema de Pedido: Tela ou dispositivo no estabelecimento que mostra o status do pedido em diferentes etapas para os clientes e para a equipe da cozinha.
12. Equipe da cozinha: Funcionários responsáveis por preparar os pedidos.
13. Status do Pedido: Indicador do progresso do pedido, dividido em:
14. Recebido: Pedido registrado e aguardando preparação.
15. Em preparação: Pedido em processo de preparação na cozinha.
16. Pronto: Pedido concluído e pronto para retirada.
17. Finalizado: Pedido entregue e finalizado.
18. Entrega: Processo de notificar o cliente quando o pedido está pronto para retirada.
19. Acompanhamento de Pedidos: Funcionalidade que permite acompanhar o status dos pedidos em andamento e estimar o tempo de espera.
20. Balcão de recolha: Local físico onde os pedidos são entregas quando finalizado para a recolha pelo cliente.
</details>

<details>
  <summary>Fluxo de Funcionalidades (Representação Pictográfica)</summary>

<p align="justify">
  Alguns dos fluxos que este Sistema se propõe a resolver são os de realização do pedido e seu pagamento e a preparação e entrega do pedido. Os fluxos foram mapeados como são feitos hoje, sem a implementação do sistema, e como se visualiza após a sua implementação.
</p>

### Preparação e entrega do pedido

<p align="center">
  <img src="profile/fluxo_1_ddd.png" />
</p>

Fluxo 1. Fluxo antigo onde o pedido é recebido pela equipe de cozinha por uma anotação em papel e prepara todos os produtos, sem informar seu status a ninguém.

### Novo fluxo de preparação e entrega do pedido

<p align="center">
  <img src="profile/fluxo_2_ddd.png" />
</p>

Fluxo 2. Novo fluxo proposto onde a equipe de cozinha pode visualizar os pedidos em um sistema de pedidos e à medida que os produtos são feitos, seu status é alterado e o cliente pode acompanhar este status.

### Realização do pedido e seu pagamento

<p align="center">
  <img src="profile/fluxo_3_ddd.png" />
</p>

Fluxo 3. Fluxo antigo onde o cliente depende de um funcionário para realizar o pedido e pagamento. Além disso o pedido só é enviado para a equipe da cozinha por uma ação do funcionário.

### Novo fluxo de realização do pedido e seu pagamento

<p align="center">
  <img src="profile/fluxo_4_ddd.png" />
</p>

Fluxo 4. Fluxo atualizado com nova proposta. Cliente pode interagir diretamente com a interface de seleção de produtos e realizar ele mesmo o pagamento. Além disso, o pedido vai diretamente para o sistema de pedidos assim que o pagamento é realizado.

</details>
