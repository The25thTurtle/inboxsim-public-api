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

