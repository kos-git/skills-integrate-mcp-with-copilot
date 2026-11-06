# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Sign up for activities

## Getting Started

Configure the teacher credentials and session signing key before starting the API. Credentials are supplied as a JSON object through environment variables and are not stored in the repository:

```sh
export TEACHER_CREDENTIALS='{"teacher":"replace-with-a-strong-password"}'
export SESSION_SECRET="$(python -c 'import secrets; print(secrets.token_urlsafe(32))')"
```

For HTTPS deployments, also set `COOKIE_SECURE=true` so the session cookie is sent only over HTTPS. Without `SESSION_SECRET`, the app generates a temporary key and all teacher sessions expire when it restarts.

1. Install the dependencies:

   ```
   pip install -r requirements.txt
   ```

2. Run the application:

   ```
   uvicorn src.app:app --reload
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| GET    | `/auth/status`                                                     | Check whether the current session is a teacher session               |
| POST   | `/auth/login`                                                      | Log in with a configured teacher account                             |
| POST   | `/auth/logout`                                                     | End the current teacher session                                      |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Register a student (teacher login required)                         |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Unregister a student (teacher login required)                    |

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
