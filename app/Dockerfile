# Используем базовый образ Python
FROM python:3.9-slim

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем зависимости проекта
COPY requirements.txt .

# Устанавливаем зависимости
ARG PIP_EXTRA_INDEX_URL
ARG TRUSTED_HOST
RUN pip install --no-cache-dir \
    -r requirements.txt \
    --extra-index-url ${PIP_EXTRA_INDEX_URL} \
    --trusted-host ${TRUSTED_HOST} \
    -v

# Копируем остальные файлы проекта
COPY . .

# Команда, которая будет запущена при старте контейнера
CMD ["python", "app.py"]