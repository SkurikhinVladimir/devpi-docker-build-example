# Devpi Docker Build Example

Пример использования Devpi как локального PyPI репозитория для сборки Python-приложений в Docker.

## 🚀 Запуск

1. Запустите Devpi сервер:
```bash
docker-compose -f devpi/docker-compose.yaml up -d
```

2. Запустите приложение:
```bash
docker-compose -f app/docker-compose.yaml up --build
```

## 🔧 Конфигурация

### Docker Compose для Devpi
```yaml
services:
  devpi-server:
    image: jonasal/devpi-server:latest
    container_name: devpi-server
    ports:
      - "3141:3141"
    volumes:
      - ./data-server:/data
    environment:
      - DEVPI_PASSWORD=password
```

### Docker Compose для приложения
```yaml
services:
  my-app:
    image: my-app
    build:
      context: .
      args:
        PIP_EXTRA_INDEX_URL: http://host.docker.internal:3141/root/pypi
        TRUSTED_HOST: host.docker.internal
```

### Dockerfile для приложения
```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

ARG PIP_EXTRA_INDEX_URL
ARG TRUSTED_HOST
RUN pip install --no-cache-dir \
    -r requirements.txt \
    --extra-index-url ${PIP_EXTRA_INDEX_URL} \
    --trusted-host ${TRUSTED_HOST} \
    -v

COPY . .

CMD ["python", "app.py"]
```

## 💡 Преимущества

- Кэширование пакетов PyPI
- Ускорение сборки Docker-образов
- Работа в офлайн-режиме

## 📝 Примечания

- `devpi/data-server` и `devpi/data-client` игнорируются в git
- Веб-интерфейс: http://localhost:3141
