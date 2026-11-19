# Stage 1: Build dependencies
FROM python:3.12-slim AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir --prefix=/install -r requirements.txt


# Stage 2: Runtime
FROM python:3.12-slim

WORKDIR /app

ENV PYTHONUNBUFFERED=1
ENV PYTHONPATH=/app

COPY --from=builder /install /usr/local

COPY . .

RUN groupadd --system app && \
    useradd --system --gid app app

USER app

EXPOSE 8000

CMD ["gunicorn", "media_service.wsgi:application", "--bind", "0.0.0.0:8000"]