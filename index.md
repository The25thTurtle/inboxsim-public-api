---
layout: default
---

# Inbox Sim - Public API Documentation

Welcome to the Inbox Sim Public API Documentation.
...

## Students

### Get Student Projects

**Endpoint:** `/students/projects/`

**Method:** `GET`

### Inputs

| Name | Data Type | Description |
|---|---|---|
| student_id | Integer | The ID of the student |

### Outputs

| Name | Data Type | Description |
|---|---|---|
| projects | Array | The student's assigned projects |
| success | Boolean | Whether the request succeeded |

### Example Usage

```http
GET /students/projects/?student_id=123
```

---

## Projects

More project endpoints will be documented here.
