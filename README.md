# 🚀 Nexus Delivery - Ecosystem

![Nexus Delivery Architecture](https://img.shields.io/badge/Architecture-Microservices-orange)
![Stack](https://img.shields.io/badge/Stack-NestJS%20%7C%20Next.js%20%7C%20Kafka-blue)
![Deploy](https://img.shields.io/badge/Infra-Kubernetes-blueviolet)

**Nexus Delivery** is a high-performance distributed delivery platform inspired by industry leaders like Uber Eats and Glovo. It is designed to solve real-world challenges related to scalability, resilience, and maintainability in mission-critical systems.

---

## 🧠 Architectural Philosophy

This project goes beyond simple CRUD applications. It is a complete ecosystem built on:

* **Domain-Driven Design (DDD):** Strong focus on core business logic with well-defined *Bounded Contexts* to avoid tight coupling.
* **Event-Driven Architecture (EDA):** Asynchronous communication using Apache Kafka ensures that failures in one service do not bring down the entire system.
* **Zero-Trust Security:** Centralized authentication handled at the API Gateway (Global Auth).
* **Observability First:** The system is fully monitored from day one (Metrics, Logs, and Traces).

---

## 🧩 Tech Stack & Rationale

| Layer                   | Technology           | Why?                                                                                       |
| :---------------------- | :------------------- | :----------------------------------------------------------------------------------------- |
| **Monorepo**            | **Nx + Turborepo**   | Efficient multi-service management, shared contracts (DTOs), and ultra-fast build caching. |
| **Backend**             | **NestJS + Fastify** | Robust modular architecture with high-performance Fastify adapter.                         |
| **Frontend**            | **Next.js 14+**      | App Router, Server Components for better SEO and performance, and native SSR support.      |
| **Messaging**           | **Apache Kafka**     | Industry-standard for high-scale distributed systems and eventual consistency.             |
| **Database**            | **PostgreSQL**       | Reliable relational database for critical transactions like orders and payments.           |
| **Cache**               | **Redis**            | Significant latency reduction for menu queries and session management.                     |
| **Infra/Orchestration** | **Kubernetes**       | Auto-healing, horizontal scaling (HPA), and container orchestration.                       |

---

## 📁 Project Structure

The monorepo approach enables code sharing between Backend and Frontend via the `packages` directory.

```text
nexus-delivery/
├── apps/
│   ├── order-service/        # Order orchestration
│   ├── payment-service/      # Payment processing
│   ├── catalog-service/      # Menus and restaurants
│   ├── notification-service/ # WebSockets and alerts
│   ├── identity-service/     # Authentication and users
│   └── web-client/           # Next.js frontend
├── packages/
│   ├── common/               # Middlewares and utilities
│   ├── contracts/            # Event schemas and TS interfaces
│   └── database/             # Global Prisma configuration
├── infra/
│   ├── docker/               # Development Docker Compose setup
│   └── k8s/                  # Kubernetes manifests (Deployments, Services, HPA)
└── .github/workflows/        # CI/CD pipelines
```

---

## ⚙️ Getting Started

### 1. Clone & Install

```bash
git clone https://github.com/your-username/nexus-delivery.git
cd nexus-delivery
npm install
```

### 2. Local Infrastructure (Docker)

Start PostgreSQL, Kafka, and Redis with a single command:

```bash
docker-compose -f infra/docker/docker-compose.yml up -d
```

### 3. Run the Ecosystem

Turborepo will start all services in parallel:

```bash
npx turbo run dev
```

---

## 🚢 CI/CD & Deployment

Our GitHub Actions pipeline automates the entire lifecycle:

* **Continuous Integration (CI):** Runs linting and automated tests on every push.
* **Dockerization:** Builds lightweight multi-stage production images.
* **Continuous Deployment (CD):** Automatically updates the Kubernetes cluster via `kubectl rollout`.

---

## 🛡️ Gateway Authentication (Global Auth)

For performance and security, Nexus avoids validating JWTs inside each microservice.

* NGINX Ingress intercepts incoming requests.
* Identity Service validates the token.
* If valid, the Gateway injects the `x-user-id` header and forwards the request.

This approach removes authentication logic from business services, making them lighter and more focused.

---

## 📈 Observability

Access dashboards to monitor the system health:

* **Grafana:** http://localhost:3001 (Traffic and error metrics)
* **Jaeger:** http://localhost:16686 (Distributed tracing)
* **Prometheus:** http://localhost:9090 (Raw metrics collection)
