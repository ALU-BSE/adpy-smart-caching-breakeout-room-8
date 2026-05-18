[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/MfYK1WWb)
[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=23945460)
# Smart Cache Layer — ALU BSE Activity

A guided hands-on activity for the **Advanced Python Programming**.  
Students add a production-quality cache layer to a Django REST API, level by level.

---

## What You'll Build

A caching layer for a blog API that handles:
- Shared cache for public data (all posts)
- User-isolated cache for personal data (drafts)
- Cache invalidation on data mutation
- Identifying and explaining a real cache security bug

---

## Quick Start

```bash
# Clone
git clone <repo-url>
cd smart-cache-activity

# Virtual environment
python -m venv venv
source venv/bin/activate      # Mac/Linux
venv\Scripts\activate         # Windows

# Install
pip install -r requirements.txt

# Setup & seed
python manage.py migrate
python manage.py seed

# Run
python manage.py runserver
```

---

## Project Structure

```
smart-cache-activity/
│
├── blog/                         # The Django app
│   ├── models.py                 # Post model
│   ├── serializers.py            # DRF serializers
│   ├── views.py                  # ← YOUR WORKING FILE
│   ├── urls.py                   # URL routing
│   └── management/
│       └── commands/
│           └── seed.py           # Database seeder (500 posts, 3 users)
│
├── config/                       # Django project config
│   ├── settings.py               # Settings (LocMemCache configured)
│   └── urls.py                   # Root URL config
│
├── docs/
│   └── ACTIVITY.md               # ← Student activity guide (start here)
│
├── timing.py                     # Response time measurement script
├── manage.py
└── requirements.txt
```

---

## Activity Levels

| Level | Concept | File |
|-------|---------|------|
| 1 — Feel the Pain | Baseline timing, no cache | `timing.py` |
| 2 — Shared Cache | Cache-aside for public endpoints | `blog/views.py` |
| 3 — User-Isolated Cache | Per-user keys + bug spotting | `blog/views.py` |
| 4 — Invalidation | Cache busting on data mutation | `blog/views.py` |
| Stretch — Query-Aware Keys | Paginated / filtered cache keys | `blog/views.py` |

Full instructions: [`docs/ACTIVITY.md`](docs/ACTIVITY.md)

---

## Test Users (after seeding)

| Username | Password | Notes |
|----------|----------|-------|
| alice | password123 | Has published posts and drafts |
| bob | password123 | Has published posts and drafts |
| carol | password123 | Has published posts and drafts |

---

## Cache Configuration

This activity uses **LocMemCache** — Django's in-process memory cache.

- No Redis install required
- Zero configuration — works out of the box
- Cache clears every time you restart the server (expected behaviour)

The cache is configured in `config/settings.py`:
```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.locmem.LocMemCache",
        "LOCATION": "smart-cache-activity",
    }
}
```
