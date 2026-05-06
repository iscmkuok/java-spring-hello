# java-spring-hello

A Spring Boot 3 application with JPA, Spring Security (JWT), REST CRUD API, Docker, and GitHub Actions CI/CD.

## Features

- **JPA / Database**: H2 (default, in-memory), PostgreSQL and MySQL profiles
- **Spring Security with JWT**: Stateless authentication using HS256 tokens
- **REST CRUD API**: `/api/items` – full CRUD for `Item` resources
- **Docker**: Multi-stage Dockerfile + Docker Compose (app + PostgreSQL)
- **CI/CD**: GitHub Actions – build, test, and Docker image on every push/PR to `main`

## Quick Start

### Local (H2 – no extra setup needed)

```bash
mvn spring-boot:run
```

### Docker Compose (PostgreSQL)

```bash
docker-compose up --build
```

### Switch to MySQL profile

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=mysql
```

## API Endpoints

| Method | URL | Auth required | Description |
|--------|-----|:---:|-------------|
| GET | `/hello` | No | Greeting |
| POST | `/api/auth/register` | No | Register new user |
| POST | `/api/auth/login` | No | Login – returns JWT |
| GET | `/api/items` | Yes | List all items |
| GET | `/api/items/{id}` | Yes | Get item by ID |
| POST | `/api/items` | Yes | Create item |
| PUT | `/api/items/{id}` | Yes | Update item |
| DELETE | `/api/items/{id}` | Yes | Delete item |

H2 console (dev only): `http://localhost:8080/h2-console`

## Usage Examples

### Register

```bash
curl -s -X POST http://localhost:8080/api/auth/register \
  -H 'Content-Type: application/json' \
  -d '{"username":"alice","password":"secret"}'
```

### Login

```bash
TOKEN=$(curl -s -X POST http://localhost:8080/api/auth/login \
  -H 'Content-Type: application/json' \
  -d '{"username":"alice","password":"secret"}' | jq -r .token)
```

### Create item

```bash
curl -s -X POST http://localhost:8080/api/items \
  -H "Authorization: Bearer $TOKEN" \
  -H 'Content-Type: application/json' \
  -d '{"name":"Widget","description":"A useful widget"}'
```

### List items

```bash
curl -s http://localhost:8080/api/items \
  -H "Authorization: Bearer $TOKEN"
```
