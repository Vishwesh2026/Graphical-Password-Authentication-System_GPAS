# Graphical Password Authentication System

A modern, secure, and user-friendly web application that authenticates users via *graphical passwords* (clicking on grid points over a personalized image), providing an innovative alternative to traditional text-based logins. Built with HTML, CSS, JavaScript, PHP, and MySQL.

---

## **Table of Contents**

- [Project Motivation](#project-motivation)
- [Key Features](#key-features)
- [System Overview](#system-overview)
- [Project Structure](#project-structure)
- [Functional Workflow](#functional-workflow)
- [Database Schema](#database-schema)
- [Security Highlights](#security-highlights)
- [Installation & Setup](#installation--setup)
- [Contributors](#contributors)
- [License](#license)

---

## **Project Motivation**

Traditional text passwords are susceptible to various attacks (brute force, shoulder surfing, keylogging), hard for users to remember securely, and contribute to frequent support issues. This system leverages human visual memory and graphical interaction to create an authentication process that is **more secure** and **naturally memorable**, reducing risky user behaviors and raising the bar for security.

---

## **Key Features**

- **Graphical Password Creation:** Users select points on a grid overlaid on their own uploaded image.
- **Graphical Authentication:** Login requires users to reproduce the click pattern on the same image.
- **OTP-Based Password Recovery:** Secure, time-limited OTP is sent via email (PHPMailer) for password reset.
- **Modern Visual Design:** Responsive UI using glassmorphism, mobile-first layouts.
- **Comprehensive Security:** Password hashing, prepared statements, file validation, and session management.
- **Binary Image Management:** Profile images stored securely as BLOB in the database.

---

## **System Overview**

### Tech Stack

- **Frontend:** HTML5, CSS3 (with glassmorphism design), JavaScript (for interactivity)
- **Backend:** PHP 7+, MySQL, PHPMailer for mailing
- **Database:** MySQL with LONGBLOB storage for images and hashed coordinate strings for passwords

### Main Modules

- **Registration:** User data capture, image upload, graphical password selection & storage
- **Login:** Username validation, image retrieval, graphical password check
- **Password Recovery:** Email OTP verification for password reset (with image + new pattern)
- **Session/User Management:** Storing user state and guaranteeing secure transitions

---

## **Functional Workflow**

### 1. User Registration
- User fills in username, phone, email, uploads an image.
- Selects a sequence of grid points; system stores coordinates as a hashed string.
- All details (including LONGBLOB image and hashed password) are stored in `usersecret`.

### 2. Login
- User enters username; corresponding image is retrieved.
- User clicks their original pattern on the grid overlay.
- System validates clicks against stored hashed coordinates, logging user in on match.

### 3. Password Reset (Forgotten Password)
- User requests a reset, enters username.
- Secure OTP is emailed (valid for 5 minutes).
- Upon OTP verification, user uploads new image & selects new pattern, which are updated in the database.

---

## **Database Schema**

CREATE TABLE usersecret (
id INT AUTO_INCREMENT PRIMARY KEY,
username VARCHAR(255) NOT NULL UNIQUE,
phone VARCHAR(20) NOT NULL,
email VARCHAR(255) NOT NULL,
otp VARCHAR(6),
otp_expiration DATETIME,
picture LONGBLOB NOT NULL,
hash_password TEXT NOT NULL
);

---

## **Security Highlights**

- **Password Hashing:** Grid patterns are not stored raw, but as robust hashed strings.
- **Image Storage:** Images are handled securely, size-checked, and never stored in web-accessible paths.
- **Email OTP:** Reset links are time-limited and sent over secure PHPMailer SMTP.
- **SQL Injection Safe:** All database operations use parameterized queries.
- **Session Management:** User state is carefully tracked and cleaned up to prevent leakage.

---

## **Installation & Setup**

### Prerequisites
- PHP 7+
- MySQL (setup with provided `database.sql`)
- Composer (to install PHPMailer dependency)
- SMTP (Gmail recommended, update credentials in backend scripts)

### Steps

1. Clone this repository.
2. Run `composer install` to pull PHPMailer.
3. Import `database.sql` into your MySQL server.
4. Update `config.php` with your database connection details.
5. (Optional) Configure SMTP credentials in PHP scripts for OTP email.
6. Host files on Apache (XAMPP/LAMP/WAMP/other) with PHP and MySQL running.
7. Open `index.html` in your browser to begin.

---

