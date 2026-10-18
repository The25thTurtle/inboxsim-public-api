---
layout: default
---

# Inbox Sim - Public API Documentation

## Overview

The Inbox Sim Public API allows client applications to retrieve student projects, access simulated messages, submit assessment responses, and retrieve project information.

This documentation decribes the endpoints, HTTP methods, inputs, outputs, and example requests.

### API Categories

- **Students:** Access assigned projects, retrieve messages, and submit responses.
- **Faculty:** View project information and assessment results, subject to permissions.
- **Projects:** Retrieve project details and assessment content.

### Authentication and Authorization

Endpoints that access student accounts, project content, or assessment results should require appropriate authentication and authorization. The specific authentication method must be confirmed by the development team.

### General Conventions

- HTTP methods describe the operation being performed.
- Integer values identify students, projects, and messages.
- String values contain text.
- Boolean values indicate true or false.
- Arrays contain collections of items.
- Objects contain related fields.

---

# 2. Student Endpoints

## 2.1 Get Student Projects

**Endpoint:** `/students/projects/`

**Method:** `GET`

### Inputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | Unique ID of the student |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | Unique student identifier |
| name | String | Students's display name |
| email | String | Students's email address |
| success | Boolean | Indicates whether the request succeed |

### Example Usage

`GET /students/profile/?students_id=123`

### Example Response

```json
{
    "success": true,
    "student_id": 123,
    "name": "Jordan Smith",
    "email": "jordan@example.com"
}
```

---

## 2.2 Get Student Projects

**Endpoint:** `/students/projects/`

**Method:** `GET`

### Inputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | Identifier of the students whose projects are requested |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the request succeed |
| projects | Array of Objects | Projects assigned to the student |

Each project object may contain:

| Field | Data Type | Description |
|---|---|---|
| project_id | Integer | Unique project indentifier |
| project_name | String | Name of the project |
| description | String | Summary of the project |

### Example Usage

`GET /students/projects/?student_id=123`

### Example Response

```json
{
  "success": true,
  "projects": [
    {
      "project_id": 10,
      "project_name": "Marketing Simulation",
      "description": "A simulated workplace inbox assessment"
    }
  ]
}
```

---

## 2.3 Get Student Messages

**Endpoint:** `/students/messages/`

**Method:** `GET`

### Inputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | Identifier of the student |
| project_id | Integer | Indentifier of the project |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the request succeed |
| messages | Array of Objects | Messages available to the student for the project |

Each message object may contain:

| Field | Data Type | Description |
|---|---|---|
| message_id | Integer | Unique message indentifier |
| subject | String | Subject line of the message |
| sender | String | Name or label of the sender |
| body | String | Message content |

### Example Usage

`GET /students/message/?student_id=123&project_id=10`

### Example Response

```json
{
  "success": true,
  "projects": [
    {
      "message_id": 45,
      "subject": "Customer Complaint",
      "sender": "Customer",
      "body": "I need help resolving an issue with my order."
    }
  ]
}
```

---
