# Hockey Tournament Registration System #

A *PHP* + *MySQL* web application built for a Web Development in Unix course. This project allows users to create accounts, log in securely, register hockey teams, add players to those teams, and view a tournament dashboard with a sample game schedule.

---

## Project Summary ##

This application was designed as a hockey-themed tournament registration system with both front-end and back-end functionality. The project combines:
- user account creation and login
- password hashing for secure authentication
- session-based access control
- team registration tied to the logged-in user
- player registration tied to a team
- a MySQL database with foreign keys
- a themed dashboard and form-based interface

The goal of the project was to demonstrate practical full-stack web development in a Unix-style environment using *PHP*, *SQL*, *HTML*, and *CSS*.

---

## Features ##
- **User Registration:** New users can create an account.
- **User Login / Logout:** Existing users can authenticate and manage a session.
- **Dashboard:** Displays a hockey-themed landing page and a sample tournament schedule.
- **Team Registration**: Logged-in users can register one or more teams.
- **Player Registration:** Logged-in users can add players to their own teams.
- **Data Isolation:** Team lookups are filtered by the currently logged-in user.
- **Secure Password Storage:** Passwords are hashed using PHP's `password_hash()` and checked with `password_verify()`.

---

## Tech Stack ##
- **Backend:** PHP
- **Database:** MySQL
- **Frontend:** HTML, CSS
- **Authentication:** PHP sessions
- **Environment:** Unix / LAMP-style web development workflow

---

## File Structure ##
```text
hockey-tournament-registration-system/
├── dashboard.php
├── db_connect.php
├── login.php
├── logout.php
├── register.php
├── registration.php
├── schema.sql
└── README.md
```
## Database Design ##
### The database uses three main tables: ###
`users`
**Stores account information for each user.**
`id` - primary key
`username` - unique username
`password_hash` - securely stored hashed password
`email` - unique email address
`created_at` - timestamp
`teams`
**Stores teams created by users.**
`id` - primary key
`name` - team name
`division` - division or skill group
`coach_id` - foreign key linking the team to the user who created it
`created_at` - timestamp
`players`
**Stores players assigned to a team.**
`id` - primary key
`team_id` - foreign key linking the player to a team
`name` - player name
`age` - player age
`position` - player position
`created_at` - timestamp

---

## Security Concepts Used ##
### Password Hashing ###
Passwords are not stored in plain text. During registration, the password is hashed using:
```php
password_hash($password, PASSWORD_DEFAULT)
```
During login, the submitted password is verified against the stored hash using:
```php
password_verify($password, $user['password_hash'])
```
This follows secure PHP authentication practices and helps protect user credentials if the database is exposed.
### Session-Based Authentication ###
The application relies on PHP sessions through a separate `auth.php` helper file (referenced in the uploaded code). After login, user information is stored in the session so protected pages can verify that the user is authenticated.
###U ser-Specific Data Access ###
When teams are retrieved, the query filters results by the logged-in user's ID. This ensures that users only see teams they created.

---

## How the Application Works ##
**1. Account Creation**
A new user fills out the registration form in `register.php`. The application validates the input, checks whether the username already exists, hashes the password, and inserts the new account into the database.
**2. Login**
A returning user submits credentials through `login.php`. The application searches the database for the username, verifies the password hash, starts a logged-in session, and redirects the user to the dashboard.
**3. Dashboard**
`dashboard.php` shows different navigation depending on whether the user is logged in. Guests are prompted to create an account or log in. Logged-in users see a welcome message, a sample list of teams, and a sample tournament schedule.
**4. Team Registration**
In `registration.php`, authenticated users can submit a new team name and division. The new team is stored in the `teams` table with the current user's ID as `coach_id`.
**5. Player Registration**
The same page also allows users to select one of their teams and add players to it. This connects each player to a specific team in the `players` table.

---

## Reconstructed Setup Steps ##
*The exact classroom setup steps were not documented at the time, so the following process is reconstructed from the source code and typical Unix web development workflow using AI.*
1. Install / use a local web development environment
Use a Unix-compatible environment with:
- Apache or another PHP-capable web server
- PHP
- MySQL or MariaDB
2. Place the project files in your web root
Move the PHP files into your web server directory, such as:
```bash
/var/www/html/hockey-tournament-registration-system
```
3. Create the database
Import the schema:
```bash
mysql -u root -p < schema.sql
```
This creates the `hockey_tournament` database and the required `users`, `teams`, and `players` tables.

4. Configure database credentials
Update `db_connect.php` with the correct MySQL credentials:
```php
$host = "localhost";
$dbname = "hockey_tournament";
$user = "root";
$pass = "";
```
5. Add the session/authentication helper
The `auth.php` file handles:
- starting the PHP session
- checking whether the user is logged in
- returning the current user ID and username
- redirecting unauthenticated users
- clearing the session on logout
6. Run the app in a browser
Open the project in a browser through your local web server and test:
- account creation
- login
- team registration
- player registration
- logout

--- 

## What I Learned ##
This project helped reinforce important web development concepts, including:
- building multi-page PHP applications
- connecting PHP to MySQL with PDO
- using prepared statements for database queries
- securing passwords with hashing
- managing authentication with sessions
- designing relational database tables with foreign keys
- creating a themed interface while keeping forms readable and usable
