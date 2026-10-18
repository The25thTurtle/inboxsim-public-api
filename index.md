---
layout: default
---

# Inbox Sim - Public API Documentation

## Overview

The Inbox Sim Public API allows client applications to retrieve student projects, access simulated messages, submit assessment responses, and retrieve project information.

This documentation describes the endpoints, HTTP methods, inputs, outputs, and example requests.

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

## 2.1 Get Student Profile

**Endpoint:** `/students/profile/`

**Method:** `GET`

### Inputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | Unique ID of the student |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | Unique student identifier |
| name | String | student's display name |
| email | String | student's email address |
| success | Boolean | Indicates whether the request succeeded |

### Example Usage

`GET /students/profile/?student_id=123`

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
| success | Boolean | Indicates whether the request succeeded |
| projects | Array of Objects | Projects assigned to the student |

Each project object may contain:

| Field | Data Type | Description |
|---|---|---|
| project_id | Integer | Unique project identifier |
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

## 2.3 Get Message Details

**Endpoint:** `/students/messages/details`

**Method:** `GET`

### Inputs

| Name | Data Type | Description |
|---|---|---|
| message_id | Integer | Identifier of the message |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the request succeeded |
| message | Object | Details of the requested message |

The message object may contain `message_id` (Integer), `subject` (String), `sender` (String), and `body` (String).

### Example Usage

`GET /projects/messages/details/?message_id=45`

### Example Response

```json
{
  "success": true,
  "message": {
      "message_id": 45,
      "subject": "Customer Complaint",
      "sender": "Customer",
      "body": "I need help resolving an issue with my order."
    }
}
```

---

## 2.4 Submit Assessment Response

**Endpoint:** `/students/submit_response/`

**Method:** `POST`

### Inputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | Identifier of the student submitting the response |
| message_id | Integer | Identifier of the message being answered |
| response | String | Text of the student's response |

Inputs are submitted in the request body as JSON.

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the submission succeeded |
| result | String | Confirmation or error description |
| response_id | Integer | Identifier of the saved response, if successful |

### Example Usage

`POST /students/submit_response/`

Example request body: 

```json
{
  "student_id": 123,
  "message_id": 45,
  "response": "I would contact the customer to resolve the issue."
}
```

### Example Response

```json
{
  "success": true,
  "result": "Response submitted successfully.",
  "response_id": 501
}
```

---

## 2.5 Get Assessment Feedback

**Endpoint:** `/students/feedback/`

**Method:** `GET`

### Inputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | Identifier of the student |
| project_id | Integer | Identifier of the project |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the request succeeded |
| feedback | Array of Objects | Feedback available to the student |

Each feedback object may contain:

| Field | Data Type | Description |
|---|---|---|
| response_id | Integer | Identifier of the submitted response |
| comments | String | Feedback text |
| score | Number | Score, if released to the student |

### Example Usage

`GET /students/feedback/?student_id=123&project_id=10`

### Example Response

```json
{
    "success": true,
    "feedback": [
        {
            "response_id": 501,
            "comments": "Your response demonstrated good customer service.",
            "score": 85
        }
    ]
}
```

---

# 3. Faculty Endpoints

Faculty endpoints are intended for authorized faculty members. Access to student information and assessment results must be restricted according to the application's permissions.

## 3.1 Get Faculty Profile

**Endpoints:** `/faculty/profile/`

**Method:** `GET`

### Inputs 

| Name | Data Type | Description |
|---|---|---|
| faculty_id | Integer | Identifier of the faculty member |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the submission succeeded |
| faculty_id | Integer | Unique faculty identifier |
| name | String | Faculty member's display name |
| email | String | Faculty member's email address |

### Example Usage

`GET /faculty/profile/?faculty_id=45`

### Example Response

```json
{
    "success": true,
    "faculty_id": 45,
    "name": "Taylor Johnson",
    "email": "taylor@example.com"
}
```

---

## 3.2 Get Faculty Projects

**Endpoints:** `/faculty/projects/`

**Method:** `GET`

### Inputs 

| Name | Data Type | Description |
|---|---|---|
| faculty_id | Integer | Identifier of the faculty member |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the submission succeeded |
| projects | Array of Objects | Projects the faculty member is authorized to access |

Each project object may contain `project_id` (Integer), `project_name` (String), and `description` (String).

### Example Usage

`GET /faculty/projects/?faculty_id=45`

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

## 3.3 Get Student Assessment Results

**Endpoints:** `/faculty/student_results/`

**Method:** `GET`

### Inputs 

| Name | Data Type | Description |
|---|---|---|
| faculty_id | Integer | Identifier of the faculty member |
| project_id | Integer | Identifier of the project |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the submission succeeded |
| results | Array of Objects | Assessment results for students in the project |

Each result object may contain:
| Field | Data Type | Description |
|---|---|---|
| student_id | Integer | Student identifier |
| response_id | Integer | Submitted response identifier |
| score | Number | Assessment score |
| status | String | Submission or assessment status |

### Example Usage

`GET /faculty/student_results/?faculty_id=45&project_id=10`

### Example Response

```json
{
    "success": true,
    "results": [
        {
            "student_id": 123,
            "response_id": 501,
            "score": 85,
            "status": "Completed"
        }
    ]
}
```

---

## 3.4 Get Student Response Details

**Endpoints:** `/faculty/response_details/`

**Method:** `GET`

### Inputs 

| Name | Data Type | Description |
|---|---|---|
| faculty_id | Integer | Identifier of the faculty member |
| response_id | Integer | Identifier of the submitted response |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the request succeeded |
| response_id | Integer | Identifier of the response |
| student_id | Integer | Identifier of the student |
| message_id | Integer | Identifier of the original message |
| response | String | Student's submitted answer |
| score | Number | Recorded score, if available |
| feedback | String | Faculty feedback, if available |

### Example Usage

`GET /faculty/response_details/?faculty_id=45&response_id=501`

### Example Response

```json
{
    "success": true,
    "response_id": 501,
    "student_id": 123,
    "message_id": 45,
    "response": "I would contact the customer to resolve the issue.",
    "score": 85,
    "feedback": "Good customer service approach."
}
```

---

# 4. Project Endpoints

## 4.1 Get Project Details

**Endpoints:** `/projects/details/`

**Method:** `GET`

### Inputs 

| Name | Data Type | Description |
|---|---|---|
| project_id | Integer | Identifier of the project |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the request succeeded |
| project_id | Integer | Unique project identifier |
| project_name | String | Name of the project |
| description | String | Description of the project |
| status | String | Current project status, if available |

### Example Usage

`GET /projects/details/?project_id=10`

### Example Response

```json
{
    "success": true,
    "project_id": 10,
    "project_name": "Marketing Simulation",
    "description": "A simulated workplace inbox assessment",
    "status": "Active"
}
```

---


## 4.2 Get Project Messages

**Endpoint:** `/projects/messages/`

**Method:** `GET`

Retrieves the messages available to a student within a specific project. The messages returned may depend on the student's current progress through the simulation and its branching assessments.

### Inputs

| Name | Data Type | Description |
|---|---|---|
| project_id | Integer | Unique identifier of the project |
| student_id | Integer | Unique identifier of the student |
| current_state | String | Identifies the student's current position in the simulation |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the request succeeded |
| messages | Array of Objects | Messages available at the student's current stage |

Each message object may contain:

| Field | Data Type | Description |
|---|---|---|
| message_id | Integer | Unique identifier of the message |
| subject | String | Subject line of the message |
| sender | String | Name or label of the sender |
| body | String | Content of the message |

### Example Usage

`GET /projects/messages/?project_id=10&student_id=123&current_state=customer_complaint`

### Example Response

```json
{
  "success": true,
  "messages": [
    {
      "message_id": 25,
      "subject": "Customer Complaint",
      "sender": "Customer Support",
      "body": "A customer has reported an issue with their order."
    },
    {
      "message_id": 26,
      "subject": "Follow-Up Request",
      "sender": "Project Manager",
      "body": "Please review the complaint and determine the next steps."
    }
  ]
}
```

---

## 4.3 Get Project Assessment Information

**Endpoints:** `/projects/assessment/`

**Method:** `GET`

### Inputs 

| Name | Data Type | Description |
|---|---|---|
| project_id | Integer | Identifier of the project |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| success | Boolean | Indicates whether the request succeeded |
| assessment | Object | Assessment information available to the requester |

The assessment object may contain:

| Field | Data Type | Description |
|---|---|---|
| assessment_name | String | Name of the assessment |
| description | String | Assessment instructions or summary |
| total_questions | Integer | Number of questions or tasks, if applicable |

### Example Usage

`GET /projects/assessment/?project_id=10`

### Example Response

```json
{
    "success": true,
    "assessment": {
        "assessment_name": "Marketing Inbox Assessment",
        "description": "Respond to the simulated workplace messages.",
        "total_questions": 10
    }
}
```

---

# 5. General Error Handling

The API should return suitable HTTP status codes and error information. The exact response format should be confirmed by the development team.

| HTTP Status | Meaning |
|---|---|
| 200 OK | Request completed successfully |
| 201 Created | Resources created successfully |
| 400 Bad Request | Request contains invalid input |
| 401 Unauthorized | Authentication is missing or invalid |
| 403 Forbidden | Requester is not permitted to perform the operation |
| 404 Not Found | Requested resource does not exist |
| 500 Internal Server Error | Unexpected server error occurred |

### Example Error Response

```json
{
    "success": false,
    "error": "The requested project could not be found."
}
```