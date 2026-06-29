# VPNBox

Portfolio project: Telegram bot for selling and managing VPN subscriptions.

The repository demonstrates a production-style bot structure: user flows, inline keyboards, FSM states, admin tools, database models, payment-provider abstractions, background jobs, and integration with a VPN panel.

> Demo status: payment keys and real VPN panel credentials are not included. The project is intended to show implementation logic and architecture.

## What It Shows

- Telegram bot on **aiogram 3**
- Inline keyboards and callback-based navigation
- FSM scenarios for admin and user flows
- PostgreSQL persistence with SQLAlchemy async models
- Alembic migrations
- Admin panel inside Telegram
- Subscription plans, trial flow, profile and connection screens
- Payment-provider architecture for YooKassa, FreeKassa, Platega, CryptoCloud, and Telegram Stars
- Celery tasks for payment checks, subscription expiration, reminders, and VPN health checks
- 3X-UI integration layer for creating and disabling VPN users

## Main User Flows

- `/start` opens the main menu
- Trial activation
- Plan selection
- Payment method selection
- User profile and active subscription view
- VPN connection instructions
- Referral flow
- `/admin` opens admin tools for analytics, plans, users, service settings, and manual subscription issuing

## Tech Stack

- Python 3.11+
- aiogram 3
- SQLAlchemy 2 async
- PostgreSQL
- Alembic
- Redis + Celery
- FastAPI web API module
- 3X-UI API client

## Project Structure

```text
app/
├── bot/
│   ├── handlers/          # User/admin scenarios and FSM flows
│   ├── keyboards.py       # Inline keyboard builders
│   ├── middlewares.py     # DB session, admin and error middlewares
│   └── main.py            # Bot startup
├── db/
│   ├── models.py          # SQLAlchemy models
│   ├── repo.py            # Repository helpers
│   └── engine.py          # Async engine/session
├── services/
│   ├── payments/          # Payment provider interface and implementations
│   ├── client_service.py
│   ├── subscription_service.py
│   └── vpn_service.py
├── vpn/                   # 3X-UI integration
└── worker/                # Celery app and scheduled tasks
```

## Local Setup

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt

cp .env.example .env
# Fill BOT_TOKEN, ADMIN_ID, DATABASE_URL, REDIS_URL and VPN_SERVERS

.venv/bin/alembic upgrade head
.venv/bin/python run_bot.py
```

Example local database URLs:

```env
DATABASE_URL=postgresql+asyncpg://vpnbox:vpnbox@localhost:5432/vpnbox
REDIS_URL=redis://localhost:6379/0
```

## Screencast

Recommended demo script:

1. Start the bot and show the main menu.
2. Open trial activation and plan selection.
3. Show payment method buttons without entering real payment credentials.
4. Open profile and connection instruction screens.
5. Open `/admin` and show analytics, plan management, service settings, and manual subscription issuing.
6. Briefly show the code structure: handlers, FSM states, keyboards, services, and database models.

This gives a clean portfolio demo without needing real payments or a production VPN server.

## Notes

- Real secrets must be stored only in `.env`.
- Payment integrations are implemented as separate provider classes, so they can be enabled when credentials are available.
- The demo can run with mocked or local infrastructure, but production use requires PostgreSQL, Redis, a real bot token, and a configured 3X-UI panel.
