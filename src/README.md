# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Sign up for activities

## Getting Started

1. Install the dependencies:

   ```
   pip install -r requirements.txt
   ```

2. Configure an administrator account. Do not commit real credentials:

   ```
   export ADMIN_USERNAME=school-admin
   export ADMIN_PASSWORD='replace-with-a-unique-password'
   export SESSION_SECRET_KEY="$(openssl rand -hex 32)"
   ```

   Set `SESSION_COOKIE_SECURE=true` when serving the app over HTTPS. Leave it unset for local HTTP development.

3. Run the application:

   ```
   uvicorn app:app --reload --app-dir src
   ```

4. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc
   - Activities page: http://localhost:8000/

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Public activity list and participant details                       |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Public activity signup                                              |
| POST   | `/auth/login`                                                      | Sign in with configured administrator credentials                  |
| GET    | `/auth/me`                                                         | Check the current administrator session                            |
| POST   | `/auth/logout`                                                     | Sign out                                                            |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Unregister a student (administrator session required)           |

Administrator credentials are read from `ADMIN_USERNAME` and `ADMIN_PASSWORD`. Configure a stable, random `SESSION_SECRET_KEY` in deployed environments so sessions remain valid across restarts. Management endpoints must require the `require_admin` dependency; public activity browsing and signup do not.

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

All data is stored in memory, which means data will be reset when the server restarts.
