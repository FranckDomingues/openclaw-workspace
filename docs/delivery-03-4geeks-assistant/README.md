# Delivery 03 — Mi Asistente 4Geeks

This directory documents the third academic delivery built inside the existing OpenClaw/KAI project.

## Parent Project

**Mi Agente, A Mi Manera**

This delivery extends the same OpenClaw instance and workspace used in the previous deliveries. It is not an isolated project and does not use a separate repository.

## Goal

Teach OpenClaw/KAI how to connect securely to the 4Geeks student API through BreatheCode and answer questions about:

- authentication status
- assigned projects
- pending work
- course progress
- additional student needs discovered during development

## Required Skills

1. Authenticate student session
2. Get assigned projects
3. Get pending work
4. Get progress summary
5. Extended skill #1 — to be discovered
6. Extended skill #2 — to be discovered

## Development Method

Skills must be created through conversation with OpenClaw.

The workflow is:

1. Express the need in natural language.
2. Let OpenClaw participate in designing the capability.
3. Keep each skill focused on one API responsibility.
4. Implement the skill.
5. Test against real 4Geeks/BreatheCode data.
6. Document the interaction and result in `SKILL_LOG.md`.
7. Commit the iteration.

## Security

The 4Geeks student token is a credential.

It must:

- be stored using secure OpenClaw configuration
- never be hardcoded into a skill
- never appear in Git history
- never be included in documentation or screenshots intended for submission

## Delivery Documentation

- `README.md` — delivery overview
- `SPECS.md` — requirements and architecture constraints
- `CHECKLIST.md` — implementation checkpoint
- `SKILL_LOG.md` — required evidence for every skill
