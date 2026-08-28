# Django Tutorial #5 - Adding Database Records

> [Video link](https://www.youtube.com/watch?v=qQPKqClSDbg) · The Net Ninja — Complete Django Tutorial

## What we do in this lesson

- Add records to the `tour` table via the **Django shell** — modifying the database "without much hassle".
- Meet **IPython** first (the shell runs on it), then add a `__str__` method for readable output.

## 1. IPython — the enhanced Python interpreter

- Plain `python` → the normal interpreter (`1 + 1`, define functions, print…).
- `ipython` (Linux/Mac: `ipython3`) → **"enhanced interactive Python"**:

| Feature | Example |
|---|---|
| Numbered prompts | `In [1]:` / `Out [1]:` — input vs output history |
| **Tab completion** | type `def` + `(` + **Tab** → shows classes/methods available |
| Shell commands | `ls` works (Unix); `dir` doesn't — it reports `dir` is a function |
| Run other languages | Java, JavaScript, SQL snippets right in the session |
| **Magic functions** | special commands starting with `%` — enhanced features not in standard Python |

- Magic example — **`%time`** measures execution time of a single command:
  ```
  In [1]: %time sum(range(10_000_000))
  CPU times: total 0, 2 ms …  Out[1]: 49999995000000
  ```
- Exit with **Ctrl + D**. More: ipython.org.

## 2. The Django shell

```bash
python manage.py shell     # (uv: uv run python manage.py shell)
```

- Essentially **IPython, but tied to your Django application** — it loads your settings, apps and models so you can query/modify the DB directly.
- Works from the VS Code integrated terminal or any editor's terminal (Vim, Atom, Sublime…) — just be in the project folder with the venv active.

## 3. Creating records

### Import the model, instantiate an object

```python
from asiatoursagency.models import Tour

t1 = Tour(origin_country='Japan', destination='China', number_of_nights=10, price=1500)
```

- Pure **OOP**: `Tour` is the blueprint; each instance = **one record (row)** in the table.
- Attributes are accessible immediately: `t1.origin_country` → `Japan`, `t1.price` → `1500`, …

### Save it — the DB "commit"

```python
t1.save()
```

- **Analogous to the `COMMIT` statement in MySQL** — tells the database you're happy with the changes and want them persisted.
- Instantiating does **not** write to the DB; `save()` does.

## 4. The `__str__` method — readable objects

Problem: typing `t1` alone outputs `<Tour: Tour object (1)>` — tells us nothing.

Fix — define `__str__` **inside the Tour class** (watch indentation):

```python
class Tour(models.Model):
    origin_country     = models.CharField(max_length=64)
    destination        = models.CharField(max_length=64)
    number_of_nights   = models.IntegerField()
    price              = models.IntegerField()

    def __str__(self):                       # string representation of the tours
        return f"Tour (id: {self.id}): from {self.origin_country} to {self.destination}, {self.number_of_nights} nights costs ${self.price}"
```

- `__str__` is a **special Python method** — its whole purpose is a nice string representation of an object.
- There's also `__repr__` (both work); the instructor prefers `__str__`.
- Note `{self.id}` — **we never declared `id`**; Django adds and auto-increments it on save.

> ⚠️ After editing models.py, **exit the shell and re-enter** (Ctrl+D → `python manage.py shell`) — changes are not applied to a running session.

Now: `t1` → `Tour (id: 1): from Japan to China, 10 nights costs $1500` ✓

## 5. The second tour — and the `id=None` lesson

```python
t2 = Tour(origin_country='Vietnam', destination='South Korea', number_of_nights=15, price=20500)
```

- Displaying `t2` **before saving** shows **`id: None`** (deliberate teaching moment!) —
  unsaved objects have no ID because the database hasn't seen them.
- After `t2.save()` → `t2` displays **`id: 2`** — Django assigned and incremented the ID **at save time**.

### Object state recap

| Action | in memory? | in DB? | id? |
|---|---|---|---|
| `t2 = Tour(...)` | ✓ | ✗ | `None` |
| `t2.save()` | ✓ | ✓ | `2` |

## Key takeaways

- The shell = IPython + your Django app context.
- Instance = row; `.save()` = SQL COMMIT; ID assigned on save, not on creation.
- Always add a `__str__` to your models — the admin dashboard (later lesson) uses it too.

## Next up

Video #6 — rendering all these records on a web page (templates + the MVT loop completed).
