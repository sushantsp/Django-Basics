# Django Tutorial #4 - Database Setup

> [Video link](https://www.youtube.com/watch?v=DZVFgMSyRXI) · The Net Ninja — Complete Django Tutorial

## What we build in this lesson

- Set up the **database** for the Asian Tours Agency app using **models**.
- Same structure as before (new folder: `lesson3_migrate_sqlite3`): venv → install Django → `startproject worldtour` → `startapp asiatoursagency`. (With uv: `uv init` → `uv add django` → `uv run django-admin startproject worldtour` …)

## 1. The Model — your table as a Python class

`models.py` starts **empty** — Django's comment right in the file encourages you to create models there.

```python
from django.db import models

class Tour(models.Model):
    origin_country     = models.CharField(max_length=64)
    destination        = models.CharField(max_length=64)
    number_of_nights   = models.IntegerField()
    price              = models.IntegerField()
```

### Why inherit from `models.Model`?

- `models.Model` provides a **structured way to define the fields and behaviors** of your database objects. That's why it's the parent class of every model.

### Field types used

| Field | Type | Constraint |
|---|---|---|
| `origin_country` | `CharField(max_length=64)` | max 64 **characters** — an SQL constraint |
| `destination` | `CharField(max_length=64)` | max 64 characters |
| `number_of_nights` | `IntegerField()` | whole number |
| `price` | `IntegerField()` | whole number (USD in this lesson) |

### Class = blueprint

- The `Tour` class acts as a **blueprint for the database table** — you manage tour records **without manually handling database operations**.
- Object-oriented: instantiate objects from it, e.g.
  `tour1 = Tour(origin_country='Japan', destination='China', number_of_nights=7, price=1500)`
- The **ORM** (Object-Relational Mapping) handles all **CRUD** operations — Create, Read, Update, Delete — on the table for you.

## 2. Migrations — propagating model changes to the schema

**The problem they solve:** as the app grows (more tours, more fields, changed types), your database must change too. Managing those changes **manually would be a big hassle** — migrations automate it.

> **Migration** = Django's way of **propagating changes made to models** (adding a field, changing a field type…) **into the database schema**.

### Two commands, two jobs

| Command | What it does | Think of it as |
|---|---|---|
| `python manage.py makemigrations` | **Reads** `models.py`, **generates** migration files describing the changes (SQL to run later) | *writes the recipe* |
| `python manage.py migrate` | **Applies** the pending migration files — executes them against the database | *cooks the dish* |

⚠️ Common beginner confusion: `makemigrations` does **not** touch the database — it only creates files in `migrations/`. `migrate` is what actually creates/alters tables.

### Prerequisite (easy to forget!)

Before any of this works, the app must be in `INSTALLED_APPS`:
```python
'asiatoursagency.apps.AsiatoursagencyConfig',
```
> Rule of thumb for future projects: **the first thing you do after creating an app is register it.**

## 3. Inside the generated migration file

`makemigrations` creates `migrations/0001_initial.py`:

- `class Migration(migrations.Migration)` — inherits from `migrations.Migration`
- `initial = True` — marks this as the initial state
- `dependencies = []` — empty for the first migration (later ones list the migrations they build upon)
- `operations = [migrations.CreateModel(...)]` — the list of changes; here: create the `Tour` table with its fields

### The auto `id` field

- We **never declared** an `id` — Django adds it automatically: **auto-generated and auto-incremented**.
- First record → `id = 1`, second → `id = 2`, and so on. Every record gets a unique id.

## 4. Applying: `migrate`

```
Applying migrations... admin, asiatoursagency, contenttypes, sessions...
```

- Django applies **all pending migrations** — including its built-in apps' tables (`admin`, `auth`, `contenttypes`, `sessions`) alongside our new `tour` table.
- Result: the **`tour` table exists** in `db.sqlite3`, records get unique IDs, and fields adhere to the model's constraints (like `max_length`).

## 5. Where is the database? (`settings.py` context)

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

- **SQLite 3** is the default DBMS — a single-file database (`db.sqlite3` in the project root). Zero setup, perfect for development/learning.
- Django's flexibility: swap the engine to **PostgreSQL**, **SQLAlchemy**, or even **MongoDB** (NoSQL) — your model code stays the same; only settings change.
- Inspect it live: `sqlite3 db.sqlite3` then `.tables` / `.schema asiatoursagency_tour`.

> **Convention note:** the table is actually named `asiatoursagency_tour` (app name + model name) — you'll see this naming in the admin and if you inspect the DB.

## Key mental model (extra context)

```
models.py  ──(makemigrations)──►  migrations/0001_initial.py  ──(migrate)──►  db.sqlite3
  class Tour                          recipe (autogenerated)                    tour table
```

- Change `models.py` (new field, new model, changed type) → run `makemigrations` → a **new timestamped** file appears → `migrate` applies it.
- Each migration file is **timestamped** and contains the details of the changes — a version history of your schema you can read (and roll back).

## Next up

Video #5 — interacting with the database directly via the **Django shell**, adding tour records and displaying them.
