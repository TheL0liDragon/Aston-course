# Task 6

## Репозитории

- **Backend:** https://github.com/TheL0liDragon/kanban-backend
- **Frontend:** https://github.com/TheL0liDragon/kanban-frontend
- **Этот репозиторий (основной):** https://github.com/TheL0liDragon/Aston-course

## Что в проекте

Демо-приложение Kanban состоит из трёх частей:

| Компонент | Технология | Что делает |
|---|---|---|
| Backend | Java 8, Spring Boot 2.1.6, Maven | REST API, миграции БД через Liquibase |
| Frontend | Angular 7.3.9, nginx | Веб-интерфейс, проксирует API на backend |
| База данных | PostgreSQL 13 | Хранит данные |

Backend и frontend **уже собираются в Docker-образы** через свои `Dockerfile` (multi-stage).

## Сборка образов вручную

### Backend

```bash
cd kanban-backend
docker build -t kanban-backend:local .
```

### Frontend

```bash
cd kanban-frontend
docker build -t kanban-frontend:local .
```

### Запуск через docker-compose

```bash
docker compose up -d
```
