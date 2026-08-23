# Django Tutorial #1 - Introduction

> [Video link](https://www.youtube.com/watch?v=3EzKBFc9_MQ) · The Net Ninja — Complete Django Tutorial (host: Amir)

## Course overview

- Beginner → advanced Django series.
- Ends with a real-life application: an **inventory management system**
  - Add products: name, SKU, price, quantity, supplier
  - Product list page with ID, name, SKU, price, quantity, supplier + actions
  - Update / delete products (with a delete confirmation screen)
  - Homepage displaying all products
- Uses **function-based views** and clean, simple styles.

## Prerequisites

- Good (intermediate) **Python** understanding
- Familiarity with **HTML & CSS**
- Net Ninja also has Python and HTML/CSS crash courses to fill gaps

## What is Django?

- A **Python-based web framework** designed for **rapid development** of efficient web applications.
- "**Batteries included**" framework — built-in features for many aspects of web development:
  - Django **admin interface**
  - Default database management system: **SQLite 3**
  - …and more

## Why Django (over other frameworks)?

| Reason | Detail |
|---|---|
| **Rapid development** | Fully-fledged web apps in a short time |
| **Database flexibility** | SQLite 3 by default, easy to switch to others (e.g. PostgreSQL) |
| **Built-in admin interface** | Simplifies website management tasks |
| **Extensive ecosystem** | Vast collection of packages for extra functionality |

## MVT architecture

Django follows the **Model–View–Template** architecture, separating app logic into three components:

1. **Model** — the data structure of your application (essentially your database)
   - Defines the schema of database tables
   - Encapsulates the logic for interacting with the database
2. **View** — a Python function (or class)
   - Receives **HTTP requests**, returns **HTTP responses**
   - Processes requests, interacts with models, prepares data for rendering
3. **Template** — HTML files defining the UI structure
   - Contain **placeholders** and **template tags** replaced with dynamic content when rendered
   - Uses the **Django Template Language (DTL)** — very similar to **Jinja 2** in Flask

### Request flow (how MVT works)

```
User/browser
   │  HTTP request (view page, submit form, ...)
   ▼
View (function/class)
   │  processes request ──► Model (via ORM)
   │                             │
   │  prepares data ◄────────────┘
   ▼
Template (DTL placeholders + tags → dynamic HTML)
   │
   ▼
View returns HTTP response → browser renders the page
```

### The ORM (Object-Relational Mapping)

- Lets you interact with the database using **Python objects** instead of writing raw SQL.
- You define database structure as Python classes (**models**):
  - Model class = database **table**
  - Class attribute = **column** in that table
- Views use the ORM for all CRUD: **retrieving, creating, updating, deleting** records.

## Next up

Video #2 — set up the environment, install Django, create a first Django project.
