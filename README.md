# Bytebank-hub

# ByteBank 💰

> Distributed banking system based on microservices, developed with Java Spring Boot and deployed on AWS.

[![Java](https://img.shields.io/badge/Java-21-ED8B00?style=flat&logo=openjdk&logoColor=white)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=flat&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Spring Cloud](https://img.shields.io/badge/Spring_Cloud-Netflix-6DB33F?style=flat&logo=spring&logoColor=white)](https://spring.io/projects/spring-cloud)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![AWS](https://img.shields.io/badge/AWS-ECS%20%7C%20RDS%20%7C%20ECR-FF9900?style=flat&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Gradle](https://img.shields.io/badge/Gradle-Multi--Module-02303A?style=flat&logo=gradle&logoColor=white)](https://gradle.org/)

---

## 📖 About the Project

**ByteBank** is a digital banking system built with microservices architecture, simulating real financial operations such as account creation, transactions between customers, and notifications. The project was developed with a focus on software engineering best practices, scalability, and cloud deployment.

This project demonstrates the practical application of patterns such as **Service Discovery**, **API Gateway**, **synchronous communication via Feign Client**, and **observability with Prometheus + Grafana**, alongside a fully containerized infrastructure with **Docker** and provisioned on **AWS**.

---

## 🏗️ Architecture

```
                        ┌─────────────────┐
                        │   API Gateway   │
                        │   (port 8080)   │
                        └────────┬────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
   ┌──────────▼──────┐  ┌────────▼────────┐  ┌─────▼───────────────┐
   │bytebank-customer│  │bytebank-accounts│  │bytebank-transactions│
   │   (Customers)   │  │   (Accounts)    │  │   (Transactions)    │
   └─────────────────┘  └─────────────────┘  └─────────────────────┘
              │                  │                  │
              └──────────────────┼──────────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   bytebank-notification  │
                    │     (Notifications)      │
                    └─────────────────────────┘

   ┌─────────────────────────────────────────────────────────────────┐
   │  Eureka Server (Service Discovery)  │  Prometheus  │  Grafana   │
   └─────────────────────────────────────────────────────────────────┘
```
- [Accounts Service](https://github.com/thalesF93/bytebank-accounts-service)
- [Customer Service](https://github.com/thalesF93/bytebank-customer-service)
- [Transactions Service](https://github.com/thalesF93/bytebank-transactions-service)
- [Notification Service](https://github.com/thalesF93/bytebank-notification-service)
- [API Gateway](https://github.com/thalesF93/bytebank-api-gateway)
- [Eureka Server](https://github.com/thalesF93/bytebank-eureka-server)
- [Infra](https://github.com/thalesF93/bytebank-infra)
---

## 🚀 Microservices

| Service | Description | Port |
|---|---|---|
| `eureka-server` | Service Discovery | 8761 |
| `api-gateway` | Gateway and Load Balancing | 8080 |
| `bytebank-customer` | Customer management | 8081 |
| `bytebank-accounts` | Account management | 8082 |
| `bytebank-transactions` | Transaction processing | 8083 |
| `bytebank-notification` | Notifications | 8084 |

---

## 🛠️ Core Technologies

- **Backend:** Java 21, Spring Boot 3.x, Spring Cloud, OpenFeign, Spring Data JPA
- **Infrastructure:** Docker, AWS ECS (Fargate), AWS ECR, AWS RDS (PostgreSQL)
- **Observability:** Prometheus, Grafana, Spring Boot Actuator
- **Build:** Gradle Multi-Module

---

## 🌐 Access Links

| Service | URL | Status |
|---|---|---|
| 📄 **Swagger / API Docs** | [bytebank.thalesf.dev/swagger-ui.html](https://bytebank.thalesf.dev/swagger-ui.html) | ✅ Online |
| 📊 **Grafana Dashboard** | https://bytebank.thalesf.dev/grafana | ✅ Online |

---

## ⚙️ How to Run Locally

### Prerequisites
- Docker and Docker Compose installed
- JDK 21+
- Gradle

### Steps

```bash
# Clone the repository
git clone https://github.com/ThalesF93/ByteBank.git
cd ByteBank

# Start all services
docker-compose up --build
```

### Local Access URLs

| Service | URL |
|---|---|
| API Gateway | http://localhost:8080 |
| Eureka Dashboard | http://localhost:8761 |
| Swagger UI | http://localhost:8080/swagger-ui.html |
| Grafana | http://localhost:3000 |
| Prometheus | http://localhost:9090 |

---

## 📡 Main Endpoints

> Full documentation available on **[Swagger UI](https://bytebank.thalesf.dev/swagger-ui.html)**

### 👤 Customers (`/customers`)
```
GET    /customers         → List all customers
GET    /customers/{id}    → Find customer by ID
POST   /customers         → Create new customer
PUT    /customers/{id}    → Update customer
DELETE /customers/{id}    → Remove customer
```

### 🏦 Accounts (`/accounts`)
```
GET    /accounts          → List accounts
GET    /accounts/{id}     → Find account by ID
POST   /accounts          → Create new account
DELETE /accounts/{id}     → Close account
```

### 💸 Transactions (`/transactions`)
```
POST   /transactions/deposit    → Deposit
POST   /transactions/withdraw   → Withdraw
POST   /transactions/transfer   → Transfer
GET    /transactions/{accountId} → Account statement
```

---

## ☁️ AWS Deployment

- **AWS ECR** — Storage of Docker images for each microservice
- **AWS ECS (Fargate)** — Serverless container execution
- **AWS RDS (PostgreSQL)** — Managed database, isolated per service

---

## 📊 Observability

- **Spring Boot Actuator** — Health checks and exposed metrics
- **Prometheus** — Metrics collection and storage
- **Grafana** — Monitoring dashboards
- **Zipkin** — Distributed request tracing 

---

## 🧠 Applied Architecture Patterns

- **Microservices Architecture** — Independent and deployable services
- **Service Discovery** (Eureka) — Dynamic service registration and discovery
- **API Gateway Pattern** — Single entry point with routing
- **Database per Service** — Data isolation per microservice
- **Synchronous Communication** (Feign Client) — HTTP communication between services
- **Containerization** (Docker) — Environment portability and consistency

---

## 📁 Project Structure

```
ByteBank/
├── eureka-server/
├── api-gateway/
├── bytebank-customer/
├── bytebank-accounts/
├── bytebank-transactions/
├── bytebank-notification/
├── docker-compose.yml
└── build.gradle
```

---

## 👤 Author

**Thales Fernandes**

[![GitHub](https://img.shields.io/badge/GitHub-ThalesF93-181717?style=flat&logo=github)](https://github.com/ThalesF93)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Thales_Fernandes-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/thales-fernandes-24418126a/)
