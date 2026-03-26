# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Sign up for activities

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Run the application:

   ```
   python app.py
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

### GET `/`
Redirects to the frontend application.

**Response:**
- **Status:** 307 (Temporary Redirect)
- **Location:** `/static/index.html`

---

### GET `/activities`
Retrieves all available extracurricular activities.

**Response:**
- **Status:** 200 OK
- **Content-Type:** `application/json`
- **Body:**
  ```json
  {
    "Activity Name": {
      "description": "string",
      "schedule": "string",
      "max_participants": "integer",
      "participants": ["email@example.com"]
    }
  }
  ```

**Example Response:**
```json
{
  "Chess Club": {
    "description": "Learn strategies and compete in chess tournaments",
    "schedule": "Fridays, 3:30 PM - 5:00 PM",
    "max_participants": 12,
    "participants": ["michael@mergington.edu", "daniel@mergington.edu"]
  }
}
```

---

### POST `/activities/{activity_name}/signup`
Signs up a student for a specific activity.

**Parameters:**
- `activity_name` (path, required): Name of the activity to sign up for
- `email` (query, required): Student email address

**Response:**
- **Status:** 200 OK (success) or 404 Not Found (activity doesn't exist)
- **Content-Type:** `application/json`
- **Body (Success):**
  ```json
  {
    "message": "Signed up {email} for {activity_name}"
  }
  ```

**Example Request:**
```
POST /activities/Chess%20Club/signup?email=john@mergington.edu
```

**Example Response:**
```json
{
  "message": "Signed up john@mergington.edu for Chess Club"
}
```

**Error Response (404):**
```json
{
  "detail": "Activity not found"
}
```

---

### Notes

- All activity names in requests must be URL-encoded
- The application stores data in memory; all changes are lost on server restart
- There are no current validations for:
  - Duplicate signups (same email can sign up multiple times)
  - Activity capacity limits (participants can exceed max_participants)

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
