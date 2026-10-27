# N8N CRUD Verification Workflow

## Overview
This beginner-level n8n workflow demonstrates four HTTP API methods against the ReqRes mock API: GET, POST, PUT, and DELETE. It starts manually and runs each HTTP Request node in sequence.

## Workflow

```text
Manual Trigger
  -> HTTP_GET_Users
  -> HTTP_POST_CreateUser
  -> HTTP_PUT_UpdateUser
  -> HTTP_DELETE_DeleteUser
```

## Requirements
- An n8n instance with HTTP Request nodes available
- Access to `https://reqres.in`
- ReqRes authentication/API key if required by the service at execution time

## Node configuration

### 1. Manual Trigger
Starts the workflow when **Execute workflow** is clicked in the n8n editor.

### 2. HTTP_GET_Users
- Method: `GET`
- URL: `https://reqres.in/api/users`
- Query parameter: `page=2`
- Purpose: Retrieve page 2 of the mock user list.

### 3. HTTP_POST_CreateUser
- Method: `POST`
- URL: `https://reqres.in/api/users`
- Body type: JSON

```json
{"name":"Alice Smith","job":"Software Engineer"}
```

- Purpose: Submit a mock user creation request and receive a generated ID and timestamp.

### 4. HTTP_PUT_UpdateUser
- Method: `PUT`
- URL: `https://reqres.in/api/users/26` in the recorded test
- Body type: JSON

```json
{"name":"Alice Smith","job":"Senior Software Engineer"}
```

- Purpose: Demonstrate a mock user update.
- Recommended dynamic URL for future runs: `{{ 'https://reqres.in/api/users/' + $('HTTP_POST_CreateUser').first().json.id }}`

### 5. HTTP_DELETE_DeleteUser
- Method: `DELETE`
- URL: `https://reqres.in/api/users/26` in the recorded test
- Request body: Off
- Purpose: Demonstrate a mock user deletion request; a successful response may have an empty body.
- Recommended dynamic URL for future runs: `{{ 'https://reqres.in/api/users/' + $('HTTP_POST_CreateUser').first().json.id }}`

## How to execute
1. Import the exported workflow JSON into n8n, or configure the nodes above.
2. Confirm ReqRes connectivity and configure authentication if prompted.
3. Click **Execute workflow**.
4. Inspect each node's output and confirm the expected response.
5. Capture the final canvas showing green checkmarks.

## Observed test results (27 October 2026)

| Test | Evidence | Result |
|---|---|---|
| GET page 2 | Returned user records and pagination | Passed |
| POST create user | Returned Alice Smith, ID `26`, and `createdAt` | Passed |
| PUT update user | Returned Senior Software Engineer and `updatedAt` | Passed |
| DELETE user | Completed with empty JSON output | Passed |
| End-to-end execution | All five nodes show success checkmarks | Passed |

## Important scope note
ReqRes is a mock API. The successful POST, PUT, and DELETE responses demonstrate HTTP request handling but do not prove persistent database changes. The screenshots also do not independently display HTTP status codes; status values should be captured separately if required by the assessment.

## Submission files
- `N8N_CRUD_Verification_Evidence.pdf`: screenshots and 1-2 line explanations
- `N8N_CRUD_Verification_README.md`: setup and usage guide
- Exported n8n workflow JSON: export separately from your n8n editor
