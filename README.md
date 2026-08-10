# 🚀 Запуск через Docker Compose

> **Это режим разработки** — всё настроено для быстрой отладки и hot-reload.  
> Не используется в production.

---

## 📋 Что нужно перед запуском

### 1. Создайте файл `.env`

Клонированный `.env.example` в директорию **`infra/`** — там же, где лежит `docker-compose-dev.yaml`:

```bash
cd infra
cp .env.example .env
```

Заполните переменные окружения, которые потребуются сервисам.

### 2. Убедитесь, что репозитории сервисов рядом

Все сервисы находятся в директориях **на одном уровне с `infra/`**:

```
.../
├── arb/
├── auth/
├── frontend/
├── infra/
│   ├── .env
│   └── docker-compose-dev.yaml
└── scanner/
```

Если каких-то репозиториев ещё нет — клонируйте их:

| Сервис     | Репозиторий |
|------------|-------------|
| **arb**    | [arb](https://github.com/iadzhak-arb/arb) |
| **auth**   | [auth](https://github.com/iadzhak-arb/auth) |
| **frontend** | [frontend](https://github.com/iadzhak-arb/frontend) |
| **scanner** | [scanner](https://github.com/iadzhak-arb/scanner) |

---

## ▶️ Запуск

Перейдите в директорию `infra/` и выполните:

```bash
docker compose -f docker-compose-dev.yaml up -d
```

Сервисы поднимутся в фоновом режиме.

### Проверьте, что всё работает

```bash
docker compose -f docker-compose-dev.yaml ps
```

### Откройте в браузере

- 🌐 **Сайт:** `http://localhost`
- 📖 **Документация arb (арбитраж):** `http://localhost/api/arb/docs`
- 🔐 **Документация auth (авторизация):** `http://localhost/api/auth/docs`

---

## 🛑 Остановка

Чтобы остановить сервисы, вернувшись в `infra/`:

```bash
docker compose -f docker-compose-dev.yaml down
```

---

## 🗂️ Сервисы

| Сервис       | Описание                    |
|------------- |-----------------------------|
| `traefik`   | Обратный прокси (порт 80)   |
| `rmq`       | RabbitMQ                    |
| `scanner`   | Сканер                      |
| `auth`      | API авторизации             |
| `arb`       | Основной API                |
| `arb_broker`| Потребитель сообщений       |
| `frontend`  | Vite dev server             |

---
