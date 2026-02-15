# Login & Registration System Setup Guide

## Project Overview
This guide provides everything you need to set up and run the login and registration system for your FirstWebsite Laravel application.

## Database Setup

### Step 1: Create the Database
In **phpMyAdmin** or using **MySQL Command Line**, run:

```sql
CREATE DATABASE firstwebsite;
```

### Step 2: Database Tables & Attributes

The system uses the following tables:

#### **users Table**
```sql
CREATE TABLE users (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    email_verified_at TIMESTAMP NULL,
    password VARCHAR(255) NOT NULL,
    remember_token VARCHAR(100) NULL,
    created_at TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    CHARSET utf8mb4,
    COLLATE utf8mb4_unicode_ci
);
```

#### **password_reset_tokens Table** (For password reset functionality)
```sql
CREATE TABLE password_reset_tokens (
    email VARCHAR(255) PRIMARY KEY,
    token VARCHAR(255) NOT NULL,
    created_at TIMESTAMP NULL,
    CHARSET utf8mb4,
    COLLATE utf8mb4_unicode_ci
);
```

#### **sessions Table** (For session management)
```sql
CREATE TABLE sessions (
    id VARCHAR(255) PRIMARY KEY,
    user_id BIGINT UNSIGNED NULL,
    ip_address VARCHAR(45) NULL,
    user_agent TEXT NULL,
    payload LONGTEXT NOT NULL,
    last_activity INT NOT NULL,
    INDEX idx_user_id (user_id),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    CHARSET utf8mb4,
    COLLATE utf8mb4_unicode_ci
);
```

## Configuration Steps

### Step 1: Update .env File
The `.env` file has been updated with MySQL configuration:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=firstwebsite
DB_USERNAME=root
DB_PASSWORD=
```

**Note:** If your MySQL has a password, add it to `DB_PASSWORD=`

### Step 2: Run Migrations (Option A - Recommended)
Open terminal in your project directory and run:

```bash
php artisan migrate
```

This will automatically create all necessary tables using Laravel's migration system.

### Step 3: Alternative Database Setup (Option B)
If migrations don't work, manually run the SQL commands provided above in phpMyAdmin.

## File Structure

The following files have been created:

```
app/Http/Controllers/
├── LoginController.php      - Handles login logic
├── RegisterController.php    - Handles registration logic
└── DashboardController.php   - Protected dashboard page

resources/views/
├── auth/
│   ├── login.blade.php      - Login form
│   └── register.blade.php    - Registration form
└── dashboard.blade.php       - Protected dashboard

routes/
└── web.php                   - Updated with auth routes
```

## Features Implemented

### ✅ Registration System
- Full name, email, and password fields
- Password confirmation validation
- Unique email validation
- Minimum 8-character password requirement
- Automatic user login after registration
- Error messages for validation failures

### ✅ Login System
- Email and password authentication
- "Remember me" functionality
- Session management
- Error handling for incorrect credentials
- Protected routes with auth middleware

### ✅ Dashboard
- Protected route (requires authentication)
- Displays user information
- Logout functionality
- Session invalidation on logout

### ✅ Security Features
- Password hashing using bcrypt
- CSRF protection on all forms
- Secure session management
- Password confirmation validation
- SQL injection protection

## Route Structure

```
GET/POST /register             - Registration page & form submission
GET/POST /login                - Login page & form submission
POST /logout                   - Logout route
GET /dashboard                 - Protected user dashboard
```

## How to Use

### 1. Create User Account (Registration)
- Navigate to `http://localhost:8000/register`
- Enter name, email, and password
- Confirm password
- Click "Register"
- You'll be automatically logged in and redirected to dashboard

### 2. Login to Existing Account
- Navigate to `http://localhost:8000/login`
- Enter email and password
- Optional: Check "Remember me" to stay logged in
- Click "Login"
- Access the dashboard at `http://localhost:8000/dashboard`

### 3. Logout
- Click the "Logout" button on the dashboard
- You'll be logged out and redirected to homepage

## Database Attributes Explained

### users Table Columns:
| Column | Type | Description |
|--------|------|-------------|
| id | BIGINT UNSIGNED | Primary key, auto-increment |
| name | VARCHAR(255) | User's full name |
| email | VARCHAR(255) | User's email (unique) |
| email_verified_at | TIMESTAMP | Email verification timestamp |
| password | VARCHAR(255) | Bcrypt hashed password |
| remember_token | VARCHAR(100) | Token for "Remember me" feature |
| created_at | TIMESTAMP | Account creation time |
| updated_at | TIMESTAMP | Last update time |

## Validation Rules

### Registration
- **Name:** Required, string, max 255 characters
- **Email:** Required, valid email format, unique in database
- **Password:** Required, minimum 8 characters, must be confirmed

### Login
- **Email:** Required, valid email format
- **Password:** Required, string

## Troubleshooting

### Issue: "Connection refused" error
**Solution:** Ensure MySQL is running in XAMPP Control Panel

### Issue: "SQLSTATE[HY000]: General error"
**Solution:** Run `php artisan migrate` or check database name in .env

### Issue: "Route not found"
**Solution:** Run `php artisan route:cache` and `php artisan route:clear`

### Issue: Sessions not working
**Solution:** Ensure `SESSION_DRIVER=database` in .env (already configured)

## Next Steps

1. ✅ Ensure MySQL is running
2. ✅ Create the "firstwebsite" database
3. ✅ Run migrations: `php artisan migrate`
4. ✅ Start your Laravel server: `php artisan serve`
5. ✅ Visit `http://localhost:8000/register` to test

## Additional Commands

```bash
# Clear all caches
php artisan cache:clear
php artisan config:clear
php artisan route:clear

# View all routes
php artisan route:list

# Generate new APP_KEY
php artisan key:generate

# Refresh migrations (CAUTION: Deletes all data)
php artisan migrate:refresh
```

## Security Reminders

⚠️ **For Production:**
- Change `APP_DEBUG=false` in .env
- Generate a strong APP_KEY
- Use environment variables for database credentials
- Enable HTTPS (SSL)
- Implement rate limiting for login attempts
- Add email verification for new accounts
- Implement password reset functionality
- Use strong password hashing (bcrypt is already configured)

## Support

For Laravel documentation, visit: https://laravel.com/docs

For questions about authentication, see: https://laravel.com/docs/11.x/authentication
