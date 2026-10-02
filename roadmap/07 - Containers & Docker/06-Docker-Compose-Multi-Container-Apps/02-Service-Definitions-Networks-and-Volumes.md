# 02 - Service Definitions, Networks, and Volumes

## 1. Complete Multi-Tier Compose Architecture

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    networks:
      - frontend_net
    depends_on:
      api:
        condition: service_healthy

  api:
    build:
      context: ./api
      dockerfile: Dockerfile
    environment:
      - DB_HOST=db
      - DB_NAME=app_db
    networks:
      - frontend_net
      - backend_net
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: app_db
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    secrets:
      - db_password
    volumes:
      - pg_data:/var/lib/postgresql/data
    networks:
      - backend_net
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 3s
      retries: 5

networks:
  frontend_net:
  backend_net:

volumes:
  pg_data:

secrets:
  db_password:
    file: ./secrets/db_pass.txt
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Compose v2 Specification](./01-Docker-Compose-v2-Specification-and-Architecture.md) | [README](./README.md) | [03 - Dependency Management & Healthchecks](./03-Dependency-Management-and-Healthchecks.md) |
