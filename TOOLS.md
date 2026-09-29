# TOOLS.md — Verified Environment Notes

Use the simplest suitable tool already connected. These notes describe evidence and known limitations; they do not grant permission for external actions. Inspect current state before changing configuration. Never record credentials, tokens, OAuth data, or other secrets here.

## OpenClaw

- **Status: VERIFIED (version at project audit).** OpenClaw 2026.7.1-2 runs in the existing VPS environment.
- Workspace: `/root/.openclaw/workspace`.
- Inspect existing configuration before any proposed change; preserve working settings by default.

## Telegram

- **Status: VERIFIED as an existing interaction channel by prior project evidence; CURRENTLY DEGRADED/WARNING for message-tool health.** The reported `openclaw doctor` baseline includes a Telegram/message-tool warning for agent `main`.
- Do not claim advanced messaging actions are fully validated until the warning is investigated and the relevant behavior is tested. Confirm intent before sending messages or causing other external effects.

## Zapier MCP / MCPorter

- **Status: VERIFIED in prior milestone evidence.** Zapier MCP was previously connected through MCPorter using OAuth.
- Reuse the existing connection where available. Do not create new OAuth/API integrations for this project. Do not infer that the connection is currently healthy without checking it safely.

## Google Docs

- **Status: VERIFIED in prior workflow evidence.** A prior Telegram request successfully created a Google Docs document through the existing Zapier MCP workflow.
- This is prior evidence, not proof of present availability. Verify the current tool/connection and destination result when needed; confirm before creating or modifying external content.

## Google Calendar

- **Status: VERIFIED in prior workflow evidence.** A prior workflow successfully created a calendar event through the existing Zapier MCP workflow.
- This is prior evidence, not proof of present availability. Verify the current tool/connection and resulting event when needed; confirm event details and intent before creating or modifying events.

## Git and GitHub

- **Status: VERIFIED for repository operations.** The workspace is a Git repository on `main` with an `origin` remote at `git@github.com:FranckDomingues/openclaw-workspace.git`.
- Follow the inspection, bounded-staging, commit, and push rules in `AGENTS.md`. GitHub CLI availability and any additional GitHub capabilities are **NOT YET VERIFIED**; do not claim or rely on them without checking.

## Tool Selection and Verification

- Prefer the least complex already-connected tool capable of completing the task.
- Distinguish prior evidence from a current health check; verify the specific capability required at time of use.
- Do not introduce a new service merely because it is available. Respect the task scope, privacy boundaries, and confirmation requirements in `SOUL.md` and `AGENTS.md`.
