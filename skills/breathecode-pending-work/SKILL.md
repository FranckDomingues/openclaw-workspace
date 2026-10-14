---
name: breathecode-pending-work
description: "List the authenticated student's currently pending BreatheCode tasks across task types and cohorts, using only API-reported status."
---

# BreatheCode Pending Work

List tasks that the BreatheCode Student API explicitly reports with `task_status=PENDING`. Query across all task types and cohorts by default. This skill reports API data only; it does not infer pending status from other values or calculate progress.

## API contract

- Read `base_url`, `projects_endpoint`, `auth_header_format`, and `default_page_size` from the existing `config/breathecode.json`. `projects_endpoint` is the shared `/v1/assignment/user/me/task` route; do not alter or remove Skill 1/2 settings.
- Request: `GET {base_url}{projects_endpoint}?task_status=PENDING&limit={limit}&offset={offset}`.
- Always include `task_status=PENDING`. Do not add a `task_type` or cohort filter unless explicitly requested by the user.
- Authentication: the configured authorization scheme and `BC_STUDENT_TOKEN`, provided securely to the runtime (existing `.env` mechanism or exported environment). Never request the token in chat.
- The observed response is a top-level object containing `count`, `first`, `next`, `previous`, `last`, and `results`; `results` is a list.
- Observed pending task types include `EXERCISE` and `LESSON`. Preserve each API-returned type and status exactly; do not treat other statuses as pending.
- Observed items include `id`, `title`, `task_type`, `task_status`, and `cohort`; cohort is an object with `id`, `name`, and `slug`.

## Request parameters and pagination

- Accept optional non-negative integer `limit` and `offset`; default to `default_page_size` (20 in current config) and `0` respectively.
- Reject invalid pagination inputs locally.
- For subsequent pages, make another request to the configured endpoint with the same `task_status=PENDING` filter and the chosen `limit`/`offset`.
- Do not follow server-supplied pagination URLs. Expose only whether `next` and `previous` are available; never reveal those URL values or query strings.

## Workflow

1. Read the token from the runtime environment using the configured secure mechanism. If missing or empty, return a missing-token result without making an API request.
2. Load the shared configuration. Do not modify it as part of this skill invocation.
3. Build the request using `task_status=PENDING`, `limit`, and `offset`; omit `task_type` and cohort filters by default. Send the token only in the Authorization header. Never log or display the token or header.
4. Classify the response:
   - HTTP 200: validate the paginated-object shape, required pagination keys, and that `results` is a list before rendering.
   - HTTP 401: report invalid/expired authentication; do not expose the response body.
   - Other HTTP statuses, connection errors, timeouts, invalid JSON, or unexpected structure: report a safe API/network error and do not output raw data or partially parsed items.
5. For each result item, output only fields confirmed for this skill: `id`, `title`, `task_type`, `task_status`, and, if present, `cohort.name` and/or `cohort.slug`. Do not output `cohort.id` or any other API fields.
6. Preserve `task_type` and `task_status` exactly as returned by the API. Do not translate, normalize, or infer status.
7. Show the API-reported total `count` and boolean availability of next/previous pages. Never show pagination URLs.
8. If the API returns `count=0` or an empty `results` list, report no pending work as a successful empty result.

## Output format

```text
Authentication: OK | FAILED
Status: 200 | 401 | error
Pending work:
  Count: <API-reported total>
  Items:
    - id: <value>
      title: <value>
      task_type: <exact API value>
      task_status: <exact API value>
      cohort.name: <value, if present>
      cohort.slug: <value, if present>
  Pagination:
    next available: yes | no
    previous available: yes | no
Error: (on failure only)
  Reason: missing token | invalid/expired token | API/network error
  Detail: <brief safe diagnostic>
```

For successful empty results, retain `Authentication: OK` and `Status: 200`, show the API count and safe pagination availability, and state `No pending work found`.

## Safety and scope

- Never print, log, expose, hardcode, or request the student token; never display the Authorization header.
- Never dump raw JSON or API error bodies.
- Never reveal raw pagination URLs or query strings.
- Return only the approved task fields; do not expose grades, submission history, descriptions, links, or unrelated personal information.
- Never add a task-type or cohort restriction by default.
- Never classify a status other than the API-returned `PENDING` as pending; preserve status and task type strings exactly.
- Do not calculate progress, completion percentages, or summaries; do not fetch extra endpoints or unrelated task data.
- Do not retry blindly on authentication or network failures.

## Error and shape handling

- Missing/empty token: `Authentication: FAILED`, reason `missing token`; no request is made.
- HTTP 401: `Authentication: FAILED`, reason `invalid/expired token`; response body is withheld.
- Other HTTP status, transport failure, timeout, malformed JSON, missing pagination keys, or a non-list `results`: `Authentication: FAILED`, reason `API/network error`, with only a brief safe detail.
- A valid empty page is not an error.
- If any result item is not an object or `cohort` has an unexpected non-null type, fail safely without exposing raw or partial result content.