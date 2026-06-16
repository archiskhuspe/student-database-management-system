# Student Database Management System

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-2.x-black?logo=flask)
![MySQL](https://img.shields.io/badge/MySQL-8.x-4479A1?logo=mysql&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

A Flask web application for managing student records, attendance, and department data, backed by a MySQL database. Built as a coursework project.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [How It Works](#how-it-works)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Limitations](#limitations)
- [License](#license)

---

## Features

- View, add, edit, and delete student records (CRUD)
- Track student attendance by roll number
- Manage academic departments
- Search student profile and attendance by roll number
- User authentication (signup, login, logout) with hashed passwords
- Audit log of database trigger events

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python 3, Flask, Flask-SQLAlchemy, Flask-Login |
| Database | MySQL 8, SQLAlchemy ORM |
| Frontend | Jinja2 templates, Bootstrap 5, jQuery |
| Auth | Werkzeug password hashing (PBKDF2) |

---

## How It Works

All routes are defined in `app/app.py`. Flask-SQLAlchemy maps six models (`Student`, `Attendance`, `Department`, `User`, `Trig`, `Test`) to MySQL tables. Login-required routes use Flask-Login's `@login_required` decorator. The `Trig` table is populated by MySQL triggers (defined in `app/students.sql`) that fire on student insert/update/delete events.

---

## Prerequisites

- Python 3.8+
- MySQL 8.x running locally
- `mysqlclient` C library (`brew install mysql` on macOS, `sudo apt install libmysqlclient-dev` on Ubuntu)

---

## Installation

```bash
# 1. Clone the repo
git clone https://github.com/archiskhuspe/student-database-management-system.git
cd student-database-management-system

# 2. Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate        # macOS/Linux
# venv\Scripts\activate         # Windows

# 3. Install dependencies
pip install -r app/requirements.txt
```

---

## Configuration

1. Copy `.env.example` to `.env` and fill in your values:

```bash
cp .env.example .env
```

```env
SECRET_KEY=your_secret_key_here
DATABASE_URI=mysql://username:password@localhost/student_dbms
```

2. Create the MySQL database and load the schema:

```sql
CREATE DATABASE student_dbms;
```

```bash
mysql -u root -p student_dbms < app/students.sql
```

---

## Usage

```bash
cd student-database-management-system
source venv/bin/activate
python app/app.py
```

Open `http://127.0.0.1:5000` in your browser.

| Route | Description |
|-------|-------------|
| `/` | Home page |
| `/signup` | Register a new admin account |
| `/login` | Log in |
| `/studentdetails` | View all students |
| `/addstudent` | Add a student (login required) |
| `/edit/<id>` | Edit a student (login required) |
| `/delete/<id>` | Delete a student (login required) |
| `/addattendance` | Record attendance |
| `/search` | Look up a student by roll number |
| `/department` | Manage departments |
| `/triggers` | View audit trigger log |

---

## Project Structure

```
student-database-management-system/
├── app/
│   ├── app.py               # Flask application and all routes
│   ├── requirements.txt     # Python dependencies
│   ├── students.sql         # Database schema and triggers
│   ├── static/
│   │   ├── assets/          # Bootstrap, jQuery, Font Awesome vendor files
│   │   ├── css/             # Custom stylesheets
│   │   └── images/          # Static images
│   └── templates/           # Jinja2 HTML templates (16 pages)
├── .env.example             # Environment variable template
├── .gitignore
├── LICENSE
└── README.md
```

---

## Limitations

- No input validation on forms (e.g. email format, attendance range 0–100, roll number uniqueness)
- No CSRF protection on forms
- No rate limiting on the login endpoint
- Attendance records are append-only; querying only returns the first match per roll number
- The `/test` route exposes database connectivity status publicly
- This is a local development prototype; it is not configured for production deployment

---

## License

Released under the [MIT License](LICENSE).
