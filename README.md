# FlashCraft

A multi-user flashcard & quiz web application. Organize knowledge into
Domains → Topics → Questions, merge topics into a quiz pool, and self-grade
your way through sequential or random quiz sessions.

See `FlashCraft_Design_Document.docx` for the full design process (scope,
stack decision, data model, URL/view contract).

## Stack

- Django (built-in auth, server-rendered templates)
- HTMX for dynamic interactivity
- Bootstrap 5 for styling
- PostgreSQL (production) / SQLite (local dev, default fallback)
- `ruff` for linting + formatting

## Local setup (without Docker)

```bash
# 1. Clone and enter the repo
git clone <repo-url>
cd flashcraft

# 2. Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements-dev.txt

# 4. Copy the env template and fill in a real SECRET_KEY
cp .env.example .env
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
# paste the output into .env as SECRET_KEY=...

# 5. Run migrations (uses SQLite automatically — no DATABASE_URL needed)
python manage.py migrate

# 6. Create an admin user (optional, for /admin/)
python manage.py createsuperuser

# 7. Run the dev server
python manage.py runserver
```

Visit `http://127.0.0.1:8000/`.

## Local setup (with Docker — app + Postgres)

```bash
docker compose up --build
```

This runs the app against Postgres (not SQLite) for closer parity with
production. Visit `http://127.0.0.1:8000/`.

Run management commands inside the container, e.g.:

```bash
docker compose exec web python manage.py migrate
docker compose exec web python manage.py createsuperuser
```

## Linting & formatting

```bash
ruff check .            # lint
ruff check --fix .      # lint, auto-fixing what's safe
ruff format .           # format
ruff format --check .   # check formatting without changing files
```

## Running tests

```bash
python manage.py test
```

## Environment variables

See `.env.example` for the full list. Key ones:

| Variable | Local default | Production |
|---|---|---|
| `SECRET_KEY` | dev placeholder | required, kept secret |
| `DEBUG` | `True` | `False` |
| `DATABASE_URL` | unset (falls back to SQLite) | Postgres connection string |
| `ALLOWED_HOSTS` | `localhost,127.0.0.1` | your real domain(s) |

## Project structure