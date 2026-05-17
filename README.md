<div align="center">

# DigitalBazar

**Промышленная маркетплейс-платформа для цифровых товаров —**
**лицензии, шаблоны, темы, графика, музыка, шрифты, курсы и электронные книги в одном окне.**

![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-5.x-092E20?style=for-the-badge&logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/DRF-3.15-A30000?style=for-the-badge&logo=django&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-14-000000?style=for-the-badge&logo=next.js&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Celery](https://img.shields.io/badge/Celery-5-37814A?style=for-the-badge&logo=celery&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-Payments-635BFF?style=for-the-badge&logo=stripe&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-Reverse_Proxy-009639?style=for-the-badge&logo=nginx&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

</div>

---

## Содержание

1. [О проекте](#1-о-проекте)
2. [Ключевые возможности](#2-ключевые-возможности)
3. [Технологический стек](#3-технологический-стек)
4. [Структура репозитория](#4-структура-репозитория)
5. [Архитектура и как это работает](#5-архитектура-и-как-это-работает)
6. [Доменная модель (крупными блоками)](#6-доменная-модель-крупными-блоками)
7. [Сервисы в Docker Compose](#7-сервисы-в-docker-compose)
8. [Быстрый старт (локально, Docker)](#8-быстрый-старт-локально-docker)
9. [Основные команды Makefile](#9-основные-команды-makefile)
10. [Ручной запуск frontend и backend](#10-ручной-запуск-frontend-и-backend)
11. [Конфигурация и переменные окружения](#11-конфигурация-и-переменные-окружения)
12. [API, очереди и интеграции](#12-api-очереди-и-интеграции)
13. [Мониторинг и эксплуатация](#13-мониторинг-и-эксплуатация)
14. [CI/CD](#14-cicd)
15. [Безопасность и хранение файлов](#15-безопасность-и-хранение-файлов)
16. [Роли компонентов в продакшене](#16-роли-компонентов-в-продакшене)
17. [Лицензия](#17-лицензия)
18. [Поддержка](#18-поддержка)

---

## 1. О проекте

**DigitalBazar** — это **продуктовый маркетплейс цифровых товаров**, где авторы (selleры) загружают
свои файлы, выставляют цены и условия лицензирования, а покупатели мгновенно получают доступ
к покупке и скачиванию. Платформа берёт на себя оплату через Stripe, генерацию лицензионных
ключей, контроль лимитов скачивания, выплаты продавцам, реферальную систему и аналитику —
всё в одном окне. Система рассчитана на конечных пользователей (B2C), креаторов (B2B-продавцов)
и партнёров (аффилиатов), с возможностью эксплуатации в Docker — от локальной разработки
до production.

### Что это за тип системы

По архитектуре DigitalBazar — **многосервисная распределённая платформа** (не монолит «в одном процессе»):

| Аспект          | Описание                                                                                  |
|-----------------|-------------------------------------------------------------------------------------------|
| Продукт         | B2C/B2B-маркетплейс цифровых товаров с лицензиями, выплатами, рефералкой и аудитом         |
| Архитектура     | Многосервисная: Django REST API, Next.js SSR, Celery workers, Stripe, Nginx               |
| Хранилище       | PostgreSQL (метаданные) + локальный/S3-совместимый сторадж (файлы) + Redis (кэш/брокер)   |
| Каналы доступа  | Веб-интерфейс (Next.js), REST API (DRF), админ-панель Django                              |
| Способ доставки | Docker Compose — единая команда поднимает БД, кэш, бэкенд, фронтенд, воркеры и Nginx       |
| Платежи         | Stripe Checkout + Webhooks + сплит между платформой и продавцом                            |

### Для кого

- **Креаторы и студии**, которые продают цифровые ассеты (UI-киты, темы, треки, шрифты, курсы).
- **Покупатели**, которым нужен мгновенный доступ к лицензированным файлам без посредников.
- **Аффилиаты и медиа**, зарабатывающие на реферальных продажах с прозрачным трекингом.
- **Команды и enterprise**, которым нужен white-label-маркетплейс под своим брендом.

---

## 2. Ключевые возможности

### Каталог и поиск
- Мультикатегорийность: софт, шаблоны, темы, графика, музыка, шрифты, курсы, ebooks.
- Расширенный поиск: фильтры по категории, цене, рейтингу, типу лицензии, формату файла.
- Превью контента: галерея изображений, аудио-семплы, live-демо.
- Версионирование товаров и история обновлений.

### Лицензии и доставка
- Несколько тарифов лицензии на товар: **Personal**, **Commercial**, **Extended**.
- Автогенерация лицензионных ключей (UUID + кастомные паттерны).
- Публичный **License Validation API** для интеграции в сторонние приложения.
- Лимиты на количество скачиваний по каждой лицензии.
- Возможность отзыва (revoke) и переноса лицензии.
- **Подписанные URL** на скачивание со сроком жизни 24 часа.

### Платежи и выплаты
- Stripe Checkout: карты, кошельки, 3-D Secure.
- **Сплит-платёж** между платформой и продавцом, настраиваемая комиссия (по умолчанию 15%).
- Автоматическое расписание выплат продавцам через Celery Beat.
- Workflow возвратов с админским подтверждением и автоматическим Stripe-refund.
- Поддержка расчёта налогов.

### Кабинет продавца
- Real-time аналитика продаж с графиками (день / неделя / месяц / год).
- Метрики по товарам: показы, конверсии, скачивания.
- История доходов и выплат.
- Демография покупателей.

### Аффилиатская программа
- Настраиваемые программы под каждый товар.
- Уникальные реферальные ссылки и cookie-attribution (по умолчанию 30 дней).
- Last-click модель, минимальная комиссия — $1.00.
- Дашборд эффективности и автоматические выплаты.

### Отзывы и репутация
- Отзывы только от верифицированных покупателей.
- Звёздный рейтинг и текстовый отклик.
- Ответы продавца, модерация админом, агрегированный рейтинг.

### Безопасность
- JWT-аутентификация с refresh-токенами (SimpleJWT).
- RBAC: Admin / Seller / Buyer / Affiliate.
- Валидация загружаемых файлов и хуки антивирус-сканирования.
- Rate limiting на API, CORS, CSRF, signed-URL доставка.

---

## 3. Технологический стек

| Слой                  | Технология                              |
|-----------------------|------------------------------------------|
| Frontend              | Next.js 14, React 18, Tailwind CSS       |
| Backend API           | Django 5.x, Django REST Framework        |
| Аутентификация        | JWT (SimpleJWT), refresh-tokens          |
| База данных           | PostgreSQL 16                            |
| Кэш и брокер очередей | Redis 7                                  |
| Очереди задач         | Celery 5 (worker) + Celery Beat (cron)   |
| Платежи               | Stripe API (Checkout, Webhooks, Refunds) |
| Reverse Proxy         | Nginx 1.25                               |
| Контейнеризация       | Docker, Docker Compose                   |
| Хранилище файлов      | Локальные тома / S3-совместимое (AWS S3) |
| Тесты                 | Pytest, Pytest-Django, factory-boy       |

---

## 4. Структура репозитория

```
DigitalBazar/
├── README.md
├── docker-compose.yml
├── .env.example
├── nginx/
│   └── nginx.conf
├── backend/
│   ├── Dockerfile
│   ├── manage.py
│   ├── requirements.txt
│   ├── config/                  # настройки Django, urls, wsgi, celery
│   │   ├── settings/
│   │   │   ├── base.py
│   │   │   ├── development.py
│   │   │   └── production.py
│   │   ├── urls.py
│   │   ├── wsgi.py
│   │   └── celery.py
│   ├── apps/
│   │   ├── accounts/            # пользователи, профили, роли
│   │   ├── products/            # каталог, категории, лицензии
│   │   ├── orders/              # заказы, скачивания, ключи
│   │   ├── payments/            # Stripe, выплаты, возвраты
│   │   ├── affiliates/          # реферальная система
│   │   ├── analytics/           # отчёты и метрики
│   │   └── reviews/             # отзывы и рейтинги
│   ├── middleware/              # кастомные middleware (audit, rate-limit)
│   └── utils/                   # pagination, file_handler, license_generator
└── frontend/
    ├── package.json
    ├── next.config.js
    ├── public/
    └── src/
        ├── app/                 # маршруты Next.js App Router
        │   ├── products/
        │   ├── seller/
        │   ├── dashboard/
        │   ├── cart/
        │   └── auth/
        ├── components/          # layout, products, dashboard, auth
        ├── lib/                 # api-клиент, auth-хелперы
        ├── context/             # AuthContext, CartContext
        └── styles/
```

---

## 5. Архитектура и как это работает

```
                         ┌──────────────────────┐
                         │    Nginx (80/443)    │
                         │   Reverse Proxy +    │
                         │   Static / Media     │
                         └──────────┬───────────┘
                                    │
                ┌───────────────────┼────────────────────┐
                │                                        │
       ┌────────▼────────┐                    ┌──────────▼──────────┐
       │  Next.js (SSR)  │   REST / JSON      │  Django REST API    │
       │  Frontend :3000 │ ─────────────────► │  Backend :8000      │
       └─────────────────┘                    └──────────┬──────────┘
                                                         │
                       ┌─────────────────────────────────┼─────────────────────────────────┐
                       │                                 │                                 │
              ┌────────▼────────┐               ┌────────▼────────┐               ┌────────▼────────┐
              │  PostgreSQL 16  │               │     Redis 7     │               │  Celery Worker  │
              │   Метаданные    │               │  Cache + Broker │               │  + Celery Beat  │
              └─────────────────┘               └────────┬────────┘               └────────┬────────┘
                                                        │                                  │
                                                        └──────────────┬───────────────────┘
                                                                       │
                                                              ┌────────▼────────┐
                                                              │   Stripe API    │
                                                              │  Checkout / WH  │
                                                              └─────────────────┘
```

### Поток данных

1. Пользователь открывает Next.js SSR-фронтенд (страницы каталога, кабинета, чекаут).
2. Фронтенд ходит в **Django REST API** по HTTP/JSON, авторизуется JWT.
3. API инкапсулирует бизнес-логику (товары, заказы, лицензии, выплаты) и пишет в PostgreSQL.
4. Тяжёлые операции — отправка писем, генерация отчётов, обработка выплат, постановка
   возвратов — уходят в **Celery worker** через **Redis-брокер**.
5. **Celery Beat** запускает периодические задачи: расчёт выплат, отчёты, чистка просроченных ссылок.
6. **Stripe Webhooks** прилетают в `/api/v1/payments/webhook/` — API проверяет подпись и
   подтверждает заказы, начисляет аффилиатские комиссии, выдаёт лицензионные ключи.
7. **Nginx** терминирует HTTPS, проксирует `/api/`, `/admin/`, `/static/`, `/media/` и
   отдаёт фронтенд.

---

## 6. Доменная модель (крупными блоками)

| Домен          | Главные сущности                                              | Зона ответственности                          |
|----------------|---------------------------------------------------------------|-----------------------------------------------|
| `accounts`     | User, Profile, Role, SellerApplication                        | Регистрация, KYC продавцов, профиль, RBAC     |
| `products`     | Product, Category, Tag, LicenseTier, ProductVersion, MediaAsset| Каталог, превью, лицензии, версии              |
| `orders`       | Order, OrderItem, LicenseKey, Download                         | Корзина, выдача ключей, история скачиваний    |
| `payments`     | PaymentIntent, Payout, Refund, Commission                      | Stripe, сплит, возвраты, выплаты              |
| `affiliates`   | AffiliateProgram, ReferralLink, Click, Commission              | Реф-ссылки, атрибуция, комиссии               |
| `analytics`    | ProductMetric, SalesReport, DownloadLog                        | Дашборды и отчёты для продавцов и админа       |
| `reviews`      | Review, ReviewReply, ReviewModeration                          | Верифицированные отзывы и ответы              |

Каждый домен — это отдельное Django-app с собственными `models / serializers / views /
urls / services / tasks`. Бизнес-логика, которую недопустимо размазывать по view, живёт
в `services/`, асинхронные операции — в `tasks.py`.

---

## 7. Сервисы в Docker Compose

| Сервис            | Контейнер                         | Порт      | Назначение                                    |
|-------------------|-----------------------------------|-----------|-----------------------------------------------|
| `db`              | `digitalbazar_db` (postgres:16)   | 5432      | Основная БД PostgreSQL                        |
| `redis`           | `digitalbazar_redis` (redis:7)    | 6379      | Кэш Django + брокер Celery                    |
| `backend`         | `digitalbazar_backend`            | 8000      | Django + Gunicorn (REST API, админка)         |
| `celery_worker`   | `digitalbazar_celery_worker`      | —         | Асинхронные задачи (email, payouts, exports)  |
| `celery_beat`     | `digitalbazar_celery_beat`        | —         | Cron-планировщик периодических задач          |
| `frontend`        | `digitalbazar_frontend` (Next.js) | 3000      | SSR + статика клиента                          |
| `nginx`           | `digitalbazar_nginx`              | 80 / 443  | Reverse proxy, TLS, статика, media            |

Тома: `postgres_data`, `redis_data`, `backend_static`, `backend_media` — данные переживают
перезапуски и пересборки контейнеров.

---

## 8. Быстрый старт (локально, Docker)

> **Требования:** Docker 24+ и Docker Compose v2. Опционально — Node.js 18+ и Python 3.12+
> для ручной разработки.

```bash
# 1. Клонировать репозиторий
git clone https://github.com/NodirOdilov/DigitalBazar.git
cd DigitalBazar

# 2. Скопировать переменные окружения и заполнить секреты (Stripe, SECRET_KEY)
cp .env.example .env

# 3. Поднять весь стек
docker compose up -d --build

# 4. Применить миграции и создать суперпользователя
docker compose exec backend python manage.py migrate
docker compose exec backend python manage.py createsuperuser

# 5. (Опционально) загрузить стартовые категории
docker compose exec backend python manage.py loaddata initial_categories
```

После старта будут доступны:

| Адрес                                      | Что там                          |
|---------------------------------------------|-----------------------------------|
| http://localhost:3000                       | Next.js — витрина и кабинет       |
| http://localhost:8000/api/v1/               | Django REST API                   |
| http://localhost:8000/admin/                | Django Admin                      |
| http://localhost                            | Nginx — единая точка входа        |

---

## 9. Основные команды Makefile

> Если в репозитории нет `Makefile`, эти команды можно выполнять напрямую через `docker compose`.

| Команда                | Что делает                                                       |
|------------------------|------------------------------------------------------------------|
| `make up`              | Поднять весь стек в фоне (`docker compose up -d`)                |
| `make down`            | Остановить и удалить контейнеры                                  |
| `make build`           | Пересобрать образы                                               |
| `make logs`            | Хвост логов всех сервисов                                        |
| `make migrate`         | Применить миграции Django                                        |
| `make makemigrations`  | Сгенерировать новые миграции                                     |
| `make superuser`       | Создать суперпользователя                                        |
| `make shell`           | Открыть Django shell                                             |
| `make test`            | Запустить тесты backend (Pytest)                                 |
| `make celery`          | Хвост логов Celery worker и Beat                                 |
| `make collectstatic`   | Собрать статику Django                                           |

---

## 10. Ручной запуск frontend и backend

### Backend (Django + Celery)

```bash
cd backend
python -m venv .venv
# Linux/Mac
source .venv/bin/activate
# Windows (PowerShell)
.venv\Scripts\Activate.ps1

pip install -r requirements.txt

# Запуск с dev-настройками
$env:DJANGO_SETTINGS_MODULE = "config.settings.development"
python manage.py migrate
python manage.py runserver 0.0.0.0:8000

# В отдельных терминалах:
celery -A config worker -l info
celery -A config beat   -l info
```

### Frontend (Next.js)

```bash
cd frontend
npm install
npm run dev
# Production-сборка
npm run build && npm start
```

---

## 11. Конфигурация и переменные окружения

Все секреты и параметры окружения собраны в `.env`. Ключевые переменные:

| Переменная                    | Назначение                                                 |
|-------------------------------|------------------------------------------------------------|
| `SECRET_KEY`                  | Секретный ключ Django                                      |
| `DEBUG`                       | Режим разработки (никогда не `True` в проде)               |
| `ALLOWED_HOSTS`               | Список разрешённых доменов                                 |
| `DATABASE_URL`                | Строка подключения к PostgreSQL                            |
| `REDIS_URL`                   | Строка подключения к Redis (кэш)                           |
| `CELERY_BROKER_URL`           | URL брокера Celery (обычно Redis DB 1)                     |
| `STRIPE_SECRET_KEY`           | Секретный ключ Stripe                                      |
| `STRIPE_PUBLISHABLE_KEY`      | Публичный ключ Stripe (используется и фронтом)             |
| `STRIPE_WEBHOOK_SECRET`       | Секрет подписи webhook-ов Stripe                           |
| `PLATFORM_COMMISSION`         | Комиссия платформы в процентах (по умолчанию `15`)         |
| `CORS_ALLOWED_ORIGINS`        | Список фронтенд-доменов для CORS                            |
| `FRONTEND_URL`                | Базовый URL фронтенда (для письма, ссылок, редиректов)     |
| `AWS_STORAGE_BUCKET`          | S3-бакет для media (опционально)                           |
| `EMAIL_*`                     | SMTP-настройки для транзакционных писем                    |
| `NEXT_PUBLIC_API_URL`         | URL API для фронта                                         |
| `NEXT_PUBLIC_SITE_URL`        | Публичный URL сайта                                        |

---

## 12. API, очереди и интеграции

### Аутентификация

| Метод | Эндпоинт                       | Описание                  |
|-------|--------------------------------|---------------------------|
| POST  | `/api/v1/auth/register/`       | Регистрация пользователя  |
| POST  | `/api/v1/auth/login/`          | Получение JWT-токенов     |
| POST  | `/api/v1/auth/token/refresh/`  | Обновление access-токена  |
| GET   | `/api/v1/auth/me/`             | Текущий профиль           |
| PUT   | `/api/v1/auth/me/`             | Обновление профиля        |

### Каталог

| Метод | Эндпоинт                              | Описание                  |
|-------|---------------------------------------|---------------------------|
| GET   | `/api/v1/products/`                   | Список товаров с фильтрами |
| POST  | `/api/v1/products/`                   | Создание (только продавец) |
| GET   | `/api/v1/products/{slug}/`            | Детали товара             |
| PUT   | `/api/v1/products/{slug}/`            | Обновление (владелец)     |
| DELETE| `/api/v1/products/{slug}/`            | Удаление (владелец)       |
| GET   | `/api/v1/products/categories/`        | Категории                  |
| GET   | `/api/v1/products/{slug}/reviews/`    | Отзывы на товар           |

### Заказы и лицензии

| Метод | Эндпоинт                                          | Описание                     |
|-------|---------------------------------------------------|------------------------------|
| POST  | `/api/v1/orders/checkout/`                        | Создать заказ + PaymentIntent |
| GET   | `/api/v1/orders/`                                 | История заказов              |
| GET   | `/api/v1/orders/{id}/`                            | Деталь заказа                |
| GET   | `/api/v1/orders/{id}/download/{file_id}/`         | Подписанная ссылка на файл   |
| GET   | `/api/v1/orders/licenses/`                        | Мои лицензионные ключи        |

### Платежи

| Метод | Эндпоинт                                | Описание                       |
|-------|-----------------------------------------|--------------------------------|
| POST  | `/api/v1/payments/create-intent/`       | Создать Stripe PaymentIntent   |
| POST  | `/api/v1/payments/webhook/`             | Stripe Webhook (HMAC проверка) |
| GET   | `/api/v1/payments/payouts/`             | История выплат продавца        |
| POST  | `/api/v1/payments/refund-request/`      | Заявка на возврат              |

### Аналитика (продавцу)

| Метод | Эндпоинт                              | Описание                  |
|-------|---------------------------------------|---------------------------|
| GET   | `/api/v1/analytics/dashboard/`        | Сводка кабинета           |
| GET   | `/api/v1/analytics/sales/`            | Продажи во времени        |
| GET   | `/api/v1/analytics/products/`         | Топ-товары                |
| GET   | `/api/v1/analytics/downloads/`        | Логи скачиваний           |

### Аффилиаты

| Метод | Эндпоинт                                | Описание                        |
|-------|-----------------------------------------|---------------------------------|
| GET   | `/api/v1/affiliates/programs/`          | Список программ                  |
| POST  | `/api/v1/affiliates/links/`             | Создать реферальную ссылку      |
| GET   | `/api/v1/affiliates/commissions/`       | История комиссий                 |
| GET   | `/api/v1/affiliates/stats/`             | Статистика конверсий             |

### Очереди (Celery)

| Очередь        | Что делает                                                         |
|----------------|--------------------------------------------------------------------|
| `default`      | Email-уведомления, генерация лицензий, постобработка заказов        |
| `payments`     | Stripe payouts, ретрай webhooks, подтверждение возвратов            |
| `analytics`    | Пересчёт дашбордов, агрегирование метрик, ежедневные отчёты         |
| `housekeeping` | Чистка просроченных подписанных URL, ротация логов, бэкап БД        |

---

## 13. Мониторинг и эксплуатация

- **Логи**: каждый сервис пишет в `stdout`, `docker compose logs -f <service>`
  даёт сквозной просмотр.
- **Healthchecks**: PostgreSQL и Redis имеют встроенные healthchecks в `docker-compose.yml`,
  backend стартует только когда они `healthy`.
- **Метрики**: легко подключается Prometheus exporter + Grafana (PostgreSQL, Redis,
  django-prometheus).
- **Sentry**: backend подготовлен под `SENTRY_DSN` — все исключения и performance-traces
  улетают в Sentry.
- **Резервное копирование**: рекомендуется `pg_dump` по cron + снапшот тома `backend_media`
  (или нативный S3-versioning, если хранение вынесено в S3).

---

## 14. CI/CD

Рекомендуемый пайплайн (GitHub Actions / GitLab CI):

1. **Lint** — `ruff`, `eslint`, `prettier`, `stylelint`.
2. **Tests** — `pytest -q` для backend, `npm test` для frontend.
3. **Build** — собрать docker-образы `backend` и `frontend`, проставить тэг по SHA.
4. **Push** — отправить образы в реестр (GHCR / Docker Hub / приватный registry).
5. **Deploy** — выкатить обновлённый stack на сервер (`docker compose pull && up -d`)
   или через Helm/ArgoCD в Kubernetes.
6. **Smoke-tests** — пройтись по `/api/v1/health/`, `/api/v1/products/?limit=1`.

---

## 15. Безопасность и хранение файлов

- **JWT + refresh-tokens**, ротация refresh-токена при использовании.
- **RBAC** на уровне DRF-permission-классов: `IsAdmin`, `IsSeller`, `IsBuyer`, `IsAffiliate`.
- **Подписанные URL** на скачивание — токен с временем жизни 24 часа, без статичного линка
  на файл.
- **Лимиты скачиваний** на лицензию, IP-логи для антифрод-анализа.
- **Антивирус-хук** при загрузке файлов продавцом (можно подключить ClamAV / VirusTotal).
- **CSRF** для админки, **CORS** строго по `CORS_ALLOWED_ORIGINS`.
- **Rate limiting** через `django-ratelimit` или DRF throttling, отдельные правила для
  auth, checkout, webhook.
- **Stripe Webhook signature** проверяется в обязательном порядке — никаких «голых» POST.
- **Секреты** только в `.env` / секрет-менеджере, никогда не в репозитории.

---

## 16. Роли компонентов в продакшене

| Компонент        | Зачем нужен в проде                                                  |
|------------------|----------------------------------------------------------------------|
| **Nginx**        | TLS-терминация, маршрутизация, отдача статики/media, защита от 4xx/5xx |
| **Django + Gunicorn** | REST API, бизнес-логика, админка, валидация платежей             |
| **Next.js (SSR)** | SEO-готовый рендер витрины, кабинеты продавца и покупателя           |
| **PostgreSQL**   | Транзакционное хранилище заказов, лицензий, профилей и аналитики      |
| **Redis**        | Кэш Django + брокер очередей Celery + сессии скачивания               |
| **Celery worker**| Фоновая обработка тяжёлых задач, чтобы API оставался быстрым          |
| **Celery beat**  | Периодические задачи: выплаты, отчёты, чистки                         |
| **Stripe**       | Платежи, выплаты, возвраты, защита от чарджбэков                       |
| **S3 / volume**  | Долговечное хранение бинарных файлов товаров                          |

---

## 17. Лицензия

Проект распространяется по лицензии **MIT** — можно свободно использовать, модифицировать
и встраивать в коммерческие продукты при условии сохранения уведомления об авторстве.

---

## 18. Поддержка

- **Issues**: [GitHub Issues](https://github.com/NodirOdilov/DigitalBazar/issues) — баги и фич-реквесты.
- **Discussions**: вопросы по архитектуре и интеграции.
- **Email**: для коммерческих и enterprise-обращений.

<div align="center">

**DigitalBazar** — продаём цифровое красиво, безопасно и в масштабе.

</div>
