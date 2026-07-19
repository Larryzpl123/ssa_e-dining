# 🍽️ Shady Side Academy E-Dining

A web platform that makes school dining easier: students browse the menu with
nutrition and allergen information, pre-order meals, build custom sandwiches, and
rate dishes — while cafeteria staff manage everything through a simple admin
interface that requires no technical skill.

**Live demo:** https://ssaedining.pythonanywhere.com/

Built by students, for our school.

---

## Why we built it

Dining halls run on guesswork: students don't know what's being served or whether
it's safe for their allergies, staff don't know how much to prepare, and useful
feedback rarely reaches the kitchen. E-Dining puts all of that in one place.

---

## Features

### For students

| Feature | What it does |
|---|---|
| **Menu** | Photos, calories, allergens, average rating, and ingredient tags for every dish. No login needed to browse. |
| **Smart filters** | Search by name, plus three filters: **Only** (e.g. all-vegetable dishes), **Include** (e.g. `chicken`), and **Not include** (e.g. `meat, egg, fish`). Tags cascade — filtering out "meat" also removes chicken, beef, and pork. |
| **Pre-order** | Order several dishes at once with quantities and a single pickup time. |
| **Sandwich builder** | Build a custom sandwich from available breads, proteins, veggies, and sauces. |
| **Feedback** | Rate any dish and leave a comment. One rating per dish per student, editable any time. |
| **Accounts** | Registration is restricted to `@shadysideacademy.org` emails; passwords are stored hashed. |

### For cafeteria staff (admin)

| Feature | What it does |
|---|---|
| **Dashboard** | See pre-orders grouped by student, sandwich orders, and all feedback. Mark items done to clear them. |
| **Search** | Find any order or feedback by student name, email, dish, or comment. |
| **Menu management** | Add, edit, and delete dishes and upload photos — all through web forms, no code. |
| **Save for later** | Archive a seasonal dish off the menu while keeping its info and ratings, then restore it when it returns. |
| **Pre-order toggle** | Mark items like the salad bar as dine-in only: visible on the menu, unavailable to pre-order. |
| **Sandwich components** | Manage the sandwich bar ingredients and toggle availability. |
| **Feedback moderation** | See who wrote each review, remove inappropriate ones, and suspend a student's feedback access (reversible). |
| **AI estimate** *(optional)* | Upload a dish photo and name; an AI suggests likely ingredients and calories per 100g for staff to review before saving. |

---

## Tech stack

Deliberately simple and dependency-light, so a student can read the whole thing:

- **Python 3** + **Flask** — the web server
- **SQLite** — a single-file database, no separate server to run
- **Jinja2** templates + plain **CSS** — no JavaScript framework
- **OpenRouter API** *(optional)* — AI ingredient/calorie estimates
- **SMTP** *(optional)* — email verification codes

---

## Project structure

```
E-Dining/
├── app.py              All routes, auth, and the auto-migration
├── setup_db.py         Creates + seeds a fresh database (first install only)
├── tags.py             Ingredient→category tagging and filter logic
├── ai.py               OpenRouter calorie/ingredient estimation
├── mailer.py           Verification-code email (SMTP or test mode)
├── dining.db           The database (created automatically; holds all data)
├── templates/          All pages (Jinja2 HTML)
├── static/
│   ├── style.css       Full theme — edit the :root variables to recolor
│   └── images/         Uploaded dish photos
├── GUIDE.md            How to run and maintain the app
├── DEPLOY.md           Step-by-step first deployment
└── UPDATING.md         Safe updates + email setup
```

---

## Quick start (local)

```bash
cd E-Dining
pip install -r requirements.txt
python3 setup_db.py      # first time only — creates and seeds the database
python3 app.py
```

Open **http://127.0.0.1:8000**

> Port 8000, not 5000 — macOS reserves 5000 for AirPlay.

Default admin password is set in `app.py`; log in via the **Admin** link.

---

## Configuration

All secrets come from environment variables, with laptop-friendly fallbacks. Set
these properly before real use:

| Variable | Purpose |
|---|---|
| `SECRET_KEY` | Signs login cookies. **Must** be a long random string in production. |
| `ADMIN_PASSWORD` | Staff/admin login password. |
| `ALLOWED_DOMAIN` | Email domain allowed to register (default `shadysideacademy.org`). |
| `SMTP_HOST` / `SMTP_PORT` / `SMTP_USER` / `SMTP_PASS` | Enables real verification emails. |

For AI estimates, put your OpenRouter key in `openrouter_key.txt` (see
`openrouter_key.txt.example`) or the `OPENROUTER_API_KEY` variable.

### Email verification behaves differently by setup

- **No SMTP configured** → registration completes immediately. Ideal for testing.
- **SMTP configured** → students must enter a 6-digit emailed code before logging in,
  so nobody can register using someone else's address.

---

## Deployment

See **DEPLOY.md** for a full first-time walkthrough (PythonAnywhere), and
**UPDATING.md** for updating a live site without losing data.

The database **auto-migrates** on startup: new columns and tables are added
automatically while existing dishes, photos, accounts, and orders are preserved.
Never run `setup_db.py` on a live site — it resets the menu.

---

## Safety and privacy notes

- **Allergen information is always entered by a human**, never generated by AI.
  AI estimates cover calories and likely ingredients only, and are clearly labeled
  as estimates for staff to review.
- Feedback is **not anonymous** — staff can see the author. Students are told this
  before they post.
- The app stores student names, school emails, and order history. Confirm data
  handling with school administration before a full rollout.

---

## Roadmap

- [ ] Password reset / account recovery
- [ ] Automatic database backups
- [ ] Rate limiting on login and registration
- [ ] Mobile layout testing on real devices
- [ ] Nutrition data beyond calories

---

## Credits

Project by the E-Dining Team at Shady Side Academy:
Larry Zhong, Leon Li, Ryan Gong, Samuel Li, Peter Liu, William Wei, and Jeff Jin
