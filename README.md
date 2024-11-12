
# Documento Técnico Oficial: Arquitetura Integrada de Aplicação e Infraestrutura da Dra. Jô, Assistente Virtual da SoluBio

## 1. Introdução
Este documento detalha a arquitetura integrada da aplicação e da infraestrutura para a Dra. Jô, assistente virtual da SoluBio. A Dra. Jô foi desenvolvida para oferecer suporte ao cliente, consultar informações no sistema SAP, e fornecer uma experiência de atendimento escalável, segura e eficiente. A arquitetura está hospedada na AWS e foi projetada para garantir alta disponibilidade, escalabilidade e conformidade com as regulamentações de segurança e privacidade (LGPD).

## 2. Objetivo e Escopo
O objetivo da Dra. Jô é oferecer suporte rápido e eficiente aos clientes da SoluBio, permitindo o acesso a informações específicas no SAP. O chatbot atende às áreas de **Serviços Financeiros**, **Suporte da Eficiência Agronômica**, **Suporte da Qualidade Onfarm**, **Suporte da Engenharia**, e **Suporte Comercial**, fornecendo respostas diretas para consultas frequentes e informações relevantes para cada setor.

## 3. Escolha do Tipo de Chatbot
A Dra. Jô é um chatbot baseado em IA/NLP, utilizando o modelo LLM da OpenAI para compreensão de linguagem natural. Esse tipo de modelo é ideal para lidar com interações complexas e fornecer respostas precisas, permitindo que o chatbot compreenda as perguntas dos clientes e acesse informações específicas no SAP em tempo real.

## 4. Arquitetura da Aplicação

### 4.1 Componentes da Aplicação
A arquitetura da aplicação está organizada em camadas, cada uma com funções específicas para atender o fluxo de atendimento ao cliente:

- **Interface de Usuário (Front-end)**: O front-end é acessado por meio de um widget de chat no site da SoluBio, além de integração com plataformas de mensagem como WhatsApp. Essa camada permite que os clientes interajam com a Dra. Jô.
  
- **Camada de Roteamento e Intenções**: Roteia o cliente para a área de atendimento adequada, com base em palavras-chave ou opções selecionadas, mapeando intenções específicas para consultas e ações de cada setor (ex.: “Consulta e Geração de Boletos” em Serviços Financeiros).

- **Motor de Processamento de Linguagem Natural (NLP)**: A Dra. Jô utiliza o LLM da OpenAI, configurado para capturar intenções, interpretar o significado das mensagens dos usuários e fornecer respostas precisas e contextualizadas.

- **Back-end em Python**: Desenvolvido em Python com frameworks como Flask ou FastAPI, o back-end gerencia a lógica de negócios e a integração com o sistema SAP.

- **Banco de Dados**: Amazon DynamoDB é usado para armazenar logs de interações e históricos de consultas. Para dados relacionais, pode-se utilizar o Amazon RDS (PostgreSQL).

### 4.2 Fluxo de Interação na Aplicação
1. **Início da Conversa**: O usuário inicia o chat e escolhe a área de atendimento ou o chatbot identifica automaticamente a intenção.
2. **Processamento de Intenções**: O motor de NLP identifica a intenção e direciona o fluxo de diálogo com base na área de suporte escolhida.
3. **Consulta ao SAP**: Se necessário, o back-end realiza uma chamada ao sistema SAP via AWS API Gateway e Lambda.
4. **Resposta ao Usuário**: O chatbot responde ao cliente com as informações solicitadas ou realiza a ação desejada.

## 5. Arquitetura de Infraestrutura (AWS)

### 5.1 Componentes da Infraestrutura
A infraestrutura foi desenvolvida na AWS para suportar a aplicação da Dra. Jô de maneira escalável e segura:

- **AWS Lambda**: Permite execução serverless de funções em Python para processamento do back-end, incluindo o fluxo de intenção e as integrações com o SAP.
  
- **Amazon API Gateway**: Facilita a comunicação entre o front-end e o back-end, gerenciando autenticação e oferecendo controle seguro para APIs externas, como as consultas ao SAP.

- **Amazon DynamoDB**: Banco de dados NoSQL para armazenar dados de interações e logs de conversa, proporcionando baixa latência e alta escalabilidade.

- **Amazon RDS (opcional)**: Banco de dados relacional para armazenar dados estruturados, como informações dos clientes, quando necessário.

- **Amazon S3**: Usado para armazenamento de logs e backups da aplicação, garantindo persistência e conformidade com políticas de retenção de dados.

- **Amazon CloudWatch**: Monitora e registra logs de atividades, permitindo auditoria e resposta rápida a incidentes.

- **AWS IAM e VPC**: IAM gerencia controle de acesso a serviços e recursos na AWS, enquanto VPC assegura isolamento de rede para proteger comunicações internas.

### 5.2 Fluxo de Dados na Infraestrutura
1. **Entrada do Usuário**: A mensagem do cliente é recebida no front-end e direcionada para o API Gateway.
2. **Processamento no AWS Lambda**: O API Gateway encaminha a solicitação para uma função Lambda, que executa a lógica de negócios em Python.
3. **Consulta a Dados**: O Lambda acessa DynamoDB (para dados de interação) ou faz chamadas ao SAP via API conforme necessário.
4. **Resposta ao Cliente**: O Lambda envia a resposta final de volta ao API Gateway, que então a encaminha para o front-end.

## 6. Cuidados e Boas Práticas

### 6.1 Qualidade dos Dados de Treinamento
Para o MVP, utilizaremos dados sintéticos de interações, simulando cenários típicos de uso. Esses dados ajudarão a validar o chatbot sem expor informações da empresa.

### 6.2 Aprendizado e Ajustes Contínuos
A infraestrutura e a aplicação foram configuradas para monitoramento e ajustes contínuos, permitindo a adição de novas intenções e o aprimoramento de respostas com base no feedback do usuário.

### 6.3 Experiência do Usuário (UX)
O fluxo de interação foi projetado para respostas rápidas e diretas, evitando mensagens vagas e criando uma experiência intuitiva e satisfatória.

### 6.4 Escalabilidade
A arquitetura baseada em microsserviços permite escalar componentes individuais conforme necessário. Com AWS Lambda e Auto Scaling, a solução se adapta automaticamente a aumentos de demanda.

### 6.5 Privacidade e Segurança
Toda a arquitetura segue a LGPD, com criptografia de dados (SSL/TLS e KMS), autenticação e autorização rigorosas, além de segurança reforçada com VPCs e IAM.

## 7. Tecnologias Envolvidas

- **Back-end**: Python (Flask/FastAPI), AWS Lambda para funções serverless.
- **NLP**: LLM da OpenAI para compreensão de linguagem natural.
- **Infraestrutura AWS**: Lambda, API Gateway, e Amazon S3 para armazenamento de logs.
- **Banco de Dados**: Amazon DynamoDB e Amazon RDS para dados de interações e estruturados.
- **Integração com APIs**: RESTful APIs via API Gateway para consulta ao SAP.
- **Monitoramento e Segurança**: CloudWatch, GuardDuty, e AWS Shield para monitoramento e proteção.

## 8. Elementos da Arquitetura e Funções
Cada elemento desempenha uma função específica para garantir o atendimento ao modelo de negócios da SoluBio:

- **AWS Lambda e API Gateway**: Núcleo de integração, permitindo consultas ao SAP e respostas em tempo real.
- **LLM da OpenAI**: Motor de interpretação e resposta para interações complexas.
- **DynamoDB e RDS**: Armazenamento de dados de interação e dados relacionais, respectivamente.
- **CloudWatch e GuardDuty**: Monitoramento de atividades e segurança.

## 9. Estimativa de Custos
Abaixo estão os custos estimados para cada serviço:

- **LLM da OpenAI**: Custos variáveis baseados no uso de tokens.
- **AWS Lambda**: Preço por chamadas e tempo de execução.
- **DynamoDB e RDS**: Custos de armazenamento e consultas.
- **API Gateway**: Tarifas baseadas no número de requisições.
- **CloudWatch e GuardDuty**: Baseado em volume de logs e monitoramento.
- **Amazon S3**: Armazenamento e backup com limites gratuitos iniciais.

## 10. Fluxo Integrado: Arquitetura de Aplicação e Infraestrutura

1. **Cliente** inicia uma conversa com a Dra. Jô via widget de chat ou plataforma de mensagem.
2. **API Gateway** recebe e encaminha a solicitação ao **Lambda**.
3. **NLP (OpenAI)** interpreta a mensagem, determinando a intenção e o fluxo de diálogo.
4. **Integração com SAP**: Lambda realiza consultas ao SAP para informações específicas.
5. **DynamoDB** armazena logs e histórico de interações.
6. **Resposta Final**: A resposta é enviada de volta ao cliente via API Gateway e front-end.

## 11. Conclusão
A arquitetura integrada entre a aplicação e infraestrutura oferece uma solução robusta, escalável e em conformidade com a LGPD. A Dra. Jô, assistente virtual da SoluBio, está equipada para oferecer suporte eficiente e personalizado, com segurança e desempenho, pronta para evoluir conforme o crescimento do volume de interações.
