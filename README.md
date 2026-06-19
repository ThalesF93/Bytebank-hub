# ByteBank 💰

> Sistema bancário distribuído baseado em microsserviços, desenvolvido com Java e Spring Boot.

[![Java](https://img.shields.io/badge/Java-25-ED8B00?style=flat&logo=openjdk&logoColor=white)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-4.x-6DB33F?style=flat&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Spring AI](https://img.shields.io/badge/Spring_AI-2.x-6DB33F?style=flat&logo=spring&logoColor=white)](https://spring.io/projects/spring-ai)
[![Kafka](https://img.shields.io/badge/Apache_Kafka-KRaft-231F20?style=flat&logo=apachekafka&logoColor=white)](https://kafka.apache.org/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![AWS](https://img.shields.io/badge/AWS-ECS%20%7C%20RDS%20%7C%20ECR-FF9900?style=flat&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)

---

## 📖 Sobre o Projeto

**ByteBank** é um sistema bancário digital construído com arquitetura de microsserviços, simulando operações financeiras reais como criação de contas, transações entre clientes e notificações. O projeto foi desenvolvido com foco em boas práticas de engenharia de software, escalabilidade e implantação em nuvem.

O ecossistema inclui um serviço de inteligência financeira com IA que permite ao usuário registrar e consultar transações em linguagem natural pelo WhatsApp, integrando Whisper, GPT-4o-mini e RAG com pgvector.

---

## 🏗️ Arquitetura

```
                        ┌─────────────────┐
                        │   API Gateway   │
                        │   (porta 8080)  │
                        └────────┬────────┘
                                 │
         ┌───────────────────────┼───────────────────────┐
         │                       │                       │
┌────────▼────────┐   ┌──────────▼──────┐   ┌───────────▼─────────┐
│bytebank-customer│   │bytebank-accounts│   │bytebank-transactions│
│   (Clientes)    │   │   (Contas)      │   │   (Transações)      │
└─────────────────┘   └─────────────────┘   └─────────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │ Kafka (transaction.created)
                    ┌────────────▼────────────┐
                    │   finance-ai-service     │
                    │  GPT-4o-mini + RAG       │
                    │  pgvector + Whisper      │
                    └─────────────────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   bytebank-notification  │
                    │     (Notificações)       │
                    └─────────────────────────┘

   ┌──────────────────────────────────────────────────────────────────┐
   │  Eureka Server  │  Kafka (KRaft)  │  Redis  │  RabbitMQ          │
   │  Prometheus  │  Grafana  │  Zipkin                               │
   └──────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Microsserviços

| Serviço | Descrição | Porta |
|---------|-----------|-------|
| `eureka-server` | Service Discovery | 8761 |
| `api-gateway` | Gateway e roteamento | 8080 |
| `bytebank-customer` | Gerenciamento de clientes | 8081 |
| `bytebank-accounts` | Gerenciamento de contas | 8082 |
| `bytebank-transactions` | Processamento de transações | 8083 |
| `bytebank-notification` | Notificações por e-mail | 8084 |
| `finance-ai-service` | IA financeira via WhatsApp | 8085 |

---

## 🛠️ Stack Tecnológica

**Backend**
Java 25, Spring Boot 4.x, Spring Cloud, Spring AI 2.x, OpenFeign, Spring Data JPA, Resilience4j

**Inteligência Artificial**
OpenAI GPT-4o-mini (function calling), Whisper (transcrição de áudio), text-embedding-ada-002 (embeddings), pgvector (busca semântica por similaridade)

**Mensageria**
Apache Kafka (KRaft, sem Zookeeper), RabbitMQ

**Dados**
PostgreSQL 16, pgvector, Redis

**Integração WhatsApp**
WAHA (WhatsApp HTTP API), n8n

**Infraestrutura**
Docker, Docker Compose, AWS ECS Fargate, AWS ECR, AWS RDS

**Observabilidade**
Prometheus, Grafana, Zipkin (Micrometer Tracing), Spring Boot Actuator

---

## 🤖 Finance AI Service

O `finance-ai-service` é o serviço de inteligência financeira do ByteBank. Permite ao usuário interagir com suas finanças em linguagem natural pelo WhatsApp.

**Registro de transações**
O usuário envia um áudio ou texto como *"gastei 50 reais no mercado"*. O serviço transcreve com Whisper, interpreta a intenção com GPT-4o-mini via function calling e persiste automaticamente.

**Consulta com RAG**
O usuário pergunta *"quanto gastei esse mês?"*. O serviço gera um embedding da pergunta, busca as transações semanticamente mais relevantes no pgvector e alimenta o GPT com esses dados para uma resposta precisa.

**Consumo de eventos Kafka**
Toda transação bancária processada pelo `bytebank-transactions` publica um evento no tópico `transaction.created`. O `finance-ai-service` consome esses eventos, persiste a operação e gera o embedding correspondente para o RAG.

---

## 🔗 Repositórios

- [Serviço de Clientes](https://github.com/thalesF93/bytebank-customer)
- [Serviço de Contas](https://github.com/thalesF93/bytebank-accounts)
- [Serviço de Transações](https://github.com/thalesF93/bytebank-transactions)
- [Serviço de Notificações](https://github.com/thalesF93/bytebank-notification)
- [Finance AI Service](https://github.com/ThalesF93/bytebank-finance-ai-service)
- [API Gateway](https://github.com/thalesF93/bytebank-api-gateway)
- [Eureka Server](https://github.com/thalesF93/bytebank-eureka-server)
- [Infra](https://github.com/thalesF93/bytebank-infra)

---

## 🌐 Links de Acesso

| Serviço | URL | Status |
|---------|-----|--------|
| 📄 Swagger / Docs | [bytebank.thalesf.dev/swagger-ui.html](https://bytebank.thalesf.dev/swagger-ui.html) | ✅ Online |
| 📊 Grafana | [bytebank.thalesf.dev/grafana](https://bytebank.thalesf.dev/grafana) | ✅ Online |

---

## ⚙️ Como Executar Localmente

### Pré-requisitos

- Docker e Docker Compose instalados
- JDK 25+
- Rede Docker criada: `docker network create bytebank-net`

### Subindo os serviços

Cada microsserviço possui seu próprio `docker-compose.yml`. Para subir um serviço:

```bash
# Exemplo: subindo o serviço de contas
cd bytebank-accounts
docker compose -p bytebank-accounts up -d --build
```

Siga a ordem recomendada:
1. `eureka-server`
2. Infraestrutura (Kafka, Redis, RabbitMQ, PostgreSQL)
3. `bytebank-customer`
4. `bytebank-accounts`
5. `bytebank-transactions`
6. `bytebank-notification`
7. `finance-ai-service`
8. `api-gateway`

### URLs de Acesso Local

| Serviço | URL |
|---------|-----|
| API Gateway | http://localhost:8080 |
| Eureka Dashboard | http://localhost:8761 |
| Swagger UI | http://localhost:8080/swagger-ui.html |
| Grafana | http://localhost:3000 |
| Prometheus | http://localhost:9090 |
| Kafka UI | http://localhost:8090 |
| RabbitMQ Management | http://localhost:15672 |

---

## 📡 Principais Endpoints

> Documentação completa disponível no **[Swagger UI](https://bytebank.thalesf.dev/swagger-ui.html)**

### 👤 Clientes (`/api/v2/customers`)
```
GET    /api/v2/customers         → Listar todos os clientes
GET    /api/v2/customers/{id}    → Buscar cliente por ID
POST   /api/v2/customers         → Criar novo cliente
PUT    /api/v2/customers/{id}    → Atualizar cliente
DELETE /api/v2/customers/{id}    → Remover cliente
```

### 🏦 Contas (`/api/v1/accounts`)
```
GET    /api/v1/accounts          → Listar contas
GET    /api/v1/accounts/{id}     → Buscar conta por ID
POST   /api/v1/accounts          → Criar nova conta
DELETE /api/v1/accounts/{id}     → Encerrar conta
```

### 💸 Transações (`/api/v1/transactions`)
```
POST   /api/v1/transactions/deposit     → Depósito
POST   /api/v1/transactions/withdraw    → Saque
POST   /api/v1/transactions/transfer    → Transferência
GET    /api/v1/transactions/{accountId} → Extrato da conta
```

### 🤖 Finance AI (`/api/v1/operations`)
```
POST   /api/v1/operations/whatsapp  → Recebe áudio ou texto do WhatsApp
POST   /api/v1/operations/create    → Persiste operação diretamente
```

---

## ☁️ Implantação na AWS

- **AWS ECR** — Armazenamento das imagens Docker de cada microsserviço
- **AWS ECS (Fargate)** — Execução de containers sem gerenciamento de servidores
- **AWS RDS (PostgreSQL)** — Banco de dados gerenciado, isolado por serviço
- **AWS ALB** — Application Load Balancer com HTTPS e domínio customizado
- **AWS ACM** — Certificado SSL gerenciado

---

## 📁 Estrutura do Ecossistema

```
Ecossistema ByteBank
├── bytebank-hub              ← este repositório (índice)
├── bytebank-eureka-server
├── bytebank-api-gateway
├── bytebank-customer
├── bytebank-accounts
├── bytebank-transactions
├── bytebank-notification
├── finance-ai-service
└── bytebank-infra
```

---

## 🧠 Padrões de Arquitetura Aplicados

**Distribuição e comunicação**
Microsserviços independentes, Service Discovery (Eureka), API Gateway com filtros customizados, comunicação síncrona via Feign Client com Circuit Breaker (Resilience4j), comunicação assíncrona via Kafka e RabbitMQ.

**Clean Architecture**
Todos os serviços seguem separação em camadas domain, application e infrastructure, permitindo evolução independente de cada camada.

**Resiliência**
Idempotência com Redis (TTL 24h), Dead Letter Topic no Kafka, retry automático com backoff, Circuit Breaker com fallback.

**IA e RAG**
Function calling com `@Tool` para persistência, RAG com pgvector para consultas semânticas, embeddings gerados via OpenAI e filtrados por `customerId` para isolamento de dados.

---

## 📊 Observabilidade

Spring Boot Actuator expõe health checks e métricas. Prometheus coleta e armazena. Grafana exibe dashboards. Zipkin rastreia requisições distribuídas entre os serviços.

---

## 👤 Autor

**Thales Fernandes**

[![GitHub](https://img.shields.io/badge/GitHub-ThalesF93-181717?style=flat&logo=github)](https://github.com/ThalesF93)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Thales_Fernandes-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/thales-fernandes-24418126a/)
