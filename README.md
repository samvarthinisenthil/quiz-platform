# Quiz Platform

Spring Boot 3.5 (Java 21) + MySQL 8 quiz application, fully containerized.

## Run everything with Docker Compose

Requires Docker and Docker Compose only — no local Java/Maven/MySQL needed.

```bash
docker compose up --build
```

This starts two containers:

- **mysql** – MySQL 8.0 database, with a persistent volume (`mysql-data`) and a health check so the app waits until MySQL is actually ready.
- **app** – the Spring Boot application, built from source via a multi-stage Dockerfile, connecting to the `mysql` service using the `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD` environment variables set in `docker-compose.yml`.

Once both containers are up, open:

```
http://localhost:8085
```

You'll land on `/questions`, a simple page to add and list quiz questions, backed by MySQL (Hibernate auto-creates the `question` table on startup via `spring.jpa.hibernate.ddl-auto=update`).

To stop:

```bash
docker compose down
```

To also wipe the database volume:

```bash
docker compose down -v
```

## Run locally without Docker (optional)

Requires local Java 21, Maven (or use `./mvnw`), and a MySQL 8 instance.

```bash
export DB_HOST=localhost
export DB_PORT=3306
export DB_NAME=quizdb
export DB_USER=quizuser
export DB_PASSWORD=quizpass

./mvnw spring-boot:run
```

## Configuration

Database connection is fully environment-variable driven (see `src/main/resources/application.properties`), so the same jar/image works both inside Docker Compose and against any external MySQL instance.

| Variable      | Default    | Description          |
|---------------|------------|-----------------------|
| `DB_HOST`     | localhost  | MySQL host            |
| `DB_PORT`     | 3306       | MySQL port            |
| `DB_NAME`     | quizdb     | Database name         |
| `DB_USER`     | quizuser   | Database user         |
| `DB_PASSWORD` | quizpass   | Database password     |

## Tests

Tests run against an in-memory H2 database (no MySQL required):

```bash
./mvnw test
```
