# Finance Tracker — Auth Service

Spring Boot auth service (JWT signup/login). Part of the Finance Tracker application (Kafka + MySQL + Kong).

## Stack

- Java / Spring Boot / Spring Security
- MySQL
- Kafka (publishes user events)
- JWT

## Setup

```bash
cp .env.example .env
# set MYSQL_*, JWT_SECRET, KAFKA_HOST
./gradlew bootRun
```

Default port: `9898`.

## Env

Copy `.env.example` → `.env`. Never commit `.env`.
