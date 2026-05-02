# Deployment Configuration Guide

This guide explains how to configure Spendly for different deployment environments (Development, Staging, Production).

## Overview

The `.claude` folder contains environment-specific configurations:
- **settings.json** — Shared settings for all environments (committed to git)
- **settings.local.json** — Local development overrides (git-ignored, use for sensitive data)
- **DEPLOYMENT.md** — This guide

## Environment Variables

### Development (Local)
```powershell
# Run locally with debug mode enabled
$env:FLASK_ENV = "development"
$env:FLASK_DEBUG = "1"
$env:DATABASE_URL = "sqlite:///expense_tracker.db"
```

### Staging
```powershell
# Railway/Heroku staging environment
FLASK_ENV=staging
FLASK_DEBUG=0
DATABASE_URL=<your-staging-database-url>
SECRET_KEY=<your-staging-secret-key>
```

### Production
```powershell
# Railway/Heroku production environment
FLASK_ENV=production
FLASK_DEBUG=0
DATABASE_URL=<your-production-database-url>
SECRET_KEY=<your-production-secret-key>
```

## Pre-Deployment Checklist

Claude will automatically run these checks when you mention "deploy", "push to", or "commit":

✅ **Tests**: `pytest -v`
✅ **Format Check**: Black (Python), Prettier (CSS/JS)
✅ **Build Check**: Flask app initialization

## Configuration Files

### settings.json (Shared - Committed)
Contains:
- Permissions (git, python, pytest, file ops)
- Code formatting hooks (Black, Prettier)
- Pre-deployment test hooks

### settings.local.json (Local - Git-Ignored)
Contains:
- Local development environment variables
- Sensitive credentials (API keys, database URLs)
- Personal overrides

## Setting Up for Deployment

### 1. Local Development
```powershell
# Install dependencies
pip install -r requirements.txt

# Run development server
python app.py

# Settings use: settings.json + settings.local.json (dev env vars)
```

### 2. Prepare for Staging

Update your Heroku/Railway remote and environment variables:

```bash
# Set staging environment variables
heroku config:set FLASK_ENV=staging DATABASE_URL=... SECRET_KEY=...

# Or for Railway:
railway env FLASK_ENV staging DATABASE_URL ... SECRET_KEY ...
```

### 3. Prepare for Production

After staging validation, promote to production:

```bash
# Set production environment variables
heroku config:set FLASK_ENV=production DATABASE_URL=... SECRET_KEY=...

# Or for Railway:
railway env FLASK_ENV production DATABASE_URL ... SECRET_KEY ...
```

## Deployment Workflow

1. **Make code changes** → Auto-formatted with Black & Prettier
2. **Run tests locally** → `pytest` or let Claude run it
3. **Commit changes** → Claude runs pre-deployment checks
4. **Deploy** → Push to staging, validate, then promote to production

## Claude Code Integration

### Pre-Deployment Hooks
When you say "deploy" or "commit", Claude automatically:
1. Runs `pytest` to verify all tests pass
2. Checks code formatting
3. Validates the Flask app can start

### Environment Awareness
- **Development**: Uses `settings.local.json` for local overrides
- **Staging/Production**: Set environment variables on your PaaS platform
- Claude respects `FLASK_ENV` and adjusts behavior accordingly

## Troubleshooting

**Tests failing before deploy?**
- Run `pytest -v` locally to debug
- Claude will block deployment if tests fail (by design)

**Formatting issues?**
- Black auto-formats on save
- Prettier auto-formats CSS/JS on save
- Manually run: `black .` or `npx prettier --write .`

**Environment variables not loaded?**
- Check `settings.local.json` for development
- Check platform dashboard (Heroku/Railway) for staging/prod
- Use `echo $FLASK_ENV` to verify

## Next Steps

1. Configure your database (SQLite for dev, PostgreSQL for staging/prod)
2. Add SECRET_KEY to environment variables
3. Set up GitHub/Git integration with your PaaS platform
4. Enable auto-deploy on push (if desired)

For more info: See the main CLAUDE.md and your platform's documentation.
