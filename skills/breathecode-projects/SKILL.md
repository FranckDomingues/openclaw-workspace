---
name: breathecode-projects
description: "Retrieve the authenticated student's assigned project tasks from the BreatheCode API and expose their current API-reported status."
---

# BreatheCode Projects — Assigned Project Tasks

Retrieve the authenticated student's assigned `PROJECT` tasks from the BreatheCode Student API. Return only the confirmed project fields and the total result count. This skill does not calculate progress, determine pending work, analyze grades, or inspect submission history.

## API contract

- Base URL and endpoint are read from `config/breathecode.json` (`base_url`, `projects_endpoint`).
- Request: `GET {base_url}{projects_endpoint}?task_type=PROJECT&limit={limit}&offset={offset}`
- Authentication: `Authorization: {auth_header_format} <BC_STUDENT_TOKEN>`.
- `BC_STUDENT_TOKEN` must be available to the runtime (secure environment configuration, such as the existing `.env` mechanism). Never request it in chat.
- The observed success response is a paginated object with `count`, `first`, `next`, `previous`, `last`, and `results`; `results` is a list.
- Each observed item has `id`, `title`, `task_status`, `revision_status`, `delivered_at`, and `cohort` (object with `id`, `name`, `slug`).

## Request parameters

- Always set `task_type=PROJECT`; callers cannot override this filter.
- Accept optional non-negative integer `limit` and `offset`.
- If `limit` is omitted, use `default_page_size` from `config/breathecode.json` (20 in the current configuration). If `offset` is omitted, use `0`.
- Reject invalid parameter values locally; do not make an API request with malformed pagination parameters.
- Do not follow server-supplied `next` or `previous` URLs. A subsequent page is requested by making a new request to the configured endpoint with the appropriate `limit` and `offset`.

## Workflow

1. Read the student token from the runtime environment using the configured secure mechanism. If missing or empty, return `Missing token` and do not call the API.
2. Read the base URL, endpoint, auth header format, and default page size from `config/breathecode.json`. Do not remove or alter the keys used by `breathecode-auth`.
3. Build the request with the fixed `task_type=PROJECT` filter and validated `limit`/`offset`. Send the token only in the Authorization header. Never log or display the token or header.
4. Handle HTTP and network outcomes:
   - HTTP 200: validate the expected top-level object and pagination fields before processing results.
   - HTTP 401: report `Invalid or expired token` without displaying the response body.
   - Other HTTP statuses, network failures, timeouts, invalid JSON, or unexpected response structure: report a safe `API/network error`; do not output raw response content or partially parsed project data.
5. For each result, output only `id`, `title`, `task_status`, `cohort.name` and/or `cohort.slug`, `revision_status`, and `delivered_at`, where present. `task_status` must be reproduced exactly as returned; never translate, normalize, or remap it. Do not output `cohort.id` or any other API field.
6. Display `count` as the API-reported total. Display only whether `next` and/or `previous` navigation is available (yes/no); never show their URL values or query strings.
7. If `count` is zero or `results` is empty, return `No assigned projects found` as a successful empty result, not an error.

## Output format

```text
Authentication: OK | FAILED
Status: 200 | 401 | error
Projects:
  Count: <API count>
  Items:
    - id: <value>
      title: <value>
      task_status: <exact API value>
      cohort.name: <value, if present>
      cohort.slug: <value, if present>
      revision_status: <value, if present>
      delivered_at: <value, if present>
  Pagination:
    next available: yes | no
    previous available: yes | no
Error: (on failure only)
  Reason: missing token | invalid/expired token | API/network error
  Detail: <brief safe diagnostic>
```

For an empty successful result, retain `Authentication: OK` and `Status: 200`, show the API count and navigation availability, and state `No assigned projects found`.

## Safety and scope

- Never print, log, expose, hardcode, or ask for the token; never output the Authorization header.
- Never dump raw JSON or API error bodies.
- Never expose raw `next`/`previous` URLs; use booleans only, so signed/private query information cannot leak.
- Output only the allowlisted project fields above. Do not return personal account data, grades, submission records/history, or other task fields.
- Do not calculate progress, infer pending work, or aggregate across task types. Preserve the API's `task_status` value exactly.
- Do not fetch lessons, exercises, or any non-project task type.
- Do not retry authentication failures or network errors blindly.

## Error and shape handling

- Missing/empty token: `Authentication: FAILED`, `Status: error`, reason `missing token`; no request.
- HTTP 401: `Authentication: FAILED`, `Status: 401`, reason `invalid/expired token`.
- Other HTTP status, transport failure, timeout, malformed JSON, missing required pagination keys, or `results` not a list: `Authentication: FAILED`, reason `API/network error`; include only a brief safe detail and status where available.
- A valid empty result is not an error.
- If a result item is not an object or has unexpected nested cohort type, fail safely without displaying raw/partial API data.