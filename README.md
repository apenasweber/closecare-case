# Design Doc: Sistema de Gerenciamento de Documentos Corporativos  
  
## 1. Visão Geral  
  
A arquitetura proposta para o Sistema de Gerenciamento de Documentos Corporativos é baseada em microserviços, implementados em Python usando FastAPI. O sistema é composto por três microserviços principais: Document Service, Employee Service e Permission Service, cada um lidando com um aspecto específico do sistema. Além desses, um serviço BFF (Backend for Frontend) é utilizado para integrar as respostas dos outros microserviços e fornecer uma interface para o frontend, desenvolvido em React. Foquei em utilizar ferramentas opensource.  
  
1. O usuário interage com a interface do usuário desenvolvida em React (Front-end).  
2. O BFF Service (FastAPI) recebe as solicitações do usuário e as roteia para os serviços adequados.  
3. O Document Service (FastAPI) gerencia os documentos corporativos, permitindo a criação, atualização, exclusão e busca de documentos. Ele utiliza um banco de dados não-relacional (DynamoDB) para armazenar os dados dos documentos.  
4. O Employee Service (FastAPI) gerencia os funcionários da empresa, permitindo a criação, atualização, exclusão e busca de funcionários. Ele utiliza um banco de dados PostgreSQL (AWS RDS) para armazenar os dados dos funcionários.  
5. O Permission Service (FastAPI) gerencia as permissões dos usuários, determinando quais funcionários ou grupos de funcionários um usuário pode acessar. Ele utiliza um banco de dados PostgreSQL (AWS RDS) para armazenar as informações de permissão.  
6. A autenticação e autorização são realizadas através de JSON Web Tokens (JWT).  
7. A comunicação entre os microserviços é feita através de requisições HTTP/REST.  
8. O Kafka é utilizado para a comunicação assíncrona e o processamento de eventos. Ele permite a troca de eventos em tempo real entre os microserviços.  
9. O Prometheus coleta métricas de desempenho do aplicativo e da infraestrutura.  
10. O Grafana é utilizado para visualizar e analisar as métricas coletadas pelo Prometheus, fornecendo painéis personalizados de monitoramento e análise.  
11. O Jenkins é responsável pela automação do processo de integração contínua e entrega contínua (CI/CD).  
12. O AWS CloudWatch é utilizado para gerenciar e analisar logs, coletar métricas e definir alarmes.  
13. O sistema é escalável, modular e resiliente, permitindo adicionar, remover ou atualizar partes do sistema sem afetar o restante do sistema.  
  
## 2. Diagrama de Arquitetura  
  
![architecture](diagramdesigndoc.png)
  
  
  
## 3. Escolha das Tecnologias  
  
- **Microserviços (FastAPI)**: A arquitetura de microserviços foi escolhida por sua modularidade, escalabilidade e facilidade de manutenção. Cada serviço pode ser desenvolvido, implementado e escalado independentemente.  
- **Banco de dados SQL (PostgreSQL no Amazon RDS)**: A estrutura relacional de SQL é adequada para os dados e consultas complexas deste projeto. O PostgreSQL foi escolhido por sua robustez, extensibilidade e conformidade com padrões.  
- **Banco de dados NoSQL (AWS DynamoDB)**: A estrutura não-relacional do Dynamo é adequada para os dados que possivelmente tendem a mudar e precisam de uma estrutura mais flexível.  
- **React para o Frontend**: Permite a criação de componentes reutilizáveis, o que melhora a eficiência do desenvolvimento e a manutenção do código.  
- **Docker e Amazon ECS com Fargate**: Optei por contêinerizar os serviços com Docker para facilitar a portabilidade, escalabilidade e isolamento. O Fargate nos ajudará a abstrair a complexidade de infraestrutura, e é uma escolha estratégica que nos permite focar mais na construção e no desenvolvimento dos serviços, enquanto a infraestrutura é gerenciada de forma automatizada. Essa abordagem nos proporciona maior flexibilidade para lidar com o aumento de demanda, garantindo que nossos serviços sejam executados de maneira eficiente e escalável.  
- **Jenkins para CI/CD**: Jenkins é uma ferramenta open-source robusta e versátil para a automação de pipelines de CI/CD. Podemos inserir, por exemplo, um CI que avalie testes, cobertura, verifique com safety e bandit brechas de segurança, além de gates de qualidade como linters(black, isort,pep8) e sonarqube.  
- **Prometheus, Grafana e AWS CloudWatch para Monitoramento e Logs**: Ferramentas robustas para monitoramento e visualização de métricas.  
  
## 4. Detalhes da Arquitetura e Estrutura de Diretórios  
  
- **Frontend (React)**: O React é usado para criar a interface do usuário, que interage com o serviço BFF.  
- **BFF Service (FastAPI)**: Este serviço se comunica com todos os outros serviços para buscar os dados necessários e fornecê-los ao frontend de uma maneira que este possa facilmente consumir.  
- **Document Service (FastAPI)**: Este serviço gerencia os documentos corporativos. Ele é responsável por criar, atualizar, deletar e buscar documentos.  
- **Employee Service (FastAPI)**: Este serviço gerencia os funcionários da empresa. Ele é responsável por criar, atualizar, deletar e buscar funcionários.  
- **Permission Service (FastAPI)**: Este serviço gerencia as permissões dos usuários. Ele é responsável por determinar quais funcionários ou grupos de funcionários um usuário pode acessar.  
  
Cada serviço possui uma estrutura de diretórios semelhante a seguinte:  
  
/service_name  
/app  
/routes  
/models  
/controllers  
/tests  
/db  
main.py  
README.md  
  
### Endpoints  
  
**1. Document Service**  
Este serviço seria responsável por gerenciar todos os documentos corporativos. Alguns exemplos de endpoints poderiam ser:  
  
- `POST /documents`: Cria um novo documento.  
- `GET /documents/{id}`: Recupera informações de um documento específico.  
- `PUT /documents/{id}`: Atualiza informações de um documento específico.  
- `DELETE /documents/{id}`: Remove um documento específico.  
- `GET /documents`: Lista todos os documentos disponíveis.  
  
**2. Employee Service**  
Este serviço seria responsável por gerenciar todos os funcionários da empresa. Alguns exemplos de endpoints poderiam ser:  
  
- `POST /employees`: Cria um novo funcionário.  
- `GET /employees/{id}`: Recupera informações de um funcionário específico.  
- `PUT /employees/{id}`: Atualiza informações de um funcionário específico.  
- `DELETE /employees/{id}`: Remove um funcionário específico.  
- `GET /employees`: Lista todos os funcionários.  
  
**3. Permission Service**  
Este serviço seria responsável por gerenciar todas as permissões do sistema. Alguns exemplos de endpoints poderiam ser:  
  
- `POST /permissions`: Cria uma nova permissão.  
- `GET /permissions/{id}`: Recupera informações de uma permissão específica.  
- `PUT /permissions/{id}`: Atualiza informações de uma permissão específica.  
- `DELETE /permissions/{id}`: Remove uma permissão específica.  
- `GET /permissions`: Lista todas as permissões.  
  
**Autenticação e Autorização**  
  
A autenticação e autorização seriam manipuladas usando JSON Web Tokens (JWT). Quando um usuário se autentica com sucesso (talvez por meio de um serviço de autenticação separado ou por um endpoint no Employee Service), ele recebe um token JWT. Este token seria incluído nas requisições subsequentes ao BFF ou aos microserviços. Cada serviço então verificaria a validade do token e usaria as informações nele para determinar quais recursos o usuário tem permissão para acessar.  
  
**Comunicação entre Microserviços**  
  
A comunicação entre os microserviços seria realizada principalmente via HTTP/REST. Por exemplo, o BFF poderia fazer uma solicitação GET ao Document Service para obter uma lista de documentos, e então fazer uma solicitação GET ao Permission Service para verificar quais desses documentos o usuário atual tem permissão para ver.  
  
Também poderíamos considerar o uso de um broker de mensagens (como RabbitMQ ou Kafka) para comunicações assíncronas ou para desacoplar os serviços em cenários onde isso faz sentido. Por exemplo, poderíamos usar um broker de mensagens para implementar um padrão de publicação/assinatura, onde o Document Service publica uma mensagem sempre que um novo documento é criado, e o Permission Service assina essas mensagens para que ele possa atualizar automaticamente as permissões correspondentes.  
  
## 5. Banco de Dados  
  
Cada serviço tem seu próprio banco de dados PostgreSQL hospedado no Amazon RDS, com exceção do Documente Service que utiliza DynamoDB da AWS. Isto permite que cada serviço seja responsável por sua própria persistência de dados, o que é uma característica importante de uma arquitetura de microserviços.  
  
## 6. Automação e Monitoramento  
  
Usamos Jenkins para CI/CD, o que nos permite automatizar o processo de teste, construção e implementação de nossos serviços. Para monitoramento e coleta de logs, usamos Prometheus, Grafana e AWS CloudWatch. Estas ferramentas nos permitem monitorar a saúde de nossos serviços e obter insights sobre seu desempenho.  
  
## 7. Logs e Monitoramento  
  
Para o gerenciamento e análise de logs, adotamos o uso de AWS CloudWatch, Prometheus e Grafana, onde cada ferramenta é responsável por capturar informações específicas:  
  
- **AWS CloudWatch**: Esta ferramenta é usada para coletar e acompanhar métricas, coletar e monitorar arquivos de log, definir alarmes e automaticamente reagir a alterações na AWS. Com o CloudWatch, podemos captar os seguintes tipos de dados:  
  
- **Logs de Aplicação**: Os logs de cada microserviço são enviados para o CloudWatch. Isso inclui logs de erros, informações, avisos e debug. Isso nos ajuda a entender o comportamento do aplicativo e resolver problemas.  
  
- **Logs de Auditoria**: O CloudWatch captura logs de atividades de usuário. Isso é crucial para a segurança e conformidade, e pode ser usado para detectar atividades suspeitas.  
  
- **Logs de Infraestrutura**: Logs dos serviços AWS que suportam o aplicativo, como Amazon RDS, ECS e o próprio CloudWatch. Isso inclui métricas como CPU, memória, rede e disco.  
  
- **Logs de Acesso**: Logs dos pedidos feitos para o aplicativo, incluindo os endereços IP do cliente, URLs de solicitação, respostas do servidor, etc.  
  
- **Prometheus**: Esta ferramenta de monitoramento e alerta de código aberto é usada para coletar métricas de desempenho do aplicativo e da infraestrutura. O Prometheus nos permite captar:  
  
- Métricas de desempenho do aplicativo, como latência, taxa de erros, taxa de solicitações, etc. Isso nos ajudará a entender como o aplicativo está se comportando e permitirá a identificação de gargalos.  
  
- Métricas de infraestrutura, como uso de CPU, memória, disco e rede dos contêineres e instâncias de banco de dados.  
  
- **Grafana**: O Grafana é uma plataforma de análise e visualização de métricas usada para visualizar as métricas coletadas pelo Prometheus. Com o Grafana, podemos criar painéis personalizados que exibem métricas importantes para monitoramento em tempo real e análise histórica.  
  
  
A combinação dessas ferramentas nos permite monitorar todos os aspectos do aplicativo e da infraestrutura, detectar problemas precocemente e analisar incidentes após o fato.  
  
## 8. Documentação e Testes  
  
Utilizaremos o Notion para criar a documentação do projeto. Para os testes automatizados, usamos pytest, integrado ao pipeline Jenkins, além do uso de locust para os testes de carga e mutmut para os testes de mutação que avaliariam não apenas a quantidade de nossos testes mas também a qualidade.  
  
## 9. Comunicação Assíncrona com Kafka  
  
Para comunicação assíncrona e processamento de eventos, optei por incorporar o Apache Kafka em nossa arquitetura. O Kafka é uma plataforma de streaming distribuída que oferece recursos de mensagens e publicação/assinatura, permitindo a troca de eventos entre os microserviços de maneira escalável e confiável.  
  
No contexto do nosso sistema de gerenciamento de documentos corporativos, podemos considerar os seguintes casos de uso para o Kafka:  
  
1. **Eventos de Criação/Atualização de Documentos**: Quando um novo documento é criado ou atualizado no Document Service, um evento pode ser publicado no Kafka. Outros serviços, como o Permission Service, podem se inscrever nesses eventos e atualizar suas permissões correspondentes.  
  
2. **Notificações em Tempo Real**: O Kafka pode ser usado para enviar notificações em tempo real para os usuários ou outros sistemas quando certos eventos ocorrerem. Por exemplo, quando um documento é compartilhado com um usuário, um evento de notificação pode ser enviado através do Kafka.  
  
Integrando o Apache Kafka em nossa arquitetura, podemos aproveitar os benefícios da comunicação assíncrona e processamento de eventos, melhorando a escalabilidade, a flexibilidade e a eficiência do sistema de gerenciamento de documentos corporativos.  
  
## 10. Conclusão  
  
Esta arquitetura é projetada para ser modular, escalável e resiliente. Ao usar microserviços, podemos facilmente adicionar, remover ou atualizar partes do sistema sem afetar o restante do sistema. A escolha do DynamoDB, PostgreSQL para banco de dados e React para o frontend nos permite lidar com consultas complexas e oferecer uma experiência de usuário rica, reaproveitável e flexiva. Finalmente, o uso de ferramentas de automação e monitoramento como Jenkins, Prometheus, Grafana e AWS CloudWatch garante que podemos manter o sistema funcionando de maneira eficiente e confiável.
