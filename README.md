# 🔐 Secure User Authentication System

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![Flask](https://img.shields.io/badge/Flask-2.3+-black?logo=flask)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16+-blue?logo=postgresql)
![bcrypt](https://img.shields.io/badge/bcrypt-4.0+-orange)
![License](https://img.shields.io/badge/License-MIT-green)

A full-stack **secure user authentication** web application built with **Flask** (Python) and **PostgreSQL**, featuring password hashing with bcrypt, session-based authentication, input validation, and a clean split-screen UI.

---

## 📋 Overview

This system provides a complete authentication workflow — user registration, login, protected dashboard, and logout — with strong security practices:

- **Passwords** are hashed using **bcrypt** (never stored in plaintext)
- **Sessions** are managed server-side via Flask's signed cookies
- **Routes** enforce authentication checks before granting access to protected content
- **Input validation** runs on both the client (HTML5 attributes) and server (Python logic)
- **Flash messages** provide real-time feedback for success/error states

---

## ✨ Features

| Feature | Description |
|---|---|
| ✅ **User Registration** | Sign up with username, email, password, SA ID number, gender, and phone number |
| ✅ **Password Strength Validation** | Minimum 8 characters, uppercase, lowercase, and digit required |
| ✅ **Password Confirmation** | Ensure passwords match on registration |
| ✅ **Secure Login / Logout** | Session-based authentication with logout |
| ✅ **bcrypt Password Hashing** | Industry-standard password hashing |
| ✅ **Duplicate Detection** | Prevents duplicate username, email, or ID number |
| ✅ **Protected Dashboard** | Authenticated-only content page |
| ✅ **Flash Messages** | User-friendly success and error notifications |
| ✅ **Responsive Split-Screen UI** | Modern two-panel layout adapting to mobile |
| ✅ **Input Sanitization** | Strip whitespace, validate types on server side |

---

## 📁 Project Structure

```
Secure_User_Authentication/
│
├── app.py                        # Flask application entry point (routes, validation logic)
├── database.py                   # PostgreSQL database layer (connection, CRUD, hashing)
├── test.py                       # Test suite (empty/placeholder)
│
├── static/
│   └── css/
│       ├── index.css             # Styling for login page (split-screen layout)
│       ├── register.css          # Styling for registration page
│       └── dashboard.css         # Styling for dashboard (cards, security status)
│
├── templates/
│   ├── index.html                # Login page (split-screen, form with flash messages)
│   ├── register.html             # Registration form (7 fields, validation)
│   └── dashboard.html            # Authenticated user dashboard (profile, protected content)
│
├── database.cpython-313.pyc      # Compiled Python bytecode (auto-generated)
└── README.md                     # This file
```

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Backend** | Python 3.10+ / Flask 2.3+ |
| **Database** | PostgreSQL 16+ |
| **Password Hashing** | bcrypt 4.0+ |
| **Frontend** | HTML5, CSS3 (vanilla, no JS frameworks) |
| **Icons** | Font Awesome 6.5.1 |
| **Database Driver** | psycopg2 (with RealDictCursor) |

---

## 📦 Prerequisites

- **Python** 3.10 or higher
- **PostgreSQL** 16 or higher installed and running
- **pip** (Python package manager)

---

## 🔧 Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/TshifhunguCodes/Secure_User_Authentication.git
cd Secure_User_Authentication
```

### 2. Create a Virtual Environment (Recommended)

```bash
python -m venv venv
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Python Dependencies

```bash
pip install flask psycopg2-binary bcrypt
```

### 4. Set Up PostgreSQL Database

Open your PostgreSQL client (pgAdmin, psql, or another tool) and run:

```sql
CREATE DATABASE "UserAuthDB";
```

Then create the required table:

```sql
CREATE TABLE "User" (
    "id" SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    idnumber VARCHAR(13) UNIQUE NOT NULL,
    gender VARCHAR(10) NOT NULL,
    phonenumber VARCHAR(15) NOT NULL,
    "Passwords" VARCHAR(255) NOT NULL
);
```

> **⚠️ Note:** The table name `"User"` and the password column `"Passwords"` are **case-sensitive and quoted** in the SQL and Python code. This is intentional but deviates from standard PostgreSQL conventions. See [Known Issues](#known-issues--fixes) below.

### 5. Configure Database Credentials

Edit **`database.py`** and update the `DB_CONFIG` dictionary with your PostgreSQL credentials:

```python
DB_CONFIG = {
    "host": "localhost",
    "dbname": "UserAuthDB",
    "user": "postgres",
    "password": "your_password_here",
    "port": 5432
}
```

### 6. Run the Application

```bash
python app.py
```

The server will start at `http://0.0.0.0:5000` (accessible from `http://localhost:5000` on your machine).

---

## 🗄 Database Schema

### Table: `"User"`

| Column | Type | Constraints | Description |
|---|---|---|---|
| `"id"` | `SERIAL` | `PRIMARY KEY` | Auto-incrementing unique user ID |
| `username` | `VARCHAR(50)` | `UNIQUE NOT NULL` | User's chosen username |
| `email` | `VARCHAR(100)` | `UNIQUE NOT NULL` | User's email address |
| `idnumber` | `VARCHAR(13)` | `UNIQUE NOT NULL` | South African ID number (13 digits) |
| `gender` | `VARCHAR(10)` | `NOT NULL` | Gender selection (Male / Female / Other) |
| `phonenumber` | `VARCHAR(15)` | `NOT NULL` | Contact phone number (10-15 digits) |
| `"Passwords"` | `VARCHAR(255)` | `NOT NULL` | bcrypt-hashed password (note: uppercase `P`, quoted) |

---

## 🌐 Application Routes

| Method | Route | Description | Auth Required |
|---|---|---|---|
| `GET` | `/` | Login page (redirects to dashboard if already logged in) | No |
| `POST` | `/login` | Authenticate user credentials and create session | No |
| `GET` | `/register` | Display registration form | No |
| `POST` | `/register` | Process registration and create new user | No |
| `GET` | `/dashboard` | Display protected user dashboard | **Yes** |
| `GET` | `/logout` | Clear session and log out | No |

---

## ✅ Validation Rules

### Password
- Minimum **8 characters**
- At least **one uppercase letter** (A-Z)
- At least **one lowercase letter** (a-z)
- At least **one digit** (0-9)

### SA ID Number
- Must be exactly **13 digits**
- Digits only (no spaces, letters, or special characters)

### Phone Number
- Must be between **10 and 15 digits**
- Digits only (spaces and dashes are stripped before validation)

### Duplicate Checks
- **Username** must be unique
- **Email** must be unique
- **ID number** must be unique

---

## 🔒 Security Features

| Security Measure | Implementation |
|---|---|
| **Password Hashing** | bcrypt with auto-generated salt via `bcrypt.gensalt()` |
| **Password Verification** | `bcrypt.checkpw()` comparing plaintext against stored hash |
| **Session Management** | Flask signed session cookies with `secret_key` |
| **Protected Routes** | All sensitive routes check `session['user_id']` before rendering |
| **Password Removal** | Password hash is removed from user data after authentication (before returning to route) |
| **Input Sanitization** | `strip()` applied to all text inputs on the server side |
| **SQL Injection Protection** | Parameterised queries (`%s` placeholders) — never string concatenation |

---

## 🎨 UI / UX

- **Split-screen layout** — branded left panel with gradient blue background, functional form on right
- **Responsive design** — stacked vertically on mobile (breakpoint at 768px)
- **Flash messages** — colour-coded: red for errors, green for success
- **Dashboard cards** — grid layout with hover effects, security status panel
- **Form validation** — HTML5 `required`, `minlength`, `pattern` attributes for instant client-side feedback

---

## ⚠️ Known Issues & Fixes

During development, several PostgreSQL-specific quirks were encountered. These are documented as comments in the source code:

1. **Quoted Table & Column Names** — The table `"User"` and the password column `"Passwords"` use double-quoted identifiers. This makes them **case-sensitive**. Standard PostgreSQL practice is to use unquoted lowercase identifiers. This was a design choice to match a pre-existing schema.

2. **`user['id']` vs `user['Id']`** — In the `login()` route (line 71 of `app.py`), accessing the user ID uses `user['id']` (lowercase) even though the column is defined as `"id"` (quoted, lowercase). This works because PostgreSQL folds unquoted identifiers to lowercase by default.

---

## 🔮 Future Enhancements

- [ ] **CSRF Protection** — Add Flask-WTF for cross-site request forgery tokens
- [ ] **Rate Limiting** — Prevent brute-force login attempts with Flask-Limiter
- [ ] **Email Verification** — Send confirmation email on registration
- [ ] **Password Reset** — "Forgot password" flow with token-based reset
- [ ] **OAuth / Social Login** — Google, GitHub authentication
- [ ] **Two-Factor Authentication (2FA)** — TOTP-based authenticator app support
- [ ] **Account Lockout** — Lock account after N failed login attempts
- [ ] **HTTPS Enforcement** — Redirect HTTP to HTTPS in production
- [ ] **Logging & Monitoring** — Structured logging with timestamps for security events
- [ ] **Database Migrations** — Use Alembic/Flask-Migrate for schema versioning

---

## 👨‍💻 Author

**TshifhunguCodes**  
[GitHub Profile](https://github.com/TshifhunguCodes)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<p align="center">
  Built with ❤️ using Flask & PostgreSQL
</p>