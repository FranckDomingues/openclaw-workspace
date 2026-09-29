# Custom Skills Design

## 1. Design Principles

- **Both skills are new implementations.** `project-session-journal` and `project-action-planner`, including their future `SKILL.md` definitions, behavior/instructions, validation scenarios, and project evidence, will be designed and implemented from scratch for this project.
- **Existing infrastructure may be reused; prior skill implementations may not.** The existing OpenClaw environment, Telegram connection, Zapier MCP/MCPorter connection, and prior Google Docs and Google Calendar connectivity evidence are context/infrastructure only. No code, prompts, instructions, packaging, or other implementation from previous milestones, existing custom skills, `openclaw-connection/`, `openclaw-connection-FranckDomingues/`, `skill-workshop/`, `plugin-skills/`, or any previous project artifact may be copied, ported, adapted, renamed, or reused. Prior connection evidence is not proof that either new skill works or that a service is currently available.
- **KAI's personalization is the behavioral contract.** Apply KAI's direct, practical style, distinguish verified facts from assumptions, investigate available context before asking Franck to repeat it, preserve working systems, work in validated phases, and respect privacy and the external-action boundaries in the five personalization files.
- **No fabricated facts.** Missing or uncertain project/session facts, decisions, validation results, dates, deadlines, Git state, event details, or outcomes must be labeled unknown/not supplied or clarified; never fill them in from plausibility. Do not request information already reliably available in the personalization files.
- **Separate preparation, authorization, execution, and verification.** Drafting an artifact or event proposal is not creating it. External effects require appropriate authorization; an explicit invocation that clearly requests creation can authorize that specifically bounded creation. Ambiguous intent or material details require clarification first.
- **Make results verifiable.** Report what was actually observed, distinguish attempted from successful actions, and independently verify a created Google Doc or Calendar event in its destination. A tool response or success claim alone is insufficient.
- **Keep each skill bounded and independently usable.** Neither skill requires the other. Use only currently available, safely verified, already-connected capability; do not add services, integrations, configuration, or credentials.

### Existing versus new

**EXISTING infrastructure/evidence:** OpenClaw environment; Telegram connection; Zapier MCP / MCPorter connection; prior Google Docs connectivity evidence; prior Google Calendar connectivity evidence. Current service health must be checked safely at use time. Prior evidence is not evidence that either new skill works.

**NEW FOR THIS PROJECT:** Project Session Journal skill; Project Action Planner skill; both `SKILL.md` definitions; their behavior/instructions; their validation scenarios; and their project evidence. Both are new implementations, not adaptations of earlier work.

## 2. Skill 1 — Project Session Journal

### Purpose

Transforms a real project or work session into a structured, reusable knowledge record that captures verified work, decision rationale, validation, and next steps without inventing missing facts.

### Trigger / Invocation Intent

Invoke when Franck asks KAI to journal, document, summarize for continuity, or create a session record from a real technical/professional project session. A request to prepare a journal means produce a reviewable conversational draft. A clear request to create/save the Google Doc may authorize the external creation, subject to sufficient details and the confirmation rules below. Do not invoke merely because a conversation mentions a project.

The skill is project-general: it supports AI Engineering, software development, automation, OpenClaw, GitHub, Garzor workflow/process work, and other technical/professional projects without hard-coding a single project.

### Required Input

For a factual journal, the minimum run-specific input is:

- A project or session identity (or enough available context to identify it without ambiguity).
- The session objective or a clear indication that the objective is unknown.
- Some reliable session evidence to record (Franck's account, visible conversation/workspace evidence, or supplied notes). Do not manufacture a completed-work narrative from an objective alone.

The journal may be drafted with gaps: every material unavailable field is explicitly marked `Not provided`, `Unknown`, or `Not applicable`, with a clarification question where it blocks accuracy or the requested external artifact. Before creating a Google Doc, confirm/resolve material gaps affecting its accuracy, title, or destination. The exact creation request may serve as authorization for the narrowly described Google Doc, but does not supply missing facts.

Expected input format: natural-language request and notes, current conversation context, or a compact structured brief. A suggested template is:

```text
Project/session:
Date (if known):
Objective:
Starting state:
Work completed / evidence:
Decisions and rationale:
Files/components changed:
Validation/tests and results:
Git state/commits (if applicable):
Problems/blockers:
Open questions:
Next step(s):
Requested output: conversational draft | Google Doc
```

### Optional Input

- Session date (otherwise leave unknown; do not assume today's date describes the session).
- Starting state, milestones, completed work, files/components, decisions and rationale.
- Test commands, validation outcomes, evidence, and limitations.
- Git branch/status/commit hashes/messages, if applicable and actually supplied or inspected with authorization.
- Problems, blockers, unresolved questions, risks, and proposed next actions.
- Preferred document title, audience, destination/folder if the connected action supports it, and a request to create the Google Doc.
- Relevant project phase or method checklist position.

### Context KAI Already Knows

Use these files to guide behavior, not as substitutes for session-specific evidence:

- **IDENTITY.md:** The assistant is KAI, Franck's technical companion; help turn ideas into structured, executable work, be clear about uncertainty, and preserve what works.
- **SOUL.md:** Be direct, useful, warm, practical, and appropriately detailed; default to Spanish in conversation and use English for technical/professional deliverables when appropriate or requested; investigate context before asking; distinguish facts from assumptions; ask before consequential external effects.
- **AGENTS.md:** Follow the project sequence (understand, inspect, specify/checklist, architecture as relevant, implement incrementally, validate, document, bounded Git, final QA); inspect before changing systems; protect secrets/private context; follow skill instructions and failure boundaries.
- **USER.md:** The user is Francisco Garcia (Franck); timezone `America/New_York`; Spanish is primary conversational language; English is appropriate for technical/professional deliverables when useful/requested; Franck values phased, systematic, verifiable, reproducible work, documentation, Git traceability, learning, and preservation of functioning systems. Relevant domains include AI Engineering, software development, automation/OpenClaw, GitHub, and Garzor workflow/process automation.
- **TOOLS.md:** Prior evidence describes the existing OpenClaw/VPS environment, Telegram, Zapier MCP/MCPorter, Google Docs, and Google Calendar, with current-health qualifications. For this skill, Google Docs use must be safely checked when needed; prior evidence is not a current health check. Confirm external intent and verify the resulting document. Never expose credentials or secret configuration.

Do not disclose the contents of these files unnecessarily in the journal. Their known information can inform format, tone, sequence, and tool boundaries; it cannot establish what happened in this particular session.

### Clarification Rules

- Ask concise, targeted questions when an unknown materially changes the journal's factual accuracy or a requested external action. Batch related questions where practical.
- A journal draft may preserve non-blocking unknowns with explicit labels. Do not ask for optional detail solely to make the record look complete.
- Never infer session dates from the current date, tests from code changes, completed work from plans, a successful outcome from an attempted action, or Git evidence from a narrative without evidence.
- If date, decisions, validation, repository state, file changes, blockers, or outcome is absent, mark it unknown/not supplied unless it is verifiably available in current authorized context.
- If session/project identity or reliable session evidence is insufficient, explain the gap and ask before claiming a factual session summary.
- Before document creation, resolve material ambiguity in content/title/destination and ensure the request authorizes creation. Do not treat a request to “draft” as permission to create a document.

### Processing Behavior

1. Establish the requested output (draft only or Google Doc) and identify the project/session from explicit input and relevant authorized context.
2. Separate supplied/observed facts from unknowns; do not conflate intended work with completed work. Preserve evidence and validation results as stated, including failures and limitations.
3. Organize the account in Franck's relevant sequence: specification/goal → checklist or starting state → architecture/design where applicable → incremental work → validation → Git → QA/delivery/next phase. Do not force irrelevant stages into a session.
4. Capture decisions with their stated rationale; if rationale is absent, record `Rationale not provided` rather than supplying one.
5. Produce the schema below in a clear, reusable form, normally in English for a technical artifact unless Franck requests Spanish or context makes Spanish more useful. Conversational framing may be Spanish.
6. If external creation is requested, present/review the exact journal content first when creation was not explicitly requested as part of invocation. Obtain confirmation before creation in that case. If explicit creation was requested, the invocation is authorization only for the bounded artifact; pause if material content or destination is unresolved.
7. Check that the existing Docs capability is available without revealing secrets, create only the authorized document, then verify the document exists and its observed title/content correspond to the approved journal. Report verification evidence and any limits; do not claim success if verification fails.

### Output Schema

Return a journal with these exact top-level fields, in order. Use `Not provided`, `Unknown`, or `Not applicable` consistently rather than silently omitting a field:

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

Field rules:

- **Project:** Name and session identifier if known; do not invent either.
- **Session date:** Explicitly provided or reliably evidenced date; otherwise unknown. A date is not necessarily a timestamp or timezone.
- **Objective / Starting state:** State the requested aim and established baseline separately.
- **Work completed:** Only completed work, with evidence/source when helpful; distinguish attempted and incomplete work.
- **Decisions and rationale:** Record decisions and Franck's/participant's stated rationale; mark missing rationale.
- **Files/components changed:** List only supported file/component facts; say not supplied if unknown.
- **Validation/tests:** Include actual check and observed result; never imply a test ran if it did not.
- **Git evidence:** Include only verified/supplied branch, status, commit identifiers/messages, and relevant clean/dirty details; otherwise mark unknown/not applicable.
- **Problems or blockers / Open questions:** Preserve unresolved items distinctly from resolved issues.
- **Next actions:** Concrete continuation items, not invented commitments or deadlines.
- **Session outcome:** Concise evidence-based status such as complete, partial, blocked, or undetermined, with basis.

**Local/conversational output:** The complete schema in the conversation for review/continuity. The conversational output remains useful even if Docs is unavailable.

**External destination (optional):** A Google Doc created only through the existing, currently available connection when authorized. The exact schema is the document body; title/destination must be known or safely resolved. No Telegram message or other external output is implied.

### Google Docs Behavior

Apply four distinct gates:

1. **Preparation:** Build the complete schema locally; label gaps; propose a clear title. Do not make a remote change while preparing.
2. **Authorization:** A request explicitly asking for Google Doc creation is authorization for that requested document if content/title/destination are sufficiently clear. If the user only asked for a journal or draft, show the prepared content and ask before creating. Obtain clarification/confirmation for material ambiguity or scope expansion.
3. **Execution:** Safely confirm the existing Docs path is currently available; create one document through the existing connected infrastructure only. Do not inspect or disclose secret-bearing configuration, add a connection, or create a duplicate on uncertainty.
4. **Verification:** Independently confirm the document exists in the intended destination and check its observed title and content against the prepared journal. Report the result with non-secret identifying evidence (for example title and available document identifier), or state precisely what could not be verified. A tool's “created” response alone is not success.

### Success Criteria

- The output has all 13 schema fields in order; unknowns are explicit and no factual claims are fabricated.
- Completed work, decisions/rationale, validation, Git evidence, and next actions are represented only to the degree supported by input/context.
- It reflects Franck's phased method where relevant and KAI's concise, direct, uncertainty-aware style.
- For conversational-only use, the structured journal is returned and its material gaps are visible.
- For authorized Docs creation, the existing connection was available, one intended document was created, and independent destination verification confirms existence plus the intended title/body. If not verified, report failure/uncertainty rather than success.

### Failure Behavior

- On inadequate evidence, pause factual claims, state what is missing, and ask targeted questions; if a useful partial draft is safe, label unknown fields.
- On ambiguous external intent or unresolved material details, do not create a document; return the draft/questions.
- If the Docs connection is unavailable or creation fails, preserve the journal in the conversation, report that no verified document was created, and do not retry blindly or switch to a new service.
- If creation may have succeeded but verification is unavailable, report `created status unverified`, avoid creating a duplicate, and offer a safe next verification step.
- Never expose credentials, tokens, secret configuration, unrelated private project context, or unsupported personal information.

### Test Scenarios

Design only; do not execute during this phase. Use real Franck project input supplied or confirmed for later tests, and retain sanitized expected/observed outcomes.

A. **Complete real session:** A real, sufficiently evidenced project session exercises the full schema, methodology order, factual/unknown distinction, and useful next actions.
B. **Missing important information:** Omit material session evidence or a key decision/validation fact; verify targeted clarification or explicit `Not provided` labels, with no fabricated completion.
C. **Session containing Git evidence:** Use actual supplied/inspected branch, status, and commit evidence; ensure only supported facts enter the Git field and no evidence is invented.
D. **Google Docs creation request:** With clear real journal content and explicit creation intent (or after review/confirmation), verify preparation/authorization/execution separation and independent destination existence/title/body verification.
E. **Connected-service failure/unavailability:** Simulate or safely encounter Docs unavailability/failure; verify the local journal is returned, no success is claimed, no blind retry/new integration occurs, and no duplicate is created.

**Designed scenarios: 5.**

## 3. Skill 2 — Project Action Planner

### Purpose

Transforms verified project state, session outcomes, or explicit goals into a small, sequenced, prioritized set of actionable next steps and schedules only appropriately authorized actions with justified, resolved dates and times.

### Trigger / Invocation Intent

Invoke when Franck asks KAI to plan next steps, prioritize a project, identify dependencies/blockers/decisions, or schedule an explicitly date-specific action from a project goal/session result. It is not a generic to-do generator: preserve phase order and distinguish immediate work from later work. A request for a plan alone creates no Calendar event. A clear request to create a specific event may authorize that bounded event after required details and current connection availability are resolved.

The planner can operate directly on a project brief/session result and does not require a journal to run.

### Required Input

Minimum information to make a useful plan:

- A project or goal that can be identified from explicit input or unambiguous available context.
- A desired outcome or current-state description sufficient to identify a next action. If neither is available, ask rather than create generic tasks.

If project phase/state or dependencies are unknown, say so and ask only when the unknown affects sequencing. A partial plan may identify a discovery action, clearly marked as a proposal, rather than assert unknown project facts.

Expected input format: natural-language request, project context/session outcome, or a compact structured brief. Suggested template:

```text
Project:
Desired outcome:
Current phase/state:
Completed work:
Unresolved items:
Blockers/dependencies:
Deadline(s):
Explicit action date/time/timezone (if any):
Scheduling request/authorization:
Constraints:
```

### Optional Input

- Current phase, specification/checklist/architecture/implementation/validation/Git/QA/delivery status.
- Completed tasks and evidence, unresolved items, risks, blocker owners, dependencies, and required decisions.
- Explicit deadline(s), event date/time and timezone, duration, attendee/context, event title, and calendar details supported by the connected action.
- Desired number of actions, priorities, constraints, sequencing or effort information if known.
- Whether Franck wants only a proposal or explicitly requests Calendar event creation.

### Context KAI Already Knows

Use the five files as durable behavior/project context; do not treat them as evidence of a particular project's current state:

- **IDENTITY.md:** KAI is Franck's execution-oriented technical companion; translate ideas into structured work and preserve functioning systems.
- **SOUL.md:** Be direct and practical; investigate before asking; distinguish facts from assumptions; ask when ambiguity changes results; act proactively on reversible internal planning; confirm consequential external actions.
- **AGENTS.md:** Respect the phased sequence from understanding and baseline through specification/checklist, architecture, incremental implementation, validation, documentation, bounded Git, final QA, delivery; inspect before changes; stop at phase boundaries when instructed; protect data and services.
- **USER.md:** Franck's preferred name, timezone `America/New_York`, Spanish-first conversation, appropriate English for technical deliverables, and preference for systematic, practical, verifiable, traceable project work and learning. His relevant domains include AI Engineering, software development, automation/OpenClaw, GitHub, and Garzor workflow/process work.
- **TOOLS.md:** Prior milestone evidence exists for Google Calendar through Zapier MCP/MCPorter, but current availability is not established by that evidence. Safely check current capability at use time; no secrets or new integrations; confirm event intent/details and verify the resulting event.

These files do not establish current phase, deadlines, commitment, event availability, or permission to create an event. The stored timezone can resolve an explicitly supplied local time if the user has not specified a different timezone; do not infer a date or time from it.

### Clarification Rules

- Ask when project/goal/current state is too vague to prioritize a meaningful next step or when unknown dependencies materially affect sequence.
- Distinguish facts from proposed discovery actions. Do not convert a hypothesis into a blocker, commitment, dependency, deadline, or completed task.
- Never invent deadlines, event dates/times, durations, attendees, owners, availability, completion estimates, or authority. If no justified date exists, leave the action unscheduled.
- A calendar candidate requires a legitimate explicit date/time or user-authorized proposed date, plus required event details and clear creation authorization. If an event detail required by the connected action is missing, ask or leave it as a candidate; do not silently guess.
- Relative dates must be resolved against a known reference date/time and timezone, with the resolved value presented for confirmation if ambiguous. Use `America/New_York` as Franck's known timezone only when applicable; do not assume it is the intended event timezone if the request indicates otherwise.
- If Franck explicitly asks KAI to propose a date, proposed dates must be labeled as proposals and remain unscheduled until he authorizes creation.

### Processing Behavior

1. Identify the project, intended outcome, known current state, and requested scope. Separate observed facts from assumptions/proposals.
2. Preserve Franck's project sequencing; do not recommend implementation before required understanding, inspection, specification/checklist, or architecture gates when relevant. Do not force every phase into every plan.
3. Identify dependencies and blockers before ordering actions. Distinguish actions that can start now from actions gated by another task, decision, or external dependency.
4. Limit the main plan to a small, prioritized set (normally 3–7 actions; fewer if that is sufficient). Separate immediate next action, later actions, blockers/dependencies, decisions required, date-specific candidates, and unscheduled items.
5. Give each action a clear verb, deliverable, reason/sequence, dependency, and completion/verification criterion. Keep estimated dates absent unless justified by input or explicitly requested as a proposal.
6. Return the schema below locally first. A planning request does not authorize remote calendar changes.
7. For a Calendar request, resolve date/time, timezone, title, duration and other fields required by the available existing action; distinguish explicit user-provided details from proposals. Obtain confirmation when details/intent are ambiguous or the invocation did not explicitly request event creation. Safely check current Calendar availability before execution.
8. Create only authorized, justified, fully specified event(s) through the existing connection. Verify each resulting event independently against its intended title, date/time/timezone and other requested details. Report observed evidence and any unverified fields; do not treat a tool response alone as success.

### Output Schema

Return exactly these top-level fields in order. If no item applies, write `None identified` or `Not provided`; do not invent content:

```text
Project:
Current phase/state:
Desired outcome:
Immediate next action:
Why it is next:
Dependencies:
Additional actions:
Decisions required:
Schedule candidates:
Unscheduled actions:
Completion/verification criteria:
```

Field rules:

- **Project / Current phase/state:** Only explicit or verifiably available facts; unknown state remains unknown.
- **Desired outcome:** Preserve Franck's requested outcome; clarify if it is not identifiable.
- **Immediate next action:** One bounded, actionable step, or ask for information if no justified action can be selected.
- **Why it is next:** Explain sequencing, dependency, risk, or phase rationale; mark rationale as a recommendation when not directly supplied.
- **Dependencies:** State prerequisite action, blocker, owner (only if known), and effect; distinguish known from suspected.
- **Additional actions:** Prioritized later actions with a short reason and prerequisites. Keep this small and not generic.
- **Decisions required:** Name the choice, who must decide if known, and which actions it gates.
- **Schedule candidates:** Only actions with explicit/legitimately authorized proposed date/time and enough information for evaluation. Mark proposal versus user-provided date, timezone, and unresolved details. A candidate is not an event.
- **Unscheduled actions:** Actions with no justified date/time, including those awaiting a deadline or decision. Explicitly say they remain unscheduled.
- **Completion/verification criteria:** Observable result/evidence for each action or the plan outcome; never claim completion in advance.

**Local/conversational output:** The complete structured plan in the conversation, useful without any calendar operation. Normal response language follows KAI's personalization.

**External destination (optional):** Google Calendar event(s) only if a legitimate date/time exists, all required details are resolved, current existing connectivity is verified, and creation is authorized. Nothing is created for unscheduled actions; no Telegram or other external action is implied.

### Google Calendar Behavior

1. **Preparation:** Produce and review the complete local plan first. Separate date-specific schedule candidates from unscheduled actions. State which details are known, proposed, or missing.
2. **Authorization:** A request for a plan or a list of next actions is not Calendar authorization. An explicit request to create the named/date-specific event(s) can authorize that bounded action when details are clear. Otherwise present the proposed event details and ask before creation. Proposed dates requested by Franck are not authorized dates until confirmed for the event.
3. **Execution:** Confirm current availability of the existing Calendar capability safely; no new connection/configuration. Require the event title, date, start time, timezone, and duration/end time, plus any other field the currently available action requires. Clarify conflicts, relative dates, or missing material details before creating. Create only the specifically authorized event(s), not every plan action.
4. **Verification:** Independently inspect the resulting destination event(s) and check existence plus intended title, date, start/end time and timezone, and other requested details where visible. Report non-secret identifying evidence (such as title, date/time, and available event identifier). If existence or any material detail cannot be checked, say so and do not claim success or blindly retry.

### Success Criteria

- All 11 schema fields are returned in order; the plan is small, sequenced, and tied to known project state and outcome.
- Immediate work, later work, blockers/dependencies, decisions, schedule candidates, and unscheduled work are distinct.
- Phase sequencing is preserved where relevant; every action has a clear completion/verification criterion.
- No deadline or date is invented; without a justified date, an action remains unscheduled unless Franck explicitly asks for a proposed date.
- Planning-only requests cause no external effect.
- For authorized event creation, current existing Calendar availability is checked, all necessary details are resolved, only approved events are created, and independent destination verification confirms existence and intended details. Otherwise report the limitation/failure accurately.

### Failure Behavior

- If the desired outcome or state is too vague for a useful plan, ask a targeted question; do not produce generic tasks masquerading as project-specific planning.
- If dependency/status evidence is incomplete, label it unknown/suspected and plan a verification/discovery action only as a proposal.
- If date/time/details are absent or conflicting, keep the action unscheduled and request clarification; never invent a deadline or silently pick a slot.
- If authorization is missing, return the plan/candidate event details without creating anything.
- If Calendar is unavailable or creation fails, return the local plan, report that no verified event was created, and do not blindly retry or use another service.
- If an event may have been created but verification fails, mark it unverified, avoid duplicates, and offer a safe verification step.
- Protect private context and secrets; do not represent a proposal as a commitment or a tool response as proof.

### Test Scenarios

Design only; do not execute during this phase. Later use realistic project inputs supplied or confirmed by Franck; record expected and observed behavior without fabricated deadlines.

A. **Clear immediate next action:** A real project with sufficient verified state should yield a phase-appropriate, bounded next action, its rationale, and observable completion criteria.
B. **Multiple dependencies:** A real project with prerequisite work/blockers should show correct dependency order, distinguish blockers from decisions, and avoid recommending gated work prematurely.
C. **Action with explicit date/time:** A real, user-provided date/time/timezone should be represented accurately as a schedule candidate; verify ambiguity handling and that the date is not altered or embellished.
D. **Action without a date:** A valid next action with no date should remain explicitly unscheduled, without an invented deadline or calendar candidate.
E. **Google Calendar creation request:** With a real request, explicit authorization, complete resolved event details, and current service availability, verify the created event independently against title/time/details.
F. **Connected-service failure/unavailability:** On unavailable/failing Calendar capability or failed verification, ensure the local plan remains available, no success/duplicate/new integration is claimed or produced, and uncertainty is reported.

**Designed scenarios: 6.**

## 4. Cross-Skill Interaction

The skills may be composed as a workflow, but neither depends on the other:

```text
Real project session evidence
        ↓ (optional)
Project Session Journal — record verified work, decisions, validation, Git, and next actions
        ↓ (optional transfer of the journal's explicit facts; not automatic authority)
Project Action Planner — prioritize sequenced next steps and identify dependencies/decisions
        ↓ (only with justified date/time and separate sufficient authorization)
Google Calendar event(s), independently verified
```

- The Journal is independently usable from session notes/conversation/work evidence and may optionally produce a Google Doc.
- The Planner is independently usable from a project brief, current-state evidence, goals, or user-provided session result; it does not require a journal or Google Doc.
- If the Planner consumes a journal, it may use only facts actually present in it. It must retain unknowns and distinguish proposed next actions from commitments.
- A journal's `Next actions` are input candidates, not automatic Planner authorization to schedule. Each scheduling action still requires legitimate date/time, resolved event details, appropriate authorization, and Calendar verification.
- No automatic document-to-calendar or cross-skill remote side effect is assumed. Franck can request either skill separately; one skill's success does not imply the other's.

## 5. Connected-Service Boundaries

| Stage | Google Docs — Session Journal | Google Calendar — Action Planner |
|---|---|---|
| **Preparation** | Build and review the complete factual journal locally; mark gaps; identify title/destination. | Build and review the local prioritized plan; separate dated candidates from unscheduled actions; resolve event fields. |
| **Authorization** | Explicit creation request may authorize the specifically requested document when scope/content are clear. A draft/journal request alone does not; show content and ask first. | A planning request alone never authorizes an event. Require explicit event-creation intent or confirmation; proposed dates need confirmation. |
| **Execution** | Safely check existing Docs capability, then create only the authorized single document via existing infrastructure. | Safely check existing Calendar capability; create only authorized events with legitimate date/time and all required details via existing infrastructure. |
| **Verification** | Independently verify document exists and observed title/body match the prepared journal. | Independently verify event exists and observed title/date/time/timezone and requested details match. |
| **Unavailable/unverified** | Return local journal; report no verified artifact; avoid blind retries/duplicates and new connections. | Return local plan; leave unscheduled or report event unverified; avoid blind retries/duplicates and new connections. |

Existing Telegram and Zapier MCP/MCPorter infrastructure may be relevant to how existing services are reached, but the design does not assume present availability, inspect secret-bearing configuration, authorize sending messages, or add/change integrations. Service availability and native tool path must be confirmed safely during later implementation/testing. Prior Google Docs/Calendar evidence is not evidence of either new skill's function.

## 6. New Implementation Boundary

**Build new:** two native OpenClaw custom skills named `project-session-journal` and `project-action-planner`; each skill's own `SKILL.md` instructions/metadata and behavior; the validation scenarios and evidence for this project; and any later project-specific implementation/test material separately authorized in implementation phases.

**May use as existing infrastructure (after safe current check):** the OpenClaw environment, Telegram connection, Zapier MCP/MCPorter connection, and the prior Google Docs/Google Calendar connectivity evidence. The connected services themselves are not recreated or reconfigured by these designs.

**Must not reuse:** implementation code, instruction text, prompts, skill packaging, or other implementation from previous milestones, existing custom skills, `openclaw-connection/`, `openclaw-connection-FranckDomingues/`, `skill-workshop/`, `plugin-skills/`, or any previous project artifact. Prior connectivity evidence is not implementation evidence and does not validate the new skills. No skill directories or `SKILL.md` files are created in this design phase.

## 7. Validation Matrix

All scenarios below are planned for later phases, not executed now. Use real project/session information Franck supplies or confirms. Do not use invented profile details or dates. Each test record should include sanitized input, expected result, observed result, date/context, and pass/fail.

| Skill | Invocation test | Missing-input test | Personalization test | Output-schema test | External-service test | Verification test | Failure test |
|---|---|---|---|---|---|---|---|
| `project-session-journal` | Invoke with a real project session and an explicit journal/draft intent; distinguish conversational output from requested Docs creation. | Omit material evidence/objective or rationale; ask targeted questions or label gaps, with no invented facts. | Verify KAI's direct/uncertainty-aware style, Franck's phased methodology and relevant known context are reflected without repeating known facts or leaking profile data. | Check all 13 exact ordered fields and unknown/not-applicable labeling against the schema. | Explicitly requested/confirmed real Google Docs creation through safely checked existing infrastructure; no action on draft-only invocation. | Independently confirm destination document existence and title/body correspondence; retain non-secret evidence. | Unavailable service, creation error, or unverified result: return local draft, report uncertainty, avoid blind retry/duplicate/new integration. |
| `project-action-planner` | Invoke from a real goal/current-state input and request a plan; verify it is specific, small, and not generic. | Omit goal/state/dependency/event details; clarify material gaps, label uncertainty, and keep date-less actions unscheduled. | Verify Franck's phased sequencing, KAI's practical style, use of known timezone only when applicable, and no invented project state/deadline. | Check all 11 exact ordered fields, action grouping, dependencies, and completion criteria. | Explicitly authorized real Calendar event with a legitimate date/time and required details using a safely checked existing connection; plan-only input creates none. | Independently inspect resulting event existence and intended title/time/details; retain non-secret evidence. | Missing authorization/details, unavailable service, failure, or unverifiable result: no unsupported event/success claim; return plan and avoid duplicate/blind retry. |

## 8. Implementation Plan

This is the intended sequence for later authorized phases. **Do not perform these steps in this design-only phase.**

1. Commit `SKILLS_DESIGN.md`.
2. Determine the correct OpenClaw custom-skill location/format from the installed environment/documentation.
3. Implement `project-session-journal` from scratch.
4. Validate it locally.
5. Test it with real Franck project input.
6. Verify Google Docs output.
7. Commit Skill 1.
8. Implement `project-action-planner` from scratch.
9. Validate it locally.
10. Test it with real Franck project input.
11. Verify Google Calendar output.
12. Commit Skill 2.
13. Cross-skill test.
14. Final QA.
