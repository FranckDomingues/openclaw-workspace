# Delivery 03 — Specification

## Project

Mi Asistente 4Geeks — Teaching OpenClaw to Follow Student Progress

## Context

This is Delivery 03 of the larger OpenClaw/KAI project.

The existing OpenClaw agent will be extended with focused skills capable of retrieving student information from the 4Geeks/BreatheCode API.

## Primary Objective

Allow KAI to answer course-progress questions directly from conversation without requiring the student to manually leave the agent and inspect the 4Geeks platform.

Examples:

- Am I authenticated?
- What projects are assigned to me?
- What do I still need to complete?
- How much progress have I made?
- What should I work on next?

## Functional Requirements

### Skill 1 — Authentication

The agent must be able to verify that the configured student token is valid and that access to the student API is available.

### Skill 2 — Assigned Projects

The agent must retrieve the student's assigned projects and expose their current status.

Relevant statuses may include:

- pending
- submitted
- graded

The actual API response remains the source of truth.

### Skill 3 — Pending Work

The agent must identify work that has not yet been completed.

### Skill 4 — Progress Summary

The agent must provide a useful summary of the student's progress through the course.

### Extended Skills

At least two additional skills must be identified through actual student needs and built using the same conversational process.

They must not be predetermined solely outside OpenClaw.

## Architecture Rules

### Single Responsibility

Each skill must map to one specific API action or a very small set of tightly related API operations.

Do not create one broad "4Geeks skill" responsible for everything.

### Conversational Development

Skills must be designed through interaction with OpenClaw.

The development conversation is part of the evidence.

### Source of Truth

Course data must come from the 4Geeks/BreatheCode API.

The agent must not invent:

- projects
- submission states
- grades
- progress
- deadlines

## Security Requirements

The student token must:

- remain outside committed files
- remain outside `SKILL_LOG.md`
- not appear in test evidence
- not appear in public logs
- be loaded using secure runtime configuration

## Documentation Requirements

For every skill, record:

1. Natural-language request given to OpenClaw
2. What OpenClaw proposed
3. Skill responsibility
4. API endpoint or endpoints used
5. Implementation summary
6. Test request
7. Test result
8. Validation outcome

## Git Strategy

`main` remains the stable branch representing the previous submitted delivery.

Development for this delivery occurs on:

`delivery-03-4geeks-assistant`

Changes should be committed incrementally.

## Definition of Done

Delivery 03 is complete only when:

- four required skills work
- at least two extended skills work
- real API responses have been tested
- token security has been verified
- every skill is documented in `SKILL_LOG.md`
- README documentation identifies this work as Delivery 03
- final QA passes
- Git working tree is clean
- work is pushed to GitHub
