# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Spendly — a Flask + SQLite expense tracker. This is a step-by-step learning scaffold: routes and files exist as placeholders (`app.py` has comments like "coming in Step 7", `database/db.py` has a comment listing functions to implement in "Step 1"). Most backend logic (DB access, auth, expense CRUD) has not been implemented yet — only the landing/legal pages and route skeleton exist.

## Environment

**Always work in WSL (Ubuntu), not Windows Git Bash/PowerShell, for Python/pip/pytest/flask commands.** The project lives at `/mnt/c/Users/nitis/OneDrive/Desktop/claude_proj/expense-tracker` inside WSL. A venv created with Windows Python produces `venv/Scripts/`, which does not work from WSL — the venv in this repo is a WSL-native Python 3.12.3 venv with `venv/bin/activate`. Git operations are fine from either side since they only touch the filesystem.

```bash
wsl
cd /mnt/c/Users/nitis/OneDrive/Desktop/claude_proj/expense-tracker
source venv/bin/activate
```

## Common commands

Run these inside the activated WSL venv:

```bash
pip install -r requirements.txt   # install deps (flask, werkzeug, pytest, pytest-flask)
python app.py                     # run the dev server on http://127.0.0.1:5001 (debug=True)
pytest                            # run tests
pytest path/to/test_file.py::test_name   # run a single test
```

Flask runs on port **5001**, not the default 5000.

## Architecture

- **`app.py`** — single-file Flask app; all routes are defined here directly (no blueprints). Implemented routes render Jinja templates (`/`, `/register`, `/login`, `/terms`, `/privacy`); placeholder routes (`/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`) currently return plain strings and are not yet built out.
- **`database/db.py`** — intended to hold `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (create tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample data). No ORM — raw `sqlite3` from the stdlib. Currently a stub.
- **`templates/`** — server-rendered Jinja2 templates. `base.html` defines the shared layout (nav, footer, `{% block content %}`) that other templates extend.
- **`static/css/style.css`** — shared site-wide styles; **`static/css/landing.css`** — landing-page-specific styles layered on top.
- **`static/js/main.js`** — currently empty/stub; vanilla JS, no framework or build tooling.
- No auth, session, or database layer is wired up yet — expect to build these when implementing the placeholder routes.
