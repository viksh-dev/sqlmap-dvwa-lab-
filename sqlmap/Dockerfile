# Собираем sqlmap из официального репозитория, а не берём случайный
# сторонний образ с Docker Hub — так надёжнее и прозрачнее для портфолио:
# видно ровно то, что внутри, и версия всегда актуальная на момент сборки.

FROM python:3.11-slim

RUN apt-get update && \
    apt-get install -y --no-install-recommends git ca-certificates && \
    rm -rf /var/lib/apt/lists/*

RUN git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git /opt/sqlmap

WORKDIR /opt/sqlmap

RUN chmod +x sqlmap.py && \
    ln -s /opt/sqlmap/sqlmap.py /usr/local/bin/sqlmap

# Контейнер остаётся "живым" через entrypoint в docker-compose.yml (sleep infinity),
# команды даём вручную через docker compose exec, чтобы удобно перебирать разные запуски.
