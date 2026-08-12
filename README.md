# Docker compose
> Руководство по запуску платформы [Arb Scanner](https://github.com/iadzhak-arb)  

## Сервисы

| Сервис       | Описание                         | Репозиторий / Образ |
|------------- |----------------------------------|----------------------|
| `traefik`    | Обратный прокси (порт 80)        | traefik:v3.7                     |
| `rmq`        | Брокер сообщений RabbitMQ        | rabbitmq:4.3-alpine                     |
| `scanner`    | Сканер рынков                    | [scanner](https://github.com/iadzhak-arb/scanner) |
| `auth`       | API авторизации                  | [auth](https://github.com/iadzhak-arb/auth) |
| `arb`        | API данных арбитража             | [arb](https://github.com/iadzhak-arb/arb) |
| `arb_broker` | Агреация и сохранение orderbooks |  [arb](https://github.com/iadzhak-arb/arb)             |
| `frontend`   | Клиентский фронтенд              | [frontend](https://github.com/iadzhak-arb/frontend) |

  
## Переменные окружения


| Переменная | Описание                                                        | Значение по умолчанию |
|------------|-----------------------------------------------------------------|----------------------|
| `EXCHANGES` | Список бирж (CCXT)                                              | `["bybit", "mexc"]` |
| `PROXIES` | Список прокси                                                   | `[""]` |
| `QUEUE_ORDERBOOKS` | очередь orderbooks                                              | `orderbooks` |
| `QUEUE_GROUPS` | очередь groups                                                  | `groups` |
| `MIN_LENGTH` | минимальная длина очереди groups для повторной публикации       | `50` |
| `TIMEOUT` | таймаут (сек.) проверки очереди groups для повторной публикации | `10` |
| `RMQ_HOST` | хост RabbitMQ                                                   | `rmq` |
| `RMQ_PORT` | порт RabbitMQ                                                   | `5672` |
| `RMQ_USER` | пользователь RabbitMQ                                           | `guest` |
| `RMQ_PASS` | пароль RabbitMQ                                                 | `guest` |
| `SECRET_KEY` | ключ для подписи JWT                                            | `super_secret_key` |
| `ALGORITHM` | алгоритм JWT                                                    | `HS256` |
| `ALLOWED_HOSTS` | разрешённые хосты                                               | `["localhost","127.0.0.1"]` |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | срок действия access-токена                                     | `15` |
| `REFRESH_TOKEN_EXPIRE_MINUTES` | срок действия refresh-токена                                    | `10080` |
| `JWT_COOKIE_SECURE` | флаг secure для cookie                                          | `False` |
| `AUTH_DB_URL` | URL базы данных auth                                            | `sqlite+aiosqlite:///database.db` |
| `ARB_DB_URL` | URL базы данных arb                                             | `sqlite+aiosqlite:///db.sqlite3` |

  
## Разработка

### Требования

Настройте переменные окружения в файле `.env`.  
Все сервисы находятся в директориях **на одном уровне с `infra/`**:

```
.../
├── arb/
├── auth/
├── frontend/
├── infra/
│   ├── .env  ⟵ создайте файл с переменными окружения
│   ├── .env.example  ⟵ образец файла с переменными окружения
│   └── docker-compose-dev.yaml
└── scanner/
```


### Запуск

Перейдите в директорию `infra/` и выполните:

```bash
docker compose -f docker-compose-dev.yaml up -d
```

Проверьте, что сервисы запустились:

```bash
docker compose -f docker-compose-dev.yaml ps
```

### Остановка

```bash
docker compose -f docker-compose-dev.yaml down
```

  
## Деплой

