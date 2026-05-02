# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Spendly** is a Flask-based personal expense tracker web application. It helps users log expenses, understand spending patterns, and stay on budget. The project emphasizes a clean, simple UI with vanilla JavaScript (no frameworks) and semantic HTML.

## Quick Start

### Run the Development Server
```powershell
python app.py
```
The app starts on `http://localhost:5001` with debug mode enabled.

### Run Tests
```powershell
pytest
```

### Run a Specific Test
```powershell
pytest tests/test_name.py -v
```

### Install Dependencies
```powershell
pip install -r requirements.txt
```

## Architecture

### Stack
- **Backend**: Flask 3.1.3 with Jinja2 templating
- **Frontend**: Vanilla JavaScript (no frameworks), semantic HTML5
- **Styling**: CSS3 with custom CSS variables for theming
- **Testing**: pytest with pytest-flask
- **Database**: SQLite (to be implemented; db.py is prepared for students)
- **Python Version**: 3.13+

### Directory Structure
```
expense-tracker/
├── app.py                 # Flask application with route definitions
├── database/
│   ├── __init__.py
│   └── db.py             # Database initialization (student-implemented)
├── templates/
│   ├── base.html         # Base template with nav, footer, block structure
│   ├── landing.html      # Public landing page
│   ├── login.html        # Login form
│   ├── register.html     # Registration form
│   └── terms.html        # Terms and Conditions page
├── static/
│   ├── css/
│   │   └── style.css     # All styling; uses CSS variables for theming
│   └── js/
│       └── main.js       # Vanilla JavaScript (no frameworks)
├── requirements.txt      # Python dependencies
└── pyproject.toml        # Project metadata
```

### Route Structure (app.py)
Routes are grouped into two categories:

**Implemented Routes:**
- `GET /` → landing page (public)
- `GET /register` → registration form
- `GET /login` → login form
- `GET /terms` → Terms and Conditions page

**Placeholder Routes (to be built):**
- `GET /logout` → Step 3
- `GET /profile` → Step 4
- `POST /expenses/add` → Step 7
- `POST /expenses/<id>/edit` → Step 8
- `POST /expenses/<id>/delete` → Step 9

Routes return rendered Jinja2 templates using `render_template()`.

### Template Hierarchy
All pages extend `base.html` using `{% extends "base.html" %}`. This provides:
- Navigation bar with brand, sign in, and "Get started" links
- Footer with brand name, copy, and links to Terms/Privacy
- Main content block for page-specific content
- Script block at the end for page-specific JavaScript

Key blocks in `base.html`:
- `{% block title %}` — page title
- `{% block content %}` — main page content
- `{% block head %}` — optional head additions
- `{% block scripts %}` — page-specific scripts (loaded after main.js)

### CSS System

**CSS Variables** (defined in style.css):
- `--ink` — dark text/primary color
- `--paper`, `--paper-warm`, `--paper-card` — background colors
- `--accent`, `--accent-2`, `--accent-light` — interactive elements
- `--font-sans`, `--font-display` — typography families
- `--radius-*` — border radius tokens
- `--max-width` — container max-width

Use these variables consistently for theming. Color changes only require updating variable values.

**Key CSS Classes:**
- `.btn-primary`, `.btn-ghost` — button styles
- `.navbar`, `.footer` — structural components
- `.hero`, `.features`, `.cta-section` — section patterns
- `.legal-*` — styles for Terms/legal pages

**Responsive Design:** Breakpoints around 900px; use flexbox and grid for layouts.

### Frontend (Vanilla JavaScript)

**No frameworks or build tools.** JavaScript runs inline in template `<script>` blocks using:
- Vanilla DOM API (`querySelector`, `addEventListener`, `classList`, etc.)
- Template blocks with `{% block scripts %}`

**Common patterns:**
- `DOMContentLoaded` event for initialization
- Event delegation for interactive elements
- No jQuery or external libraries

Example from `landing.html`: Modal functionality for video playback uses vanilla JS to open/close and stop video.

## Important Notes

### Database
The `database/db.py` module is a placeholder. Students implement:
- `get_db()` — SQLite connection factory
- `init_db()` — schema creation
- `seed_db()` — sample data loading

### Styling Consistency
All colors, fonts, and spacing go through CSS variables in `style.css`. Do not use inline styles or magic values; add new variables to the system if needed.

### Asset Linking in Templates
Always use `url_for()` for static assets:
```jinja2
<link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
<script src="{{ url_for('static', filename='js/main.js') }}"></script>
```

### Template Modifications
When editing templates, maintain the `{% extends "base.html" %}` pattern and use named blocks for content isolation. Don't duplicate base HTML structure.

### Video Modal Example (landing.html)
The "See how it works" button opens a YouTube video in a modal. JavaScript:
- Finds the button by text content
- Opens modal on click
- Closes modal on close button or overlay click
- Stops video by resetting iframe src to prevent background playback
- Prevents page scroll when modal is active

This pattern can be reused for other modals.

## Future Steps
The app is structured as a learning project with placeholder routes for student implementation. The database module is prepared but needs implementation before CRUD operations can work.
