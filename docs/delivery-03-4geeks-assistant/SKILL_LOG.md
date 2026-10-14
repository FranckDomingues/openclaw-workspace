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

# Skill 3 — Get Pending Work (`breathecode-pending-work`)

## Natural-Language Design and Implementation Requests

**Design request:** Design a focused Skill 3 for “Get Pending Work” through conversational discovery. Do not implement until the endpoint response has been safely inspected. Determine task types and fields from a real authenticated response; do not invent them. Keep it separate from assigned-project listing and overall progress, and never request or expose the student token.

**Approved implementation request:** Create `breathecode-pending-work` to query `GET /v1/assignment/user/me/task` with the required `task_status=PENDING` filter, without restricting task type or cohort. Return only the approved task fields, preserve API-returned task types/statuses, include count and safe pagination availability, handle missing/invalid authentication and empty results, and do not calculate progress or expose raw response data. Do not modify Skills 1 or 2 or commit.

## Skill Name

`breathecode-pending-work`

## Single Responsibility

Retrieve tasks for which the BreatheCode API reports `task_status=PENDING`, across task types and cohorts. Do not infer that any other status is pending, calculate progress, or analyze grades or submission history.

## API Contract and Observed Response

- **Endpoint:** `GET /v1/assignment/user/me/task`
- **Required filter:** `task_status=PENDING`; no `task_type` or cohort filter by default.
- **Confirmed response:** HTTP 200; paginated top-level object with `count`, `first`, `next`, `previous`, `last`, and `results`.
- **Observed pending task types:** `EXERCISE`, `LESSON`.
- **Observed status:** `PENDING`.
- **Observed cohort structure:** object with `id`, `name`, and `slug`.
- **Approved output fields:** `id`, `title`, `task_type`, `task_status`, `cohort.name`, and/or `cohort.slug`; API `count`; boolean next/previous availability.
- **Pagination:** supports `limit` and `offset`. Raw pagination URLs are not returned.

## Implementation

- `skills/breathecode-pending-work/SKILL.md` — focused procedure, response-field allowlist, pagination and error handling.
- Reuses the existing `config/breathecode.json`; no additional configuration was required.

## Security and Scope Behavior

- Uses the configured token securely from the runtime environment; never asks for, prints, or logs the token.
- Never displays the Authorization header, raw JSON/API error body, or pagination URL.
- Returns only the approved task fields; no grades, submission history, or other task fields.
- Preserves `task_type` and `task_status` exactly as returned. Does not add task-type/cohort restrictions or infer pending from another status.
- Does not calculate progress or fetch unrelated data.

## Test Results

### Valid token and real pending-task parse

- **Authentication endpoint:** HTTP `200`.
- **Pending-work endpoint:** HTTP `200`.
- **API-reported total:** `48`; `20` results on the tested page.
- **Task types observed:** `EXERCISE`, `LESSON`.
- **Task status observed:** `PENDING`.
- Real task data was parsed using only the approved output fields; safe next/previous availability was returned.

### Missing token

- **Result:** PASS — reports a missing token and makes no API request.

### Invalid token

- **Test input:** fake invalid token.
- **HTTP status:** `401`.
- **Result:** PASS — correctly classified as invalid/expired; response body withheld.

### Empty results

- **Test:** empty paginated-response fixture (not a live API empty result).
- **Result:** PASS — treated as a successful empty result, not an error.

### Final KAI runtime end-to-end test

- **Request:** “What work do I currently have pending?”
- **Result:** PASS — `E2E_SKILL_TEST: PASS`.
- Returned only approved fields, preserved task types/statuses, and provided safe next/previous availability.
- No raw JSON, pagination URLs, token, Authorization header, grades, submission history, or progress calculations were returned.

## Validation

**Status: COMPLETED ✅**

- [x] Approved API contract and single responsibility implemented.
- [x] Authenticated pending endpoint returned HTTP 200 and real tasks were parsed.
- [x] Missing-token, fake-token/401, and empty-fixture paths passed.
- [x] Final KAI runtime end-to-end test passed.
- [x] Output remained within the approved field/security boundaries.
- [ ] Commit created — not part of this implementation/QA step.

---

# Skill 4 — Get Progress Summary

## Natural-Language Request

> "Give me a high-level progress summary across all my BreatheCode tasks: total tasks and counts and percentages by task_status, task_type, and cohort. Preserve API labels, keep DONE and APPROVED separate, fetch every page, and do not return individual tasks or expose credentials."

Approved implementation scope: create only `breathecode-progress-summary`, reuse
the existing secure token/configuration mechanism, validate live and controlled
cases, and update this log and the checklist. Do not modify Skills 1-3, unrelated
OpenClaw files, stage, or commit.

## Purpose

Produce aggregate progress across all authenticated student tasks. It does not
list projects or pending tasks, infer completion, combine statuses, or analyze
grades/submission history. Separate responsibility from `breathecode-projects`
and `breathecode-pending-work`.

## API Endpoint(s)

- `GET /v1/assignment/user/me/task` at the existing configured BreatheCode base URL.
- Query parameters: `limit` and `offset` only; no status, type, or cohort filter.
- Uses the existing `BC_STUDENT_TOKEN` runtime environment and configured Token
  authentication scheme. No additional endpoint or service is required.

## Implementation

- Created `skills/breathecode-progress-summary/SKILL.md` with a focused workflow,
  output contract, and executable standard-library Python reference procedure.
- Reuses `config/breathecode.json` unchanged, including `projects_endpoint` as
  the shared task route and `default_page_size=20`.
- Pagination starts at zero and advances offset by the received page length.
  Requests are rebuilt against the configured endpoint; server pagination URLs
  are never followed or exposed. Redirects are disabled. Timeout is 30 seconds
  per request; safety cap is 1,000 pages.
- Validates response shape, stable API count, continuation consistency, page
  size, and repeated task IDs internally when present. Early stop, count drift,
  repeated tasks, cap, or sum mismatch returns `INCOMPLETE` with no partial totals.
- Requires every task to contribute to status, type, and cohort counts. Status
  and type strings are preserved verbatim, including new labels. Missing or
  non-string status/type values fail safely instead of being omitted.
- Cohorts use the exact name/slug pair. Missing/null/unlabeled cohorts remain in
  `Absent/unknown cohort`; no task is dropped. Invalid cohort structures fail.
- Every bucket percentage is `round(count / total_tasks * 100, 1)`, displayed
  with one decimal. Zero tasks is successful: total 0, all sums 0, and each empty
  dimension displays `No buckets: 0 (0.0%)`. No division by zero.
- Independently verifies all three count sums equal collected total, which
  must equal API count. Rounded percentages need not sum to exactly 100.0%.
- Pagination is not an atomic snapshot; undetectable same-count changes are
  not claimed to be prevented.

## Output and Security Contract

- Successful output: authentication/status, COMPLETE, total tasks, aggregate
  counts and percentages by exact status/type and cohort labels, sum-check
  result, and pages fetched. No individual task records.
- Errors: missing token without a request; invalid/expired token on HTTP
  401/403; safe API/network, malformed JSON, unexpected shape, configuration,
  and pagination-cap failures. Withhold all aggregates on failure, explicitly
  marking interrupted/inconsistent aggregation INCOMPLETE. No blind retry.
- Never request, hardcode, print, log, or persist the token or auth header.
  Never output task titles/IDs, grades, submission history, raw JSON/error
  bodies, exception details, or raw pagination URLs. Labels are inert data.
- Live QA loaded the existing ignored `.env` with shell tracing disabled;
  responses were processed only in memory and output was aggregate-only.

## Test Request

Run read-only QA against the exact Python block embedded in the new skill:
execute it in memory under a non-main module name, inject page fetchers for
controlled cases, then use its real GET fetcher with the securely loaded token.
Controlled cases use no student data and do not write fixtures to the repository.

## Test Result

**QA date:** 2026-10-14. **Controlled QA:** PASS. **Live API QA:** PASS.

### Live token and real pagination

- HTTP 200 on all 7 pages; limit 20; offsets 0, 20, 40, 60, 80, 100, 120.
- Collected total and API count both **130**; final page contains 10 tasks.
- Observed task statuses, verbatim: `DONE` **82 (63.1%)**, `PENDING`
  **48 (36.9%)**. `APPROVED` was not observed live and is not inferred.
- Observed task types, verbatim: `EXERCISE` **90 (69.2%)**, `LESSON`
  **15 (11.5%)**, `PROJECT` **25 (19.2%)**.
- 12 cohort buckets; counts in returned order:
  20, 29, 5, 12, 17, 20, 6, 7, 4, 4, 3, 3.
- Cohort percentages in that same order:
  15.4%, 22.3%, 3.8%, 9.2%, 13.1%, 15.4%, 4.6%, 5.4%, 3.1%, 3.1%, 2.3%, 2.3%.
- Status sum **130**, type sum **130**, cohort sum **130**: PASS.
- Verified sequential offsets, equality with API count, and aggregate output
  without token, auth header, URLs, or individual task fields.

### Authentication and controlled cases

- Missing token: PASS; no fetcher call; main output reports missing token and
  contains no total. Runtime environment was temporarily cleared for this test.
- Fake invalid token: live HTTP **401**, classified invalid/expired; API error
  body withheld. No configured token used in this case.
- Empty paginated fixture: PASS; total 0, three sums 0, three empty dimensions
  show **0 (0.0%)**; successful result, not an error.
- Two-page fixture: PASS; offsets 0 and 2; `DONE`, `APPROVED`, and a custom
  status remain separate verbatim buckets; custom type preserved; 1/3 rounds
  to **33.3%**. Missing cohorts aggregate as **2 (66.7%)**.
- Forced pagination cap, absent continuation, and premature empty page:
  PASS; INCOMPLETE, not final partial totals.
- Unexpected top-level shape and null status: PASS; safe shape error.
- Deliberate sum mismatch: PASS; independent validator rejects it as INCOMPLETE.
- Simulated network failure: PASS; generic error without exception disclosure.
- Redirect rejection and skill name/description frontmatter: PASS.

These tests execute the skill's reference implementation directly. No new
Telegram/KAI end-to-end invocation was performed or claimed in this iteration.

## Validation

**Status: IMPLEMENTED AND QA PASSED. READY FOR COMMIT.**

- [x] Approved single responsibility and unfiltered task endpoint implemented.
- [x] Real token/HTTP 200, fake token/401, and no-request missing-token QA passed.
- [x] All real pages aggregated; three sums equal total and API count.
- [x] Empty result and zero percentages passed controlled QA.
- [x] Verbatim status/type labels, separate DONE/APPROVED, and absent cohorts tested.
- [x] Safe network/shape errors, incomplete pagination, and cap behavior tested.
- [x] Configuration and Skills 1-3 unchanged; no unrelated files modified.
- [ ] Commit created - intentionally not authorized; no staging or commit performed.

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
