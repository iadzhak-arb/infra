# Docker compose
[![CI/CD Pipline](https://github.com/iadzhak-arb/infra/actions/workflows/main.yml/badge.svg)](https://github.com/iadzhak-arb/infra/actions)
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

### CCXT
| Переменная | Описание                                                        | Значение по умолчанию |
|------------|-----------------------------------------------------------------|----------------------|
| `EXCHANGES` | Список бирж (CCXT)                                              | `["bybit", "mexc", "gate", "kucoin", "kucoinfutures"]` |
| `PROXIES` | Список прокси                                                    | `[""]` |

### Consumer
| Переменная | Описание                                                        | Значение по умолчанию |
|------------|-----------------------------------------------------------------|----------------------|
| `QUEUE_ORDERBOOKS` | имя очереди orderbooks                                         | `orderbooks` |

### Publisher
| Переменная | Описание                                                        | Значение по умолчанию |
|------------|-----------------------------------------------------------------|----------------------|
| `QUEUE_GROUPS` | имя очереди groups                                              | `groups` |
| `MIN_LENGTH` | минимальная длина очереди groups для повторной публикации       | `50` |
| `TIMEOUT` | таймаут (сек.) проверки очереди groups для повторной публикации | `10` |

### RabbitMQ
| Переменная | Описание                                                        | Значение по умолчанию |
|------------|-----------------------------------------------------------------|----------------------|
| `RMQ_HOST` | хост RabbitMQ                                                   | `rmq` |
| `RMQ_PORT` | порт RabbitMQ                                                   | `5672` |
| `RMQ_USER` | пользователь RabbitMQ                                           | `guest` |
| `RMQ_PASS` | пароль RabbitMQ                                                 | `guest` |

### Security (JWT)
| Переменная | Описание                                                        | Значение по умолчанию |
|------------|-----------------------------------------------------------------|----------------------|
| `SECRET_KEY` | ключ для подписи JWT                                            | `super_secret_key` |
| `ALGORITHM` | алгоритм JWT                                                    | `HS256` |
| `ALLOWED_HOSTS` | разрешённые хосты                                               | `["localhost","127.0.0.1","192.168.0.100"]` |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | срок действия access-токена (минуты)                            | `15` |
| `REFRESH_TOKEN_EXPIRE_MINUTES` | срок действия refresh-токена (минуты)                           | `10080` |
| `JWT_COOKIE_SECURE` | флаг secure для cookie                                          | `False` |

### Database (Auth)
| Переменная | Описание                          | Значение по умолчанию |
|------------|-----------------------------------|-----------------------|
| `AUTH_DB_TYPE` | тип БД auth  `sqlite \| postgres` | `sqlite` |
| `AUTH_DB_USER` | пользователь БД auth              | `postgres`            |
| `AUTH_DB_PASS` | пароль БД auth                    | `postgres`            |
| `AUTH_DB_HOST` | хост БД auth                      | `auth_db`             |
| `AUTH_DB_PORT` | порт БД auth                      | `5432`                |
| `AUTH_DB_NAME` | имя БД auth                       | `postgres`            |
| `AUTH_SQLITE_PATH` | путь к SQLite файлу auth          | `database.db`         |

### Database (Arb)
| Переменная | Описание                                                        | Значение по умолчанию |
|------------|-----------------------------------------------------------------|-----------------------|
| `ARB_DB_TYPE` | тип БД arb `sqlite \| postgres`                                                     | `sqlite`              |
| `ARB_DB_USER` | пользователь БД arb                                             | `postgres`            |
| `ARB_DB_PASS` | пароль БД arb                                                   | `postgres`            |
| `ARB_DB_HOST` | хост БД arb                                                     | `arb_db`              |
| `ARB_DB_PORT` | порт БД arb                                                     | `5432`                |
| `ARB_DB_NAME` | имя БД arb                                                      | `postgres`            |
| `ARB_SQLITE_PATH` | путь к SQLite файлу arb                                         | `db.sqlite3`          |

  
## Разработка

### Требования

Настройте переменные окружения в файле `.env`.  
Все сервисы находятся в директориях **на одном уровне с `infra/`**:

```
.../
├── arb/
├── auth/
├── frontend/
├── docs/ ⟵ папка с документами использования сайта (cookies.pdf, policy.pdf, rules.pdf)
├── infra/
│   ├── .env  ⟵ создайте файл с переменными окружения
│   ├── .env.example  ⟵ образец файла с переменными окружения
│   └── docker-compose-dev.yaml  ⟵ файл с описанием контейнеров
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

Для запуска платформы выполните:

```bash
docker compose up -d
```

> **Важно:** в директории запуска должен присутствовать файл `.env` с настроенными переменными окружения.

