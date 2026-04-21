# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

## Project Context

**Spendly** is a personal expense tracker web app targeting Indian users (currency: ₹). It is built as a teaching/boilerplate project — the frontend scaffold (pages, design system, routing skeleton) is complete, and the backend logic (auth, database, CRUD) is left as incremental, step-by-step student exercises. The stack is intentionally minimal: Flask + SQLite + vanilla HTML/CSS/JS, no ORMs or frontend frameworks. 

---

## Architecture & Directory Structure

```
expense-tracker/
├── app.py                  # Flask app entry point — all routes live here, runs on port 5001
├── database/
│   ├── __init__.py
│   └── db.py               # Stub for get_db(), init_db(), seed_db() — to be implemented
├── templates/
│   ├── base.html           # Master layout: navbar, footer, Jinja2 block slots
│   ├── landing.html        # Public homepage (hero, features, YouTube modal)
│   ├── login.html          # Login form (UI complete, backend stub)
│   ├── register.html       # Register form (UI complete, backend stub)
│   ├── terms.html          # Terms & Conditions
│   └── privacy.html        # Privacy Policy
├── static/
│   ├── css/style.css       # Full design system — CSS variables, all component styles (~720 lines)
│   └── js/main.js          # Client-side JS placeholder
└── requirements.txt
```

**Data flow:** Browser → Flask route in `app.py` → Jinja2 template rendered server-side → HTML response. There is no REST/JSON API layer; forms POST directly to Flask routes. Sessions are managed by Flask's built-in session mechanism.

**Database:** SQLite accessed via Python's `sqlite3`. `database/db.py` will expose three helpers:
- `get_db()` — returns a connection with `row_factory = sqlite3.Row` and `PRAGMA foreign_keys = ON`
- `init_db()` — creates tables with `CREATE TABLE IF NOT EXISTS`
- `seed_db()` — inserts dev sample data

**Planned schema:**
- `users(id, name, email, password_hash)`
- `expenses(id, user_id, category, amount, date, description)`

---

## Coding Style & Conventions

**Python / Flask**
- Single-file app pattern — all routes stay in `app.py` unless blueprints are explicitly introduced
- Route functions are named after the resource they serve (e.g., `add_expense`, `edit_expense`)
- Group routes with `# --- section ---` comment banners (see existing style in `app.py`)
- Use `render_template()` for all page responses; never return raw HTML strings in production routes
- Stub routes return plain strings like `"Feature — coming in Step N"` as temporary placeholders
- Use `werkzeug.security.generate_password_hash` / `check_password_hash` for all passwords — never store plaintext

**HTML / Jinja2**
- Every page template must `{% extends "base.html" %}` and fill `{% block title %}`, `{% block content %}`
- Use `{% block scripts %}` for page-specific JS, `{% block head %}` for page-specific CSS/meta
- Always use `url_for()` for internal links and static asset references — never hardcode paths
- Footer links are the only exception (use `/terms`, `/privacy` direct paths, matching existing style)

**CSS**
- All new styles go in `static/css/style.css` — no inline styles, no separate per-page files
- Use existing CSS custom properties exclusively; never introduce raw hex values in component styles
- Section headers follow the `/* --- Section Name --- */` banner style used throughout the file
- Responsive rules go at the bottom of the file inside `@media (max-width: 900px)` / `@media (max-width: 600px)` blocks

**JavaScript**
- Vanilla JS only — no libraries, no `import`/`export` (loaded as plain script)
- All JS lives in `static/js/main.js` unless a page needs isolated logic via `{% block scripts %}`

---

## Libraries & Frameworks

| Dependency | Version | Purpose |
|---|---|---|
| Flask | 3.1.3 | Web framework — routing, templating, sessions, request/response handling |
| Werkzeug | 3.1.6 | WSGI utilities, `generate_password_hash` / `check_password_hash` |
| pytest | 8.3.5 | Test runner |
| pytest-flask | 1.3.0 | Flask test client fixtures and app context helpers |
| Jinja2 | (via Flask) | Server-side HTML templating with template inheritance |
| sqlite3 | (stdlib) | Built-in Python SQLite driver — no ORM |
| DM Serif Display / DM Sans | (Google Fonts) | Display and body typefaces loaded via CDN |

---

## Commands

```bash
# Activate virtual environment (required before anything else)
source venv/bin/activate

# Install / sync dependencies
pip install -r requirements.txt

# Run development server (http://localhost:5001, debug + auto-reload)
python app.py

# Run all tests
pytest

# Run a single test file
pytest tests/test_auth.py

# Run a single test by name
pytest -k "test_login_valid_user"

# Run tests with stdout output visible
pytest -s
```

---

## Critical Rules

1. **Never store plain-text passwords.** Always use `werkzeug.security.generate_password_hash` on write and `check_password_hash` on verify.

2. **Always enable foreign keys in SQLite.** Every connection returned by `get_db()` must run `PRAGMA foreign_keys = ON` immediately after opening.

3. **Use `url_for()` for all internal links and static assets.** Hardcoded paths break when the app is mounted at a sub-path.

4. **All templates must extend `base.html`.** Standalone HTML files will lack the navbar, footer, and design system stylesheet.

5. **Keep all routes in `app.py`.** Do not introduce blueprints or split routes across files unless explicitly planned — this is a single-file teaching app.

6. **Do not commit the database file or `.env`.** `expense_tracker.db` and `.env` are in `.gitignore`; secrets and DB state must never enter version control.

7. **Stub routes return strings, not templates.** Until a feature is implemented, placeholder routes return `"Feature — coming in Step N"` — not a 404 or an empty template — so the app stays navigable during development.
