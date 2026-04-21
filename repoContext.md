# Spendly — Expense Tracker: Repository Context

## Tech Stack

**Backend:**
- Python 3 with Flask 3.1.3
- Werkzeug 3.1.6 (WSGI utilities, password hashing)
- SQLite (built-in sqlite3, file-based database)

**Frontend:**
- HTML with Jinja2 templating
- Vanilla CSS (custom stylesheets)
- Vanilla JavaScript (no frameworks)

**Testing:**
- pytest 8.3.5
- pytest-flask 1.3.0

---

## Project Structure

```
expense-tracker/
├── app.py                      # Main Flask application, all routes
├── database/
│   ├── __init__.py             # Package initialization
│   └── db.py                   # DB helpers (placeholder — to be implemented)
├── templates/
│   ├── base.html               # Master layout (nav, footer, blocks)
│   ├── landing.html            # Public homepage with hero/features
│   ├── login.html              # Login form
│   ├── register.html           # Registration form
│   ├── terms.html              # Terms and Conditions page
│   └── privacy.html            # Privacy Policy page
├── static/
│   ├── css/style.css           # All application styles (~720 lines)
│   └── js/main.js              # Client-side JS (placeholder)
├── requirements.txt            # Python dependencies
└── venv/                       # Python virtual environment (not committed)
```

---

## What's Built

- Landing page with hero section, feature cards, and YouTube modal CTA
- Register page (UI only — no backend logic yet)
- Login page (UI only — no backend logic yet)
- Terms of Service page
- Privacy Policy page
- Responsive design (breakpoints at 900px, 600px)

---

## What's Not Built Yet (Intentional Placeholders)

These are left for incremental step-by-step implementation:

| Feature | Status |
|---|---|
| Auth logic (register, login, logout) | Route stubs only |
| Database schema & helpers | Comments only in `database/db.py` |
| Expense CRUD (add, edit, delete) | Routes defined, bodies empty |
| Dashboard / expense list view | Not created |
| User profile page | Not created |
| Spending insights / analytics | Not created |
| Budget tracking | Not created |

---

## Key Files

| File | Purpose |
|---|---|
| `app.py` | Flask app entry point, all route definitions, runs on port 5001 in debug mode |
| `database/db.py` | Will hold `get_db()`, `init_db()`, `seed_db()` helpers for SQLite |
| `templates/base.html` | Parent template — sticky navbar, footer, block slots for all pages |
| `static/css/style.css` | Full design system: CSS variables, components, responsive layout |
| `static/js/main.js` | Client-side JS placeholder |

---

## Routes

| Route | Status |
|---|---|
| `/` | Implemented — landing page |
| `/register` | Implemented — registration form (UI only) |
| `/login` | Implemented — login form (UI only) |
| `/terms` | Implemented — terms page |
| `/privacy` | Implemented — privacy policy page |
| `/logout` | Stub — not implemented |
| `/profile` | Stub — not implemented |
| `/expenses/add` | Stub — not implemented |
| `/expenses/<id>/edit` | Stub — not implemented |
| `/expenses/<id>/delete` | Stub — not implemented |

---

## Database (Planned Schema)

- **Users:** id, name, email, password_hash
- **Expenses:** id, user_id, category, amount, date, description
- Password hashing via `werkzeug.security.generate_password_hash` / `check_password_hash`
- Session management via Flask's built-in sessions

---

## Design System

**Colors (CSS variables):**
- `--ink`: #0f0f0f (text)
- `--paper`: #f7f6f3 (background)
- `--accent`: #1a472a (primary green)
- `--accent-2`: #c17f24 (orange highlight)
- `--danger`: #c0392b (errors/destructive actions)

**Typography:**
- Headings: DM Serif Display (Google Fonts)
- Body: DM Sans (Google Fonts)

**Branding:**
- App name: Spendly
- Icon: `◈`
- Currency: Indian Rupees (₹)

---

## Architecture Notes

- **Teaching/boilerplate project** — scaffold is complete, backend logic is left as incremental steps
- Single-file Flask app (`app.py`) — no blueprints or application factory
- Template inheritance — all pages extend `base.html`
- No CSS framework (no Bootstrap/Tailwind) — hand-written CSS with semantic variables
- No JS framework — vanilla JS only
