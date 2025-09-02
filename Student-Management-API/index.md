> This document was authored in Markdown format to demonstrate my ability to create developer-friendly documentation.

# School Management API – Auth & Role Dashboards

The School Management API enables integration of authentication and role-based operations in an educational platform. This document provides sample endpoints for login, logout, and dashboards for students, teachers, and admins, along with standard error responses.

## Base URL
https://api.schoolmanager.com/v1

## Authentication
### This API uses JWT bearer tokens.

- On successful login, the API returns access_token (JWT) and expires_in (seconds).
- Include the token in all protected requests:

Authorization: Bearer <ACCESS_TOKEN>

## Endpoints

### 1.Admin Login
POST /auth/admin/login

### Body

{
  "email": "admin@example.com",
  "password": "StrongPassword!23"
}

### Response 200

{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "role": "admin",
  "admin": {
    "id": "adm_001",
    "name": "School Admin",
    "email": "admin@example.com"
  }
}

### 2.Teacher Login
POST /auth/teachers/login

### Body

{
  "email": "teacher.rahul@example.com",
  "password": "StrongPassword!23"
}


### Response 200

{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "role": "teacher",
  "teacher": {
    "id": "tch_102",
    "name": "Rahul Verma",
    "email": "teacher.rahul@example.com",
    "subjects": ["Physics", "Mathematics"]
  }
}

### 3.Student Login
POST /auth/students/login

### Body
{
  "email": "ananya.sharma@example.com",
  "password": "StrongPassword!23"
}

### Response 200

{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "role": "student",
  "student": {
    "id": "std_345",
    "name": "Ananya Sharma",
    "email": "ananya.sharma@example.com",
    "grade": "10"
  }
}

### 4.Logout (all roles)
POST /auth/logout

### Headers

Authorization: Bearer <ACCESS_TOKEN>
### Response 204
No content.

### Role Dashboards
### 1.Student Dashboard

See enrolled courses, own details, and assigned teachers
GET /students/me
#### Headers
Authorization: Bearer <ACCESS_TOKEN>

#### Response 200
{
  "id": "std_345",
  "name": "Ananya Sharma",
  "email": "ananya.sharma@example.com",
  "grade": "10",
  "enrollments": [
    {
      "course_id": "crs_101",
      "course_name": "Mathematics",
      "teacher": {
        "id": "tch_102",
        "name": "Rahul Verma",
        "email": "teacher.rahul@example.com"
      }
    },
    {
      "course_id": "crs_202",
      "course_name": "English",
      "teacher": {
        "id": "tch_089",
        "name": "Priya Mehta",
        "email": "priya.mehta@example.com"
      }
    }
  ]
}

### 2.Teacher Dashboard

See own students’ details and lecture schedule
GET /teachers/me

#### Headers
Authorization: Bearer <ACCESS_TOKEN>

#### Response 200
{
  "id": "tch_102",
  "name": "Rahul Verma",
  "email": "teacher.rahul@example.com",
  "courses": [
    {
      "course_id": "crs_101",
      "course_name": "Mathematics",
      "students": [
        {"id": "std_345", "name": "Ananya Sharma", "grade": "10"},
        {"id": "std_412", "name": "Vikram Singh", "grade": "10"}
      ]
    }
  ],
  "schedule": [
    {
      "lecture_id": "lec_9001",
      "course_id": "crs_101",
      "topic": "Algebra – Quadratic Equations",
      "start_time": "2025-09-01T09:00:00Z",
      "end_time": "2025-09-01T10:00:00Z",
      "room": "A-204"
    }
  ]
}



### 3.Admin Dashboard

#### Full control for teachers & students (create, update, assign) 
#### Get overview

GET /admin/overview

#### Headers
Authorization: Bearer <ACCESS_TOKEN>

#### Response 200
{
  "stats": {
    "students": 520,
    "teachers": 38,
    "courses": 64
  },
  "recent_activity": [
    {"type": "enrollment", "student_id": "std_412", "course_id": "crs_101", "timestamp": "2025-08-28T11:22:10Z"}
  ]
}
### Error Responses (common)
|Status |Code|Mesage|
| ----- | --- | ----- |
|400 |bad_request |Invalid payload|
|401 |unauthorized |Missing/invalid token|
|403 |forbidden |Insufficient permissions|
|404 |not_found |Resource not found|
|409 |conflict |Duplicate or invalid state|
|422 |validation_error |Field validation failed|
|500 |server_error |Unexpected server errord|

### Error body example

{
  "error": "validation_error",
  "message": "Email is required"
}



