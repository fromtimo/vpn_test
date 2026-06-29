# VPNBox

Портфолио-проект: Telegram-бот для продажи и управления VPN-подписками.

Репозиторий показывает, как можно собрать бота с продуманной структурой: пользовательские сценарии, inline-кнопки, FSM-состояния, админ-панель, модели БД, слой платежных провайдеров, фоновые задачи и интеграцию с VPN-панелью.

> Статус демо: реальные платежные ключи и доступы к VPN-панели не включены. Проект предназначен для демонстрации логики, архитектуры и качества реализации.

## Что Реализовано

- Telegram-бот на **aiogram 3**
- Inline-кнопки и навигация через callback-запросы
- FSM-сценарии для пользовательских и админских действий
- PostgreSQL + асинхронный SQLAlchemy 2
- Alembic-миграции
- Админ-панель прямо внутри Telegram
- Тарифы, пробный период, профиль пользователя и экран подключения
- Архитектура платежных провайдеров: YooKassa, FreeKassa, Platega, CryptoCloud и Telegram Stars
- Celery-задачи для проверки платежей, истечения подписок, напоминаний и healthcheck VPN-серверов
- Слой интеграции с 3X-UI для создания и отключения VPN-пользователей

## Основные Сценарии

- `/start` открывает главное меню
- Активация пробного периода
- Выбор тарифа
- Выбор способа оплаты
- Просмотр профиля и активной подписки
- Инструкции по подключению к VPN
- Реферальный сценарий
- `/admin` открывает админ-панель: аналитика, тарифы, пользователи, настройки сервиса и ручная выдача подписки

## Стек

- Python 3.11+
- aiogram 3
- SQLAlchemy 2 async
- PostgreSQL
- Alembic
- Redis + Celery
- FastAPI-модуль для web API
- 3X-UI API client

## Структура Проекта

```text
app/
├── bot/
│   ├── handlers/          # Пользовательские и админские сценарии, FSM
│   ├── keyboards.py       # Inline-клавиатуры
│   ├── middlewares.py     # DB session, admin и error middleware
│   └── main.py            # Запуск бота
├── db/
│   ├── models.py          # SQLAlchemy-модели
│   ├── repo.py            # Репозитории для работы с БД
│   └── engine.py          # Async engine/session
├── services/
│   ├── payments/          # Интерфейс и реализации платежных провайдеров
│   ├── client_service.py
│   ├── subscription_service.py
│   └── vpn_service.py
├── vpn/                   # Интеграция с 3X-UI
└── worker/                # Celery app и фоновые задачи
```

## Локальный Запуск

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt

cp .env.example .env
# Заполнить BOT_TOKEN, ADMIN_ID, DATABASE_URL, REDIS_URL и VPN_SERVERS

.venv/bin/alembic upgrade head
.venv/bin/python run_bot.py
```

Пример локальных URL для БД и Redis:

```env
DATABASE_URL=postgresql+asyncpg://vpnbox:vpnbox@localhost:5432/vpnbox
REDIS_URL=redis://localhost:6379/0
```
