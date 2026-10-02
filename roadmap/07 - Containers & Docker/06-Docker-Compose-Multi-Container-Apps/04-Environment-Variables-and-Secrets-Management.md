# 04 - Environment Variables and Secrets Management

## 1. Variable Precedence Order

Docker Compose evaluates environment variables in a strict hierarchy (highest to lowest):
1. Command-line flags (`docker compose run -e FOO=bar`)
2. Shell environment variables on the host
3. Environment variables defined in the Compose file (`environment:`)
4. Variables sourced via `env_file:` in Compose file
5. Default `.env` file in the project directory

---

## 2. Docker Compose Secrets

```yaml
services:
  backend:
    image: myapp
    secrets:
      - api_key

secrets:
  api_key:
    file: ./secrets/api_key.txt # Mounts into /run/secrets/api_key inside container
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Dependency Management and Healthchecks](./03-Dependency-Management-and-Healthchecks.md) | [Index](../../../README.md) | [05 - Compose Profiles and Multi Environment Overrides →](./05-Compose-Profiles-and-Multi-Environment-Overrides.md) |
