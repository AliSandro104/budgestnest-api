# BudgetNest API

Backend for **BudgetNest** — a personal and shared household finance manager. Built with Spring Boot 4, Java 26, Maven, and PostgreSQL 18.

## Tech Stack

| Layer       | Technology                                      |
|-------------|-------------------------------------------------|
| Runtime     | Java 26                                         |
| Framework   | Spring Boot 4 (Web, Security, Data JPA, Actuator, Mail, Validation) |
| Build tool  | Maven                                           |
| Database    | PostgreSQL 18                                   |
| Migrations  | Liquibase                                       |
| Docs        | OpenAPI / Springdoc                             |
| Auth        | JWT (access + refresh tokens)                   |
| Local infra | Docker Compose v2                               |
| CI/CD       | GitHub Actions                                  |

## Project Structure

```
budgetnest-api/            ← this repo (Spring Boot backend)
src/
  main/
    java/io/github/alisandro104/budgetnest_api/
      auth/                ← JWT, email verification, password reset
      household/           ← household management and member roles
      transaction/         ← income and expense records
      subscription/        ← recurring bills and reminders
      budget/              ← monthly budgets and rollover
      notification/        ← scheduled alerts and email dispatch
      audit/               ← audit event logging
      reporting/           ← aggregation and export
    resources/
      application.yaml
      db/changelog/        ← Liquibase changelogs
  test/
pom.xml
```

## Getting Started

### Prerequisites

- Java 21+
- Maven 3.9+
- Docker Engine + Docker Compose v2

### Run locally (DB in Docker, app on host)

```bash
# Start PostgreSQL
docker compose -f infra/docker-compose.yml up postgres -d

# Run the app
./mvnw spring-boot:run
```

The API will be available at `http://localhost:8080`.

### Run fully containerised

```bash
docker compose -f infra/docker-compose.yml up --build
```

Services started:

| Service   | Port  | Description                  |
|-----------|-------|------------------------------|
| backend   | 8080  | Spring Boot REST API         |
| postgres  | 5432  | PostgreSQL 18 database       |
| frontend  | 4200  | Angular 21 dev server        |
| mailpit   | 8025  | Email testing UI             |

## Environment Variables

Copy `infra/.env.example` to `infra/.env` and fill in the values before starting Docker Compose.

| Variable                  | Description                         |
|---------------------------|-------------------------------------|
| `DB_URL`                  | JDBC URL for PostgreSQL             |
| `DB_USERNAME`             | Database username                   |
| `DB_PASSWORD`             | Database password                   |
| `JWT_SECRET`              | Secret key for signing JWTs         |
| `JWT_ACCESS_EXPIRY_MS`    | Access token lifetime in ms         |
| `JWT_REFRESH_EXPIRY_MS`   | Refresh token lifetime in ms        |
| `MAIL_HOST`               | SMTP host (e.g. `mailpit` locally)  |
| `MAIL_PORT`               | SMTP port                           |

## API Documentation

When the application is running, OpenAPI docs are available at:

- Swagger UI: `http://localhost:8080/swagger-ui.html`
- OpenAPI JSON: `http://localhost:8080/v3/api-docs`

## Running Tests

```bash
# Unit + integration tests (uses Testcontainers for PostgreSQL)
./mvnw verify
```

## CI/CD

GitHub Actions pipelines are defined in `.github/workflows/`. The pipeline covers:

1. Backend: `mvn verify` (unit tests + integration tests)
2. Security: dependency scanning via Dependabot
3. Docker image build for the backend
4. Deploy to staging / manual promotion to production

## Planned Features

### Phase 1 — MVP
- [ ] Register / login / logout (JWT)
- [ ] Email verification and password reset
- [ ] Create categories
- [ ] Add income and expense transactions
- [ ] Monthly budget overview
- [ ] Dashboard with spending totals

### Phase 2 — Subscriptions
- [ ] Add recurring bills with renewal date and price
- [ ] Scheduled reminders via email
- [ ] Async notification processing

### Phase 3 — Household collaboration
- [ ] Create household and invite members by email
- [ ] Role-based access control (owner / member)
- [ ] Shared wallet and expense splitting
- [ ] Activity history / audit log

### Phase 4 — Reports
- [ ] Monthly spend charts
- [ ] Category breakdown
- [ ] CSV export
- [ ] Anomaly alerts

### Phase 5 — Security & ops polish
- [ ] Rate limiting
- [ ] Session history
- [ ] Health endpoints (Actuator)
- [ ] Full Dockerised local stack

## Related Repositories

| Repo            | Description            |
|-----------------|------------------------|
| `budgetnest-ui` | Angular 21 frontend    |
| `budgetnest-infra` | Docker Compose, Nginx, scripts |

## License

This project is private and not yet licensed for public redistribution.
