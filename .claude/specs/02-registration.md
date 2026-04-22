# Spec: Registration

## Overview
Allow new visitors to create a Spendly account by submitting their name, email, and password via the existing registration form. The `POST /register` route validates input server-side, rejects duplicate emails, hashes the password with werkzeug, inserts the new user into the `users` table, and redirects to `/login` on success. On any validation failure the form is re-rendered with a clear inline error. This is the gateway to all authenticated features in the Spendly roadmap.

## Depends on
- Step 01 — Database Setup: `get_db()` and the `users` table must be implemented and working.

## Routes
- `GET /register` — render the empty registration form — public *(already exists, no change needed)*
- `POST /register` — validate input, create user, redirect to `/login` — public

## Database changes
No database changes. The `users` table created in Step 01 already has all required columns: `id`, `name`, `email`, `password_hash`, `created_at`.

## Templates
- **Create:** none
- **Modify:** `templates/register.html` — no changes needed; the form already POSTs to `/register` and the `{% if error %}` block is already present

## Files to change
- `app.py`
  - Add `request`, `redirect`, `url_for` to the Flask import
  - Add `generate_password_hash` import from `werkzeug.security`
  - Add `app.secret_key` immediately after `app = Flask(__name__)`
  - Expand the `register()` route to accept `GET` and `POST` and implement the POST handler

## Files to create
No new files.

## New dependencies
No new dependencies.

## Rules for implementation
- No SQLAlchemy or ORMs — use `sqlite3` via `get_db()` only
- Parameterised queries only — never use string formatting in SQL
- Passwords hashed with `werkzeug.security.generate_password_hash` — never store plaintext
- Use CSS variables — never hardcode hex values (no template changes required here)
- All templates must extend `base.html` (already satisfied)
- Always close the DB connection in every code path, including early error returns
- Strip whitespace from `name` and `email`; lowercase `email` before storing

## Definition of done
- [ ] `GET /register` loads the form with HTTP 200 (no regression)
- [ ] Submitting a valid name / email / password inserts a new row in `users` and redirects to `/login`
- [ ] The stored `password_hash` is not equal to the plain-text password
- [ ] Submitting an email that already exists re-renders the form with an error — no duplicate row is created
- [ ] Submitting with an empty name re-renders the form with an error
- [ ] Submitting with an empty email re-renders the form with an error
- [ ] Submitting a password shorter than 8 characters re-renders the form with an error
- [ ] App starts without errors after the changes (`python app.py`)
