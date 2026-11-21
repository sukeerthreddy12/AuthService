# Finance Tracker — Auth Service

Spring Boot service that owns **identity**: signup, login, logout, JWT access tokens, and refresh tokens. After a successful signup it also **publishes a user event to Kafka** so the User Service can create the profile row asynchronously.

Part of the Finance Tracker microservices app (Kong + Kafka + MySQL).

**System map (architecture, Kong, all repos):** [ARCHITECTURE.md](./ARCHITECTURE.md)

## What this service does

- Register users and store credentials (password hashed)
- Issue JWT access tokens + refresh tokens
- Validate session (`/auth/v1/ping` returns user id when Bearer token is valid)
- Publish signup/user info events to Kafka topic `user_service` for the User Service consumer

It does **not** own rich profile fields long-term — that is User Service’s job after the Kafka event.

## Main HTTP API

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/auth/v1/signup` | Create account + return tokens |
| POST | `/auth/v1/login` | Authenticate + return tokens |
| POST | `/auth/v1/refreshToken` | Rotate access token |
| POST | `/auth/v1/logout` | Invalidate refresh token |
| GET | `/auth/v1/ping` | Auth check (needs Bearer token) |
| GET | `/auth/v1/health` | Liveness |

Behind Kong, the frontend calls the same paths on the gateway base URL (route prefix `/auth`).

## Stack

- Java / Spring Boot / Spring Security
- MySQL
- Kafka producer
- JWT

## Setup

```bash
cp .env.example .env
# set MYSQL_*, JWT_SECRET, KAFKA_HOST
./gradlew bootRun
```

Default port: **9898**.

## Env

Copy `.env.example` → `.env`. Never commit `.env`.

## Related repos

- [User Service](https://github.com/sukeerthreddy12/FinanceTrackerUserService) — consumes `user_service` events
- [Expense Service](https://github.com/sukeerthreddy12/FinanceTrackerExpenseService)
- [DS Service](https://github.com/sukeerthreddy12/FinanceTrackerDsService)
- [Frontend](https://github.com/sukeerthreddy12/FinanceTrackerFrontend)
