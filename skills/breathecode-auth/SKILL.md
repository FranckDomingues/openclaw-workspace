---
name: breathecode-auth
description: "Verify BreatheCode Student API authentication using a student token from the environment, returning identity confirmation or a specific error reason."
---

# BreatheCode Auth — Student Token Verification

Verify authentication against the BreatheCode Student API. This is the foundation skill for all BreatheCode API interactions. It only performs authentication and identity verification; it does not fetch cohort, assignment, progress, or any other student data.

## Prerequisites

- **Environment variable:** `BC_STUDENT_TOKEN` must be set in the runtime environment (loaded via `.env` or exported directly).
- **Configuration file:** `config/breathecode.json` defines the base URL and auth endpoint.
- **Network:** The VPS must have outbound HTTPS access to `breathecode.herokuapp.com`.

## Authentication Contract

| Property | Value |
|---|---|
| Base URL | `https://breathecode.herokuapp.com` |
| Endpoint | `GET /v1/admissions/user/me` |
| Header | `Authorization: Token <student_token>` |
| Success code | `200` |
| Unauthorized code | `401` |

## Workflow

1. **Read the token.** Read `BC_STUDENT_TOKEN` from the environment. Never print, log, hardcode, or expose the token value in any output, error message, or debug line. If the variable is unset or empty, report `Missing token` and stop.

2. **Load configuration.** Read `config/breathecode.json` to get `base_url`, `auth_endpoint`, and `auth_header_format`.

3. **Call the API.** Execute a GET request against the authentication endpoint with the token in the `Authorization` header. Capture the HTTP status code and response body separately.

4. **Classify the result:**

   | Status | Result |
   |---|---|
   | `200` | Authenticated successfully |
   | `401` | Invalid or expired token |
   | Other 4xx/5xx or connection failure | API/network error |

5. **On success (`200`), return only identity information** appropriate for authentication verification. Extract the following from the JSON response body if present:
   - `id`
   - `email`
   - `first_name` / `last_name` or `full_name`
   - `github` (if present, extract the nested `username` field)
   - `username`

   Return these fields concisely. Do not display the token or the raw response body. Do not fetch or display cohort details, assignments, lessons, progress, or any non-identity data.

6. **On error**, return a clear message classifying the failure as one of:
   - Missing token (environment variable not set)
   - Invalid or expired token (HTTP 401)
   - API or network error (connection failure, timeout, unexpected status)

   Do not expose the token in any error message.

## Output Schema

Return a structured result in this format:

```text
Authentication: [OK | FAILED]
Status: [200 | 401 | error]
Identity: (on success only)
  • [field]: [value]   (one line per available field)
Error: (on failure only)
  • Reason: [missing token | invalid/expired token | API/network error]
  • Detail: [brief diagnostic without exposing the token]
```

## Safety Rules

- **Never** print, log, expose, echo, or hardcode the token value.
- **Never** include the `Authorization` header value or token in any output, error, debug, or diagnostic message.
- **Never** dump the raw API response body to output.
- **Never** fetch any data beyond identity verification (no cohort, assignments, lessons, progress, grades, or other student data).
- If the environment variable is missing, report `Missing token` — do not guess or prompt for the token value.
- If the API returns a non-identity response, report only the status classification and a generic diagnostic.

## Failure Behavior

- **Missing token:** Report `Missing token` — the `BC_STUDENT_TOKEN` environment variable is not set. Do not attempt the API call.
- **HTTP 401:** Report `Invalid or expired token`. Do not retry.
- **Connection error / timeout:** Report `API/network error` with a brief, token-safe diagnostic (e.g., "Connection refused", "DNS resolution failed", "Timeout after X seconds"). Do not retry blindly.
- **Unexpected HTTP status:** Report `API/network error` with the status code and a generic message. Do not expose the response body.