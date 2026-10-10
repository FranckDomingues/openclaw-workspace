# Delivery 03 — Skill Log

This document records the conversational design, implementation, API mapping and validation evidence for every 4Geeks skill created for OpenClaw/KAI.

No credentials or student tokens may be included in this file.

---

## Discovery Conversation

### Natural-Language Request

I asked OpenClaw/KAI to help design a secure integration with the 4Geeks Academy student account using the BreatheCode Student API.

The request explicitly required:

- secure token handling
- one API responsibility per skill
- starting with authentication
- reviewing the approach before implementation
- no hardcoded credentials
- no implementation until the proposed design was reviewed

### What OpenClaw Proposed

OpenClaw proposed:

- storing the student token in an environment variable named `BC_STUDENT_TOKEN`
- keeping local configuration outside Git
- creating a first focused skill named `breathecode-auth`
- validating authentication before building additional BreatheCode skills
- creating separate future skills for cohort, assignments and other student capabilities

OpenClaw initially suggested discovering the authentication endpoint empirically because it could not resolve the official API reference.

### Information Requested by OpenClaw

OpenClaw requested:

- the 4Geeks student token
- confirmation of the BreatheCode base URL
- confirmation of the Authorization header format

The token will not be shared through chat. It will be configured directly in the OpenClaw VPS environment.

### Review and Corrections

The proposed architecture was accepted with corrections after reviewing the official 4Geeks Student API reference.

Official API details:

- Base URL: `https://breathecode.herokuapp.com`
- Authorization format: `Authorization: Token <student_token>`
- Authentication/identity endpoint: `GET /v1/admissions/user/me`

The initially proposed `/v1/auth/user/me` endpoint was not adopted because the official project reference identifies `/v1/admissions/user/me` as the current-user endpoint.

The first skill will remain focused only on authentication and identity verification. Cohort retrieval and other student data will remain separate responsibilities.

### Result

Discovery completed.

Approved first skill:

`breathecode-auth`

Responsibility:

Verify that the configured 4Geeks student token is valid and return the authenticated student's basic identity using `GET /v1/admissions/user/me`.

Implementation has not started yet.

---

# Skill 1 — Authenticate (`breathecode-auth`)

## Natural-Language Request

> "Use the `breathecode-auth` skill now to verify my BreatheCode Student API authentication. Do not ask me for the token. Do not display or log the token or Authorization header."

## Skill Name

`breathecode-auth`

## Responsibility

Verify that the configured 4Geeks student token is valid and return the authenticated student's basic identity.

## API Endpoint(s)

`GET /v1/admissions/user/me`

Base URL:

`https://breathecode.herokuapp.com`

Authorization:

`Authorization: Token <student_token>`

## Implementation Files

- `skills/breathecode-auth/SKILL.md` — skill definition, workflow, output schema, safety rules
- `config/breathecode.json` — non-secret API configuration (base URL, auth endpoint, header format)

The student token is stored outside the repository in `.env` (gitignored).

## Security Behavior

- The token is read exclusively from the runtime environment (`.env`), never from code, arguments, or user input.
- The token value is never printed, logged, exposed in error messages, or included in any output.
- The `Authorization` header value is never displayed or logged.
- The raw API response body is never dumped to output.
- No cohort, assignment, lesson, progress, or other student data is fetched — only identity fields.
- If the environment variable is missing, the skill reports `Missing token` and does not attempt the API call.

## Test Results

### Valid token

- **Method:** Direct API call via KAI runtime with the configured token from `.env`
- **Endpoint:** `GET /v1/admissions/user/me`
- **HTTP Status:** `200`
- **Result:** Authenticated successfully
- **Identity returned:** `id`, `email`, `first_name`, `last_name`, `username`, `github.username`
- **Token exposed:** No
- **Authorization header exposed:** No
- **Non-identity data fetched:** No

### Missing token

- **Method:** Runtime environment without `BC_STUDENT_TOKEN` set
- **Result:** Correctly reports `Missing token` — no API call attempted

### Invalid token

- **Method:** API call with a fake token value
- **HTTP Status:** `401`
- **Result:** Correctly reports `Invalid or expired token`

### Final KAI runtime end-to-end test

The skill was invoked through KAI in the Telegram conversation. The runtime loaded the token from the approved `.env` mechanism, called the API, and returned the structured result:

```
Authentication: OK
Status: 200
Identity:
  • id: 21626
  • email: franciscomicros1@gmail.com
  • first_name: Francisco Alonso
  • last_name: Garcia Domingues
  • username: franciscomicros1@gmail.com
  • github.username: FranckDomingues
```

No token was requested from the user. No token or Authorization header was exposed. No non-identity student data was fetched.

## Validation

**Status: COMPLETED ✅**

All acceptance criteria satisfied:
- [x] Authentication endpoint validated: `GET /v1/admissions/user/me` returns HTTP 200 with valid token
- [x] Missing token correctly detected and reported without API call
- [x] Invalid/expired token correctly detected (HTTP 401) and reported
- [x] Only basic identity fields returned — no cohort, assignments, lessons, or progress data
- [x] Token never exposed in any output, log, or error
- [x] Authorization header never displayed or logged
- [x] Implementation files created: `skills/breathecode-auth/SKILL.md`, `config/breathecode.json`
- [x] Non-secret config trackable: `.gitignore` allows `config/breathecode.json` while keeping `config/mcporter.json` ignored
- [x] `.env` remains gitignored
- [x] Final end-to-end runtime test passed through KAI

---

# Skill 2 — Get Assigned Projects (`breathecode-projects`)

## Natural-Language Design and Implementation Requests

**Design request:** Design a second focused 4Geeks skill that answers “What projects are assigned to me?” without implementing it before conversational review. Keep it separate from authentication, pending-work logic, and progress summaries. Use the candidate Student API endpoint and inspect a successful authenticated response before assuming fields or pagination behavior. Do not request or expose the token.

**Approved implementation request:** Implement `breathecode-projects` against `GET /v1/assignment/user/me/task?task_type=PROJECT`, preserving the API's status values and returning only the approved project fields. Support pagination and safe error handling; do not calculate progress, infer pending work, include grades/submission history, modify Skill 1, or document/commit before implementation QA passes.

## Skill Name

`breathecode-projects`

## Single Responsibility

Retrieve the authenticated student's assigned project tasks and expose their current API-reported status. The skill does not calculate progress, infer pending work across task types, analyze grades or submission history, or fetch lessons/exercises.

## API Contract

- **Base URL:** `https://breathecode.herokuapp.com`
- **Endpoint:** `GET /v1/assignment/user/me/task`
- **Required filter:** `task_type=PROJECT`
- **Authentication:** `Authorization: Token <student_token>` using the securely configured runtime token.
- **Confirmed response shape:** paginated top-level object with `count`, `first`, `next`, `previous`, `last`, and `results`.
- **Confirmed project fields used:** `id`, `title`, `task_status`, `cohort.name`, `cohort.slug`, `revision_status`, and `delivered_at`.
- **Status handling:** preserve `task_status` exactly as returned by the API; do not translate or remap it.
- **Pagination:** supports `limit` and `offset`; exposes total `count` and only safe availability metadata for `next`/`previous`, never their URL values.

## Implementation Files

- `skills/breathecode-projects/SKILL.md` — focused skill definition and behavior.
- `config/breathecode.json` — added `projects_endpoint` and `default_page_size`, preserving the existing Skill 1 configuration keys.

## Security and Scope Behavior

- Reads the student token from the configured runtime environment; never asks for, hardcodes, prints, or logs it.
- Never displays the Authorization header or raw API response/error body.
- Returns only the confirmed allowlisted project fields and safe pagination metadata.
- Does not return grades, submission history, or unrelated personal data.
- Does not calculate progress, filter pending work by default, or fetch non-project task types.
- Handles missing token, HTTP 401, API/network errors, unexpected response shapes, and empty results safely.

## Test Results

### Valid token and real project parse

- **HTTP status:** `200`
- **Result:** paginated object parsed successfully; total count observed was `25`, with `20` items in the tested page.
- **Project parse:** at least one real item parsed using only approved output fields; `task_status` was preserved as returned.
- **Pagination:** `next` was available and `previous` was not available on the tested page; URL values were not exposed.
- **Token/header/raw response:** not exposed in the skill output.

### Missing token

- **Result:** PASS — reports missing token and skips the API call.

### Invalid token

- **Test input:** fake token only (the configured token was not used for this case).
- **HTTP status:** `401`
- **Result:** PASS — classified as invalid/expired; response body not exposed.

### Empty results

- **Test:** empty paginated-response fixture; not a live API query.
- **Result:** PASS — reports no assigned projects as a successful empty result.

### Final KAI runtime end-to-end test

- **Result:** PASS — `E2E_SKILL_TEST: PASS`.
- Returned the project list using only approved fields and safe pagination metadata.
- No raw JSON, token, Authorization header, progress calculation, pending-only filtering, grade, or submission-history analysis was included.

## Validation

**Status: COMPLETED ✅**

- [x] Approved single responsibility and API contract implemented.
- [x] Successful authenticated response and real project item parsed.
- [x] Missing-token, fake-token/401, and empty-fixture paths tested.
- [x] KAI runtime end-to-end test passed.
- [x] Token, Authorization header, raw response, grades, and submission history not exposed.
- [x] Pagination metadata kept safe; `task_status` preserved as returned.
- [ ] Commit created — not part of this implementation/QA step.

---

# Skill 3 — Get Pending Work

## Natural-Language Request

Pending.

## Purpose

Identify the student's incomplete or pending course work.

## API Endpoint(s)

Pending discovery.

## Implementation

Pending.

## Test Request

Pending.

## Test Result

Pending.

## Validation

Pending.

---

# Skill 4 — Get Progress Summary

## Natural-Language Request

Pending.

## Purpose

Provide a high-level view of the student's current course progress.

## API Endpoint(s)

Pending discovery.

## Implementation

Pending.

## Test Request

Pending.

## Test Result

Pending.

## Validation

Pending.

---

# Extended Skill 1

## Identified Need

Pending discovery.

## Natural-Language Request

Pending.

## Purpose

Pending.

## API Endpoint(s)

Pending.

## Implementation

Pending.

## Test Request

Pending.

## Test Result

Pending.

## Validation

Pending.

---

# Extended Skill 2

## Identified Need

Pending discovery.

## Natural-Language Request

Pending.

## Purpose

Pending.

## API Endpoint(s)

Pending.

## Implementation

Pending.

## Test Request

Pending.

## Test Result

Pending.

## Validation

Pending.

---

# Final Validation

- [ ] Four required skills implemented
- [ ] Two extended skills implemented
- [ ] All skills tested against real API data
- [ ] Each skill has one clear responsibility
- [ ] No credentials committed
- [ ] All prompts documented
- [ ] All endpoints documented
- [ ] All test results documented
