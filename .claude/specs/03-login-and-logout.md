# Spec: Login and Logout

## Overview
Allow registered users to sign in with their email and password, and sign out when done. The `POST /login` route looks up the user by email, verifies the password with werkzeug, stores the user's `id` and `name` in the Flask session on success, and redirects to `/`. On failure it re-renders the login form with a generic error message. The `GET /logout` route clears the session and redirects to `/`. Together these two routes are the foundation for all access-controlled pages in the Spendly roadmap.

## Depends on
- Step 01 — Database Setup: `get_db()` and the `users` table must exist.
- Step 02 — Registration: At least one real user must exist in the database to log in.

## Routes
- `GET /login` — render the empty login form — public *(already exists, change method list to accept POST)*
- `POST /login` — validate credentials, set session, redirect to `/profile` — public
- `GET /logout` — clear session, redirect to `/` — public (stub already exists; replace it)

## Database changes
No database changes. The `users` table already has `id`, `email`, and `password_hash`.

## Templates
- **Create:** none
- **Modify:** `templates/login.html` — no structural changes needed; the form already POSTs to `/login` and the `{% if error %}` block is already present

## Files to change
- `app.py`
  - Add `session` to the Flask import (`from flask import Flask, render_template, request, redirect, url_for, session`)
  - Add `check_password_hash` to the werkzeug import
  - Expand the `login()` route to accept `GET` and `POST` and implement the POST handler:
    - Read `email` and `password` from `request.form`; strip and lowercase email
    - Query `users` for a row matching the email
    - If no row found, or `check_password_hash` returns False → re-render `login.html` with `error="Invalid email or password."`
    - On success: call `session.clear()`, then set `session['user_id']` and `session['user_name']`; redirect to `url_for('profile')`
  - Replace the `logout()` stub with a real implementation:
    - Call `session.clear()`
    - Redirect to `url_for('landing')`

## Files to create
No new files.

## New dependencies
No new dependencies.

## Rules for implementation
- No SQLAlchemy or ORMs — use `sqlite3` via `get_db()` only
- Parameterised queries only — never use string formatting in SQL
- Passwords verified with `werkzeug.security.check_password_hash` — never compare plaintext
- Use a single generic error message for both "email not found" and "wrong password" to avoid user enumeration
- Always close the DB connection after querying
- Use CSS variables — never hardcode hex values (no template changes required here)
- All templates extend `base.html` (already satisfied)
- `app.secret_key` is already set in `app.py` — do not change it

## Definition of done
- [ ] `GET /login` loads the form with HTTP 200 (no regression)
- [ ] Submitting the correct email and password sets the session and redirects to `/`
- [ ] After login, `session['user_id']` and `session['user_name']` are populated with the correct values
- [ ] Submitting a wrong password re-renders the form with an error — no session is created
- [ ] Submitting an email that does not exist re-renders the form with an error — no session is created
- [ ] `GET /logout` clears the session and redirects to `/`
- [ ] After logout, `session` contains no `user_id` key
- [ ] App starts without errors after the changes (`python app.py`)
