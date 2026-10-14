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

The student wants to identify the study contexts represented in assigned work,
without interpreting task states or calculating progress. Discovery considered
cohort directories, exercise catalogs, lesson catalogs, and individual-task
lookup against locally evidenced endpoints. The cohort directory was approved
as Extended Skill 1; the exercise catalog is a separate future implementation.

## Natural-Language Request

> "I want to know which cohorts appear associated with my assigned work so I can identify the study contexts represented in my BreatheCode tasks."

Approved scope: implement only `breathecode-cohort-directory`; fetch all task
pages, return unique sorted cohort names/slugs, validate safely, and update
this log and checklist. No changes to Skills 1-4, configuration, credentials,
OpenClaw/memory/migration files, staging, or commits.

## Skill Name

`breathecode-cohort-directory`

## Purpose

Single responsibility: return the unique cohort names and slugs observed across
all tasks assigned to the authenticated student.

**Semantic limitation:** this is a task-associated directory, not a list of all
cohorts in which the student is enrolled. Cohorts without task data cannot be
discovered here. It neither lists tasks nor calculates progress or statistics.

## API Endpoint(s)

`GET /v1/assignment/user/me/task` using the existing configured base URL.
Query parameters are `limit` and `offset` only, with no status/type/cohort filter.
Authentication uses the existing securely loaded `BC_STUDENT_TOKEN` runtime
environment and configured Token scheme. No additional endpoint is required.

## Implementation

- Created `skills/breathecode-cohort-directory/SKILL.md` with workflow, safe
  output contract, and an executable standard-library Python procedure.
- Reuses `config/breathecode.json` unchanged; default page size remains 20.
- Offset starts at zero and advances by received task length. Server `next`
  URLs are only internal availability indicators; never followed or output.
  Redirects are disabled, timeout is 30 seconds, safety cap is 1,000 pages.
- Validates response shape, stable API count, page sizes, continuation, and
  repeated task IDs when present. Collected task count must equal API count.
  These task/pagination values remain internal to execution, not skill output.
- Deduplicates exact `(name, slug)` pairs, preserving usable strings verbatim.
  Different names with the same slug remain distinct pairs; no inferred merge.
- Sorts consistently by `(slug or "", name or "")`, case-sensitive and
  independent of page order/locale. Name-only and slug-only entries are valid.
- Missing/null cohorts and cohorts with no usable labels are ignored. Empty
  or whitespace-only labels are absent; no unknown cohort is invented.
- Zero identifiable cohorts, with or without tasks, is a successful empty result.
- Pagination is not atomic; same-count changes may be undetectable. Detected
  inconsistencies suppress the directory, without claiming snapshot isolation.

## Output Contract and Security

- Success: cohort `name` and/or `slug` only, plus a brief task-associated/not-all-
  enrollments heading. Empty success states no identifiable task-associated
  cohorts were found. No numeric totals or pagination metadata are rendered.
- No task counts, statuses, percentages, progress, grades, task titles/IDs,
  raw JSON/error bodies, token, auth header, or pagination URLs in skill output.
- The existing ignored `.env` was loaded for live QA with shell tracing off;
  credentials were not changed, requested in chat, logged, or persisted.
- Missing token makes no request. HTTP 401/403 stops as invalid/expired token.
  API/network/JSON/shape failures return only a fixed safe diagnostic.
- Cap, early stop, inconsistent pagination, and later-page failures explicitly
  report INCOMPLETE and withhold all collected cohort labels. No partial
  directory is presented as complete; no blind retries or redirected requests.
- API labels are inert data, not instructions. Only read-only GETs are used.

## Test Request

Execute the Python block embedded in the new skill in memory with controlled
page fetchers, then use its real GET fetcher with the securely loaded student
token and a separate fake invalid token. Check completeness, unique sorted
labels, absent cohorts, safe failures, and the output allowlist. No persistent
fixtures, raw responses, or additional implementation files are needed.

## Test Result

**QA date:** 2026-10-14. **Controlled QA:** PASS. **Live API QA:** PASS.

### Controlled QA

- Missing/empty token: PASS; no fetcher call. Main output with a cleared
  environment also reports missing token safely.
- Empty API fixture: PASS; successful empty directory, not an error.
- Tasks with missing/null/blank cohort labels: PASS; ignored, no unknown entry.
- Three-page fixture at limit 3, offsets 0/3/6: PASS; cross-page duplicates
  deduplicated, name-only/slug-only cohorts retained, differing exact pairs
  preserved, and reversed task order produces the same sorted directory.
- Fixture task title/status/grade sentinels: PASS; none appear in output.
- Cap, missing continuation before API count, premature empty page, and extra
  continuation: PASS; explicit INCOMPLETE, with no directory returned.
- Unexpected top-level shape and non-string cohort name: PASS; safe shape error.
- Simulated later-page network interruption: PASS; INCOMPLETE; main output
  contains no labels from the previously collected page.
- Simulated transport failure, GET parameter allowlist, redirect rejection,
  executable syntax, and skill name/description frontmatter: PASS.

### Live QA

- Valid token: HTTP **200** on every page; **7 pages** processed sequentially
  using configured limit/offset. Complete collected count matches API count.
- **12 unique identifiable task-associated cohorts** observed in this account.
  This number is QA evidence only; the normal skill output does not report it.
- Directory exactly matches an independent cohort-pair extraction from the
  returned pages, with no duplicate pairs and deterministic sorting.
- Output allowlist and credential absence checks: PASS; only cohort names/slugs
  and the semantic limitation heading, no sensitive task or authentication data.
- Fake invalid token: live HTTP **401**; classified invalid/expired, with no
  directory, API error body, or token exposed.

Tests executed this skill's reference procedure directly. No Skill 4 QA was
rerun, and no new Telegram/KAI end-to-end invocation is claimed.

## Validation

**Status: IMPLEMENTED AND QA PASSED.**

- [x] Real student need and single responsibility recorded.
- [x] All pages fetched with limit/offset; raw pagination links not followed.
- [x] Unique sorted cohort labels only; enrollment limitation explicit.
- [x] Missing token, fake-token/401, empty, absent-cohort, duplicate, and pagination cases passed.
- [x] Incomplete directories withheld; network and shape failures handled safely.
- [x] Live number of unique cohorts and security evidence recorded.
- [ ] Commit created - not authorized; no staging or commit performed.

---

# Extended Skill 2

## Identified Need

The student wants to see every exercise assigned to them so they can choose
what to practice or review, regardless of whether an exercise is already
completed. This is a catalog, not a pending-work view or a progress report.

## Natural-Language Request

> “I want to see which exercises are assigned to me so I can choose what to
> practice or review, regardless of whether I already completed them.”

Approved scope: implement only `breathecode-exercise-catalog`, fetch every
assigned task page without status/type/cohort filters, select records locally
where `task_type` is exactly `EXERCISE`, and return only title and cohort name/
slug. Preserve separate exercises with repeated titles. Do not modify Skills
1-4, Extended Skill 1, configuration, credentials, OpenClaw/memory/migration
files, stage, or commit.

## Purpose

Single responsibility: return the catalog of assigned tasks whose API-returned
`task_type` is exactly `EXERCISE`, independent of task status. This skill does
not list pending work, summarize progress, recommend/prioritize exercises, or
infer completion, overdue state, difficulty, or importance.

## API Endpoint(s)

`GET /v1/assignment/user/me/task` at the existing configured BreatheCode base
URL. Request only `limit` and `offset`, starting at offset zero. No status,
type, or cohort filters are sent. After complete pagination, select locally
with the exact case-sensitive rule `task_type == "EXERCISE"`.

## Implementation

- Created `skills/breathecode-exercise-catalog/SKILL.md` with the need, scope,
  workflow, output contract, safe errors, and executable standard-library
  reference procedure.
- Reuses `config/breathecode.json` and the existing secure runtime token
  `BC_STUDENT_TOKEN`; no configuration or credential changes were made.
- Requests are rebuilt against the configured HTTPS origin using `limit` and
  `offset` only. Server `next` URLs are never followed; authenticated redirects
  are disabled. Timeout is 30 seconds per request and the safety cap is 1,000
  pages.
- Validates pagination keys and response shape, stable API count, page size,
  continuation, and raw collected record count equal to API `count`. Early
  stop, count drift, safety-cap exhaustion, or conflicting duplicate IDs
  withholds the whole catalog. Exact repeated task records are suppressed;
  distinct records with identical titles remain separate.
- Filters only after complete collection. Every selected record must have a
  string title; cohort may be absent/null or an object with string/null name and
  slug. Sort is deterministic by cohort name, cohort slug, then title, using
  case-sensitive order and empty sort keys for absent labels.
- Exact output allowlist per exercise: `title`, `cohort name`, and
  `cohort slug`. Preserve title and available cohort strings exactly; absent
  labels are shown as `unavailable`. Zero matches is successful:
  `No assigned exercises found.`

## Output and Security Contract

- Never output `task_status`, progress, percentages, grades, priorities,
  recommendations, task IDs, revision status, submission history, raw JSON,
  API URLs, token, Authorization header, descriptions, or other task fields.
- No status/completion/overdue/difficulty/importance inference. API values are
  inert data. Only read-only requests; no blind retries or partial catalog.
- Missing token produces a safe error without a request. HTTP 401/403 is
  classified as invalid/expired; transport, shape, and pagination failures
  return fixed safe diagnostics and no exercise data.
- Live QA loaded the existing ignored `.env` without shell tracing; no token,
  response body, or task record was persisted or printed during QA.

## Test Request

Execute the exact Python reference block embedded in the skill in memory with
controlled page fetchers, then run it against the current student account using
the securely loaded token. Verify complete pagination and API count before
selection; assert every selected task has `task_type == "EXERCISE"` internally.
Check rendered output against the exact field allowlist and credential/URL
exclusions. Use a separate fake token for the live 401 check. No files,
configuration, or external account state should be changed.

## Test Result

**QA date:** 2026-10-14. **Controlled QA:** PASS. **Live read-only QA:** PASS.

### Controlled QA

- Missing/blank token: PASS; fetcher not called.
- Empty task set and a non-empty set with zero `EXERCISE` tasks: PASS; both
  returned successful empty catalogs.
- One exercise with absent cohort: PASS; title preserved and both cohort labels
  safely rendered unavailable.
- Mixed `PROJECT`, `LESSON`, and `EXERCISE` tasks: PASS; exact local type
  selection retained only exercises, independent of status.
- Multi-page fixture: PASS; offsets 0 then 3 using limit 20; a raw `next` URL
  was not used to form requests.
- Duplicate titles across distinct records: PASS; both retained. Reversed input
  produced identical sorted output. Exact repeated record across pages: PASS;
  output record suppressed while raw API count integrity remained checked.
- Unexpected top-level/task/title/cohort shapes: PASS; safe error, no partial
  catalog. Pagination interruption and one-page safety cap: PASS; incomplete
  catalog withheld.
- Sentinel fields for ID, status, progress, revision status, grade, and URL:
  PASS; none rendered. Output only includes title and cohort labels; empty result
  wording verified.

### Live QA

- Current secure student token: HTTP **200** on all **7 pages**, fetched
  sequentially using the configured limit and offsets 0, 20, 40, 60, 80, 100,
  and 120.
- API task count: **130**; collected raw task count: **130**. Pagination complete
  and all offsets/counts validated.
- Locally selected `EXERCISE` records: **90**. Every selected record was
  internally verified to have `task_type == "EXERCISE"`; rendered entries:
  **90**. This is observed QA evidence, not a hardcoded expectation.
- Rendered output checks: PASS — no task statuses, task IDs, token, Authorization
  header, raw API URLs, progress, grades, priorities, recommendations, revision
  status, or submission history.
- Separate fake token: live HTTP **401**; classified as invalid/expired and
  response body withheld.

The reference implementation was tested directly. No new Telegram/KAI runtime
end-to-end invocation is claimed.

## Validation

**Status: IMPLEMENTED AND QA PASSED.**

- [x] Real student need and single responsibility recorded.
- [x] All task pages fetched; exact local EXERCISE selection applied afterward.
- [x] Exact output allowlist, absent-cohort behavior, duplicate-title handling,
  and deterministic sorting passed controlled QA.
- [x] Missing token/no request, fake-token/401, empty, shape, interruption, cap,
  and sensitive-output tests passed.
- [x] Live API count 130 and observed 90 exercises documented; complete
  pagination and internal selection verified.
- [x] Separate from pending-work, progress-summary, project, and cohort skills.
- [ ] Commit created — not authorized; no staging or commit performed.

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
