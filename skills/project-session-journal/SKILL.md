---
name: project-session-journal
description: "Journal a real technical or professional project session into a factual, reusable record; use when Franck asks KAI to document work, capture session decisions, or prepare continuity notes."
---

# Project Session Journal

Turn evidence about a real work session into a concise record Franck can review and reuse. Use for AI Engineering, software development, automation, OpenClaw, GitHub, Garzor process work, and other technical or professional projects. Do not invoke merely because a project is mentioned.

## Workflow

1. **Establish scope.** Determine the project/session and whether Franck wants a conversational journal or explicitly asks to create/save a Google Doc. A request to journal, summarize, or draft is conversation-only; do not infer permission for an external document.
2. **Inspect available evidence first.** Use the current conversation and authorized, relevant project context before asking Franck to repeat information. KAI's workspace guidance and profile inform tone and workflow, not what happened in this session. Do not disclose private context unnecessarily. Do not inspect secrets or unrelated private data.
3. **Separate evidence from gaps.** Record only work and outcomes supported by Franck's input or authorized observable evidence. Where provenance matters, label claims `Verified`, `User-provided`, `Not verified`, or `Inferred`; user-provided information is not independent verification, and an inference must never be presented as established fact. If evidence conflicts, surface the conflict. For high-risk claims (Git state, files, commits, timestamps, authentication, configuration/infrastructure, validation, external-resource creation, and system state), use `Verified` only when direct evidence supports that exact claim; otherwise label `Not verified` or omit it. Never infer dates, tests, decisions/rationale, or Git facts from plausibility. An HTTP 401 establishes that an authentication failure was observed, not that a token expired or which authentication boundary rejected it. Do not claim files are staged, committed, pushed, tracked, clean, modified, or unmodified—or that configuration/infrastructure was unchanged—unless direct evidence verifies that specific state.
4. **Ask only material questions.** If project/session identity or reliable evidence is insufficient to make any accurate journal, explain the gap and ask a focused question. Otherwise produce a useful partial journal, mark missing fields `Not provided`, `Unknown`, or `Not applicable`, and ask only about gaps that materially affect accuracy or a requested external creation. Do not fill the record merely for completeness.
5. **Organize the real work.** Where relevant, reflect Franck's sequence: specification/goal → checklist or starting state → architecture/design → incremental implementation → validation/testing → Git discipline → final QA → delivery. Omit irrelevant stages and do not imply a stage occurred when it did not. Include decision rationale only when supplied or evidenced; otherwise say `Rationale not provided`.
6. **Return the schema below**, in order, normally in English for a technical deliverable unless Franck requests Spanish or the context makes Spanish more useful. Keep it practical, concise, and usable as durable project knowledge.
7. **Google Docs is optional.** Prepare the complete journal locally first. Create a Google Doc only when Franck explicitly asks for that external creation; a journal/draft request alone is not authorization. For explicit creation, ensure title/content/destination are sufficiently clear, then safely confirm the existing connected Docs path is currently available and use it only if exposed and permitted. Do not inspect or reveal credentials, configure a new API/OAuth/integration, switch services, or create a duplicate on uncertainty. If material details are missing, ask before proceeding.
8. **Verify and report honestly.** After an authorized creation, independently confirm the document exists and check the observed title and content against the prepared journal using available non-secret evidence. A tool success message alone is insufficient. If availability, creation, or verification fails, return the journal in conversation, report precisely what is unverified, and do not claim success or retry blindly.

## Journal schema

Return all fields, with these exact labels and order. Use `Not provided`, `Unknown`, or `Not applicable` consistently instead of silently omitting a field.

```text
Project:
Session date:
Objective:
Starting state:
Work completed:
Decisions and rationale:
Files/components changed:
Validation/tests:
Git evidence:
Problems or blockers:
Open questions:
Next actions:
Session outcome:
```

- Identify the project/session only when known. Use an explicitly provided or reliably evidenced session date; otherwise mark it unknown.
- Keep objective separate from starting state. List only supported completed work and changed files/components; distinguish incomplete attempts.
- Report actual validation and results, including failures and limits. Include only directly verified or clearly user-provided Git branch/status/commit details; do not silently upgrade a user report to verified evidence. If the exact state was not checked or supplied, say `Not verified` or `Not provided`.
- Keep blockers distinct from open questions. Make next actions concrete without inventing owners, deadlines, or commitments.
- State an evidence-based outcome such as complete, partial, blocked, or undetermined, with a brief basis. A document creation attempt is not a successful outcome unless verified.

## Safety and failure behavior

Follow KAI's direct, practical, uncertainty-aware style and workspace rules. Preserve functioning systems and keep any repository inspection or change within the user's authorization. Drafting the journal does not authorize edits to project files or external systems. Never expose secrets or unrelated private context.

If evidence is weak, preserve uncertainty rather than narrating a plausible story. If Docs is unavailable or cannot be verified, provide the conversational journal and state that no verified document creation is established. If creation may have succeeded but verification is unavailable, mark it `created status unverified`, avoid a duplicate, and state a safe verification step.
