# Django Tutorial #2 - Setup & Installation

> [Video link](https://www.youtube.com/watch?v=aAACOgAHg90) · The Net Ninja — Complete Django Tutorial

## 1. Install Python

- Download from **python.org** → Downloads section (installer is straightforward).
- Verify in the terminal:
  ```bash
  python --version
  ```

## 2. pip — Python's package manager

- Comes **bundled with Python** — if Python is installed, you have pip.
- Verify:
  ```bash
  pip --version        # e.g. pip 24.0
  pip --help           # lists all commands/flags
  ```
- Most important commands:
  | Command | Purpose |
  |---|---|
  | `pip install <pkg>` | install packages |
  | `pip download <pkg>` | download packages |
  | `pip uninstall <pkg>` | remove packages |
  | `pip freeze` | list installed packages |

## 3. Install Django

- Latest release downloadable from **djangoproject.com** (5.0.4 at time of recording).
- Linux/Mac: `pip install django`
- Windows:
  ```bash
  python -m pip install django==5.0.4
  ```
  The `-m` flag tells Python to **run a module as a script** (here: run pip as the main program).
- Verify:
  ```bash
  django-admin --version
  ```

## 4. Virtual environments (best practice)

- Installing globally **isn't best practice** — instead isolate each project's dependencies from the global file system with a **virtual environment**.
- The instructor uses **pipenv**:
  ```bash
  pip install pipenv        # install pipenv
  pipenv shell              # activate the virtual environment
  pipenv install django     # install Django inside the env
  pip freeze                # verify: django 5.0.4 + sqlparse, typing_extensions, …
  ```
- **Editor:** VS Code (lightweight, versatile, great extensions). Open the integrated terminal with **Terminal → New Terminal** or the shortcut **Ctrl + `**.

## 5. Create the project

```bash
django-admin startproject myproject
```
Creates:
```
myproject/            ← root folder
├── manage.py
└── myproject/        ← inner project package
```

### Files explained

| File | Role |
|---|---|
| `__init__.py` | Empty; its presence marks the directory as a **Python package** |
| `asgi.py` | **Asynchronous Server Gateway Interface** — standard interface between async Python web servers/frameworks and the app. Bridge to async features like **WebSockets / long-lived connections** (chat apps, live updates). Lets Django stay responsive and handle multiple requests at the same time |
| `settings.py` | Project configuration — the most important file |
| `urls.py` | The **URL dispatcher** — maps URLs to view functions/classes (the `urls.py` step of the MVT flow) |
| `wsgi.py` | **Web Server Gateway Interface** — standard interface between web servers and Python web apps/frameworks; entry point for WSGI applications |
| `manage.py` | Command-line utility for every Django project (see below) |

### Inside `settings.py`

- `SECRET_KEY` — **don't share it with anyone**
- `DEBUG = True` — for development
- `INSTALLED_APPS` — register apps here when you create them
- `MIDDLEWARE` — processes incoming HTTP requests & outgoing responses
- `DATABASES` — SQLite 3 by default; engine can be swapped (PostgreSQL, SQLAlchemy, MongoDB for NoSQL) — Django is flexible
- `STATIC_URL` — for static assets: images, CSS, embedded JavaScript

## 6. Create an app

- Django's idea: a **project contains multiple applications** — as the project grows, split it into smaller apps.
```bash
cd myproject
python manage.py startapp myapp
```

### `manage.py` — wrapper around Django's admin tasks

- Creating applications (`startapp`)
- Creating database tables
- Running the development server (`runserver`)
- Creating superusers
- Running tests

## 7. Run the server

```bash
python manage.py runserver
```
- Starts the development server at **http://127.0.0.1:8000**
  - `127.0.0.1` = **localhost** (your own machine), port **8000**
- Visiting it shows the Django **welcome page** ("The install worked successfully!") — proof everything is set up.

## Next up

Video #3 — dive into creating our first application.

> **Note for macOS users:** the video is done on Windows; on macOS use `python3` / `pip3`, and the `pip install django` form directly (no `python -m` needed).
