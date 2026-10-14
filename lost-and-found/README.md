# Lost & Found Desk

A multi-user web system built with **Python Flask**, **Python OOP** and **HTML/CSS**.
Students report lost items and claim found ones, staff handle found items and review claims,
and an admin manages everything. All data is kept in Python lists (no database).

## Features

| Role | What they can do |
|------|------------------|
| **Admin** | Add, edit and delete users; view, edit and delete all items; delete claims; see system statistics |
| **Staff** | Post found items; update any item's status; approve or reject claims; mark claimed items as returned |
| **Student** | Register an account; report lost items; browse found items; claim an item with proof of ownership; edit or delete own reports; track claim status; withdraw a pending claim |

Also included: login/logout with sessions, role-based access control, search and filter by keyword, type, category and status, form validation, flash messages, and custom 403, 404, 405 and 500 pages.

## Demo accounts

| Role | Username | Password |
|------|----------|----------|
| Admin | `admin` | `admin123` |
| Staff | `staff` | `staff123` |
| Student | `student` | `student123` |

Sample items and two sample claims are loaded when the server starts.

## Project structure

```
lost-and-found/
  app/
    app.py          Flask routes, login/role decorators, error pages
    models.py       All classes (User, Admin, Staff, Student, Item, LostItem, FoundItem, Claim)
    storage.py      The three lists (users, items, claims) and helper functions
    validators.py   Form validation
    templates/      base.html and all pages
    static/style.css
  requirements.txt
  README.md
  PRESENTATION.md   Slide outline and where each OOP concept lives
```

## Run locally

```bash
# 1. Open a terminal in the project folder
cd lost-and-found

# 2. (Recommended) create a virtual environment
python -m venv venv
venv\Scripts\activate          # Windows
source venv/bin/activate       # macOS / Linux

# 3. Install requirements
pip install -r requirements.txt

# 4. Start the app
cd app
python app.py
```

Open http://127.0.0.1:5000 and log in with a demo account.
Set `FLASK_DEBUG=1` before running if you want auto-reload while editing.

## Push to GitHub

```bash
cd lost-and-found
git init
git add .
git commit -m "Lost and Found system"
git branch -M main
# create an empty repository on github.com first, then:
git remote add origin https://github.com/YOUR-USERNAME/lost-and-found.git
git push -u origin main
```

## Deploy online

Important: because data is stored in lists in memory, the app must run as **one process**.
Several worker processes would each have their own separate lists. Data also resets
whenever the server restarts. This is expected for this assignment.

### Option A: PythonAnywhere (free, simplest)

1. Create a free account at pythonanywhere.com.
2. Open a **Bash console** and clone your repo:
   `git clone https://github.com/YOUR-USERNAME/lost-and-found.git`
3. Go to the **Web** tab, click **Add a new web app**, choose **Manual configuration**, then your Python version (3.10 or newer).
4. In the Web tab, click the **WSGI configuration file** link and replace its contents with:

   ```python
   import os
   import sys

   path = "/home/YOUR-USERNAME/lost-and-found/app"
   if path not in sys.path:
       sys.path.insert(0, path)

   os.environ["SECRET_KEY"] = "put-a-long-random-string-here"

   from app import app as application
   ```
5. If Flask is missing, run `pip install --user flask` in a Bash console.
6. Click **Reload**. Your live URL is `https://YOUR-USERNAME.pythonanywhere.com`.

### Option B: Render

1. Push the project to GitHub.
2. On render.com choose **New > Web Service** and connect the repository.
3. Settings:
   - **Build command:** `pip install -r requirements.txt`
   - **Start command:** `gunicorn --chdir app app:app --workers 1 --threads 4`
4. Under **Environment**, add `SECRET_KEY` with a long random value.
5. Deploy. Render gives you the live URL. Free instances sleep after inactivity, so the first load can be slow.

## Notes for improvement

- Forms do not use CSRF tokens. Add Flask-WTF if you extend this beyond a class project.
- Passwords are stored as hashes, never as plain text.
