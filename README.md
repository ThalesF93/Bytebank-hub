# Bytebank-hub
 
# ByteBank 💰
 
> Sistema bancário distribuído baseado em microsserviços, desenvolvido com Java Spring Boot e implantado na AWS.
 
[![Java](https://img.shields.io/badge/Java-21-ED8B00?style=flat&logo=openjdk&logoColor=white)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=flat&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Spring Cloud](https://img.shields.io/badge/Spring_Cloud-Netflix-6DB33F?style=flat&logo=spring&logoColor=white)](https://spring.io/projects/spring-cloud)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![AWS](https://img.shields.io/badge/AWS-ECS%20%7C%20RDS%20%7C%20ECR-FF9900?style=flat&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Gradle](https://img.shields.io/badge/Gradle-Multi--Module-02303A?style=flat&logo=gradle&logoColor=white)](https://gradle.org/)
 
---
 
## 📖 Sobre o Projeto
 
**ByteBank** é um sistema bancário digital construído com arquitetura de microsserviços, simulando operações financeiras reais como criação de contas, transações entre clientes e notificações. O projeto foi desenvolvido com foco em boas práticas de engenharia de software, escalabilidade e implantação em nuvem.
 
Este projeto demonstra a aplicação prática de padrões como **Service Discovery**, **API Gateway**, **comunicação síncrona via Feign Client** e **observabilidade com Prometheus + Grafana**, junto a uma infraestrutura totalmente conteinerizada com **Docker** e provisionada na **AWS**.
 
---
 
## 🏗️ Arquitetura
 
```
                        ┌─────────────────┐
                        │   API Gateway   │
                        │   (porta 8080)  │
                        └────────┬────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
   ┌──────────▼──────┐  ┌────────▼────────┐  ┌─────▼───────────────┐
   │bytebank-customer│  │bytebank-accounts│  │bytebank-transactions│
   │    (Clientes)   │  │    (Contas)     │  │   (Transações)      │
   └─────────────────┘  └─────────────────┘  └─────────────────────┘
              │                  │                  │
              └──────────────────┼──────────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   bytebank-notification  │
                    │     (Notificações)       │
                    └─────────────────────────┘
 
   ┌─────────────────────────────────────────────────────────────────┐
   │  Eureka Server (Service Discovery)  │  Prometheus  │  Grafana   │
   └─────────────────────────────────────────────────────────────────┘
```
 
- [Serviço de Contas](https://github.com/thalesF93/bytebank-accounts)
- [Serviço de Clientes](https://github.com/thalesF93/bytebank-customer)
- [Serviço de Transações](https://github.com/thalesF93/bytebank-transactions)
- [Serviço de Notificações](https://github.com/thalesF93/bytebank-notification)
- [API Gateway](https://github.com/thalesF93/bytebank-api-gateway)
- [Eureka Server](https://github.com/thalesF93/bytebank-eureka-server)
- [Infra](https://github.com/thalesF93/bytebank-infra)
---
 
## 🚀 Microsserviços
 
| Serviço | Descrição | Porta |
|---|---|---|
| `eureka-server` | Service Discovery | 8761 |
| `api-gateway` | Gateway e Balanceamento de Carga | 8080 |
| `bytebank-customer` | Gerenciamento de clientes | 8081 |
| `bytebank-accounts` | Gerenciamento de contas | 8082 |
| `bytebank-transactions` | Processamento de transações | 8083 |
| `bytebank-notification` | Notificações | 8084 |
 
---
 
## 🛠️ Tecnologias Principais
 
- **Backend:** Java 21, Spring Boot 3.x, Spring Cloud, OpenFeign, Spring Data JPA
- **Infraestrutura:** Docker, AWS ECS (Fargate), AWS ECR, AWS RDS (PostgreSQL)
- **Observabilidade:** Prometheus, Grafana, Spring Boot Actuator
- **Build:** Gradle Multi-Module
---
 
## 🌐 Links de Acesso
 
| Serviço | URL | Status |
|---|---|---|
| 📄 **Swagger / Docs da API** | [bytebank.thalesf.dev/swagger-ui.html](https://bytebank.thalesf.dev/swagger-ui.html) | ✅ Online |
| 📊 **Dashboard Grafana** | https://bytebank.thalesf.dev/grafana | ✅ Online |
 
---
 
## ⚙️ Como Executar Localmente
 
### Pré-requisitos
- Docker e Docker Compose instalados
- JDK 21+
- Gradle
### Passos
 
```bash
# Clone o repositório
git clone https://github.com/ThalesF93/ByteBank.git
cd ByteBank
 
# Inicie todos os serviços
docker-compose up --build
```
 
### URLs de Acesso Local
 
| Serviço | URL |
|---|---|
| API Gateway | http://localhost:8080 |
| Eureka Dashboard | http://localhost:8761 |
| Swagger UI | http://localhost:8080/swagger-ui.html |
| Grafana | http://localhost:3000 |
| Prometheus | http://localhost:9090 |
 
---
 
## 📡 Principais Endpoints
 
> Documentação completa disponível no **[Swagger UI](https://bytebank.thalesf.dev/swagger-ui.html)**
 
### 👤 Clientes (`/customers`)
```
GET    /customers         → Listar todos os clientes
GET    /customers/{id}    → Buscar cliente por ID
POST   /customers         → Criar novo cliente
PUT    /customers/{id}    → Atualizar cliente
DELETE /customers/{id}    → Remover cliente
```
 
### 🏦 Contas (`/accounts`)
```
GET    /accounts          → Listar contas
GET    /accounts/{id}     → Buscar conta por ID
POST   /accounts          → Criar nova conta
DELETE /accounts/{id}     → Encerrar conta
```
 
### 💸 Transações (`/transactions`)
```
POST   /transactions/deposit     → Depósito
POST   /transactions/withdraw    → Saque
POST   /transactions/transfer    → Transferência
GET    /transactions/{accountId} → Extrato da conta
```
 
---
 
## ☁️ Implantação na AWS
 
- **AWS ECR** — Armazenamento das imagens Docker de cada microsserviço
- **AWS ECS (Fargate)** — Execução de contêineres sem servidor
- **AWS RDS (PostgreSQL)** — Banco de dados gerenciado, isolado por serviço
---
 
## 📊 Observabilidade
 
- **Spring Boot Actuator** — Health checks e métricas expostas
- **Prometheus** — Coleta e armazenamento de métricas
- **Grafana** — Dashboards de monitoramento
- **Zipkin** — Rastreamento distribuído de requisições
---
 
## 🧠 Padrões de Arquitetura Aplicados
 
- **Arquitetura de Microsserviços** — Serviços independentes e implantáveis
- **Service Discovery** (Eureka) — Registro e descoberta dinâmica de serviços
- **API Gateway Pattern** — Ponto único de entrada com roteamento
- **Database per Service** — Isolamento de dados por microsserviço
- **Comunicação Síncrona** (Feign Client) — Comunicação HTTP entre serviços
- **Conteinerização** (Docker) — Portabilidade e consistência de ambiente
---
 
## 📁 Estrutura do Projeto
 
```
Ecossistema ByteBank
├── bytebank-hub
├── bytebank-eureka-server
├── bytebank-api-gateway
├── bytebank-customer
├── bytebank-accounts
├── bytebank-transactions
├── bytebank-notification
└── bytebank-infra
```
 
---
 
## 👤 Autor
 
**Thales Fernandes**
 
[![GitHub](https://img.shields.io/badge/GitHub-ThalesF93-181717?style=flat&logo=github)](https://github.com/ThalesF93)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Thales_Fernandes-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/thales-fernandes-24418126a/)
