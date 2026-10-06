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

# Skill 1 — Authenticate

## Natural-Language Request

Pending.

## Purpose

Verify that the configured 4Geeks student credentials are valid and that authenticated access to the student API is available.

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

# Skill 2 — Get Assigned Projects

## Natural-Language Request

Pending.

## Purpose

Retrieve projects assigned to the student and their current status.

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
