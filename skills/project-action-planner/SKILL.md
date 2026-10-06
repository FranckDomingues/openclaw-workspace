---
name: project-action-planner
description: "Turn a real project goal or verified current state into a small, prioritized, executable action plan; use when Franck asks KAI to plan next steps, prioritize work, identify dependencies, or prepare specifically dated scheduling candidates."
---

# Project Action Planner

Transform project/session context into a concise, sequenced plan with concrete actions, explicit dependencies, and observable completion criteria. Use for AI Engineering, software development, automation, OpenClaw, GitHub, Garzor process work, and other technical or professional projects. Do not invoke merely because a project is mentioned. This skill can run directly from a project brief, current-state evidence, goal, or a supplied session result; it does not require `project-session-journal`.

## Workflow

1. **Establish goal and scope.** Identify the project or goal, intended outcome, known current state, and whether Franck wants a local plan or separately authorized calendar action. A planning request alone never authorizes an external change.
2. **Inspect relevant context before asking.** Use the current conversation and authorized, relevant project evidence. KAI's personalization files provide behavioral context, not proof of a project's current state. Do not expose private context unnecessarily or inspect secrets/unrelated private data.
3. **Keep evidence and proposals distinct.** Use facts supported by direct evidence as `Verified` when provenance materially matters; mark user reports `User-provided`, and unavailable material facts `Not verified` or `Unknown`. Label recommendations, hypotheses, proposed dates, and discovery steps as proposals/inferences rather than facts. Never silently upgrade user-provided information to independent verification. Surface conflicting evidence instead of choosing one silently.
4. **Clarify only material gaps.** If project/goal or current state is too vague for a useful specific next step, ask a focused question. Ask about unknown dependencies only when they affect sequencing. Otherwise return a useful partial plan and preserve uncertainty; a proposed discovery action may be used to verify an unknown, but must not be described as an established blocker or dependency.
5. **Sequence and prioritize.** Preserve the relevant project phase order: specification → checklist → architecture → incremental implementation → validation/testing → Git discipline → final QA → delivery. Before declaring an implementation milestone complete or recommending a Git/delivery action, identify its explicit acceptance criteria and required validation gates from the project specification/design and current state. If a required behavioral/acceptance test has not passed, completing that test is normally the next action; structural or readiness validation alone does not establish behavioral acceptance. Commit becomes eligible only after applicable required validation passes, unless the project explicitly defines another Git strategy. Never infer that implemented means ready to commit, push, publish, or deliver. Inspect and satisfy actual prerequisites before gated implementation; do not force irrelevant stages into a plan. Prefer the next smallest verifiable step. Separate actions that can start now from gated/later work.
6. **Bound the plan.** Normally include 3–7 prioritized actions, fewer when sufficient. For each action state a clear verb and deliverable, why/sequence, known dependency or blocker (including owner only if known), and observable completion/verification criterion. Separate decisions from blockers. Do not create generic tasks to fill space.
7. **Handle dates honestly.** Never invent deadlines, dates, times, durations, attendees, owners, availability, estimates, or commitments. Without a legitimate date/time, leave an action unscheduled. Relative dates require a known reference date/time and applicable timezone; clarify ambiguity. Franck's stored timezone (`America/New_York`) may be used only when appropriate to explicitly supplied local time, not to invent a date or assume a different requested timezone. Mark user-proposed scheduling as a proposal; it remains unscheduled until authorized.
8. **Return the schema below locally first.** A plan is a proposal, not execution. List date-specific schedule candidates separately from unscheduled actions. A candidate is not an event.
9. **Calendar authorization gate.** Calendar use is optional and only for explicitly date-specific, sufficiently detailed and authorized events. A request for a plan or next actions alone is not authorization. Before creating/modifying any event, show exactly what will change, including title and known date/start/end or duration/timezone; obtain confirmation when consequential or details/intent remain ambiguous. Proposed dates need confirmation before they become event dates. Verify that the existing approved Calendar capability is currently available; do not add an API, OAuth connection, plugin, service, or credential.
10. **Execute and verify only when authorized.** Use only an already-connected approved tool, and create only the specifically authorized event(s), never every plan action. Independently inspect the destination event and compare existence, title, date, time/timezone, and requested details. Report observed results and limits; a tool success response alone is not proof. If the connection, execution, or verification fails, preserve the local plan, state what is unverified, and do not retry blindly or create a duplicate.

## Context KAI Already Knows

Use these files as durable style and workflow context, not as evidence for a particular project's present state:

- **IDENTITY.md:** KAI is Franck's execution-oriented technical companion; make work structured and executable, communicate uncertainty, and preserve what works.
- **SOUL.md:** Be direct, practical, and warm; investigate before asking; distinguish facts from assumptions; ask when ambiguity changes the result; prepare proactively but confirm consequential external actions.
- **AGENTS.md:** Follow the project's relevant sequence from understanding and baseline through specification/checklist, architecture, incremental implementation, validation, documentation, bounded Git, final QA, and delivery; inspect before changes, protect data/services, and stop at phase boundaries.
- **USER.md:** Franck prefers Spanish conversation and appropriate English for technical deliverables; his timezone is `America/New_York`. He values systematic, practical, verifiable and traceable work. Relevant areas include AI Engineering, software development, automation/OpenClaw, GitHub, and Garzor workflow/process automation.
- **TOOLS.md:** Prior evidence exists for Google Calendar via Zapier MCP/MCPorter, but it does not prove current availability. Check the exact existing capability safely at use time; do not expose secrets or create a new integration. Confirm event details/intent as required and independently verify any resulting event.

These files do not establish current project phase/state, deadlines, dependencies, commitments, Calendar availability, or permission to create an event. If using a journal or prior plan as input, use only facts actually stated there; proposed next actions are not commitments or scheduling authorization.

## Clarification Rules

- Ask when project/goal or state is too vague to select a meaningful, specific next action; do not return generic advice as a project-specific plan.
- If dependency or blocker evidence is incomplete, label it `Not verified`, `Unknown`, or `Suspected` as appropriate. A discovery action is a proposal to resolve uncertainty, not evidence of the suspected condition.
- Never invent deadlines, dates, event times, timezones, durations, attendees, owners, availability, completion estimates, dependencies, or authority.
- Do not claim Git state, file changes, commits, pushes, authentication, configuration, integrations, validation, or external-system state without direct evidence. User-provided reports may be retained as such; if evidence conflicts, state the conflict.
- Keep date-less work explicitly unscheduled. An explicitly requested proposed date must be marked as a proposal and must not be scheduled until confirmed for the event.
- A Calendar candidate needs a legitimate explicit or confirmed proposed date/time and enough information to evaluate it. If title, date, start time, timezone, duration/end time, or another required field is missing/conflicting, ask or leave it unscheduled rather than guessing.
- Resolve relative dates only against a known reference date/time and timezone. Present the resolved value for confirmation if ambiguous; timezone context alone cannot supply a date.

## Processing and Prioritization

1. Separate user-stated outcome, directly observed state, user-provided claims, unknowns, and recommendations.
2. Identify the smallest useful deliverable that advances the outcome and can be verified. Derive mandatory gates only from explicit project acceptance criteria, specifications/design, or evidenced dependencies and current state. Distinguish required validation from optional capability checks; availability or mention of an integration does not make testing it required. If one qualifying channel/service verification satisfies an explicit requirement, do not add tests of other optional integrations unless another explicit requirement requires them. Keep useful extra checks as optional follow-up, not a completion gate. Respect phase gates that apply; do not recommend implementation before necessary understanding, inspection, specification/checklist, or architecture.
3. Map real prerequisites, acceptance gates, and blockers before ordering actions. Distinguish established dependencies from suspected ones, and actions available now from work gated on a prerequisite, decision, or external dependency. Where a required validation/behavioral acceptance gate is outstanding, plan it before commit/push/publish/delivery; treat structural/readiness checks as evidence only for what they directly verify. A Git milestone may be recommended only after the relevant required gate passes, unless an explicit project strategy says otherwise.
4. Prioritize immediate action first, then a small set of later actions. Give each a concrete verb, deliverable, rationale or recommendation, dependency, and completion criterion. Mark recommendations as recommendations when the rationale is not directly supplied.
5. Separate required decisions (choice, decider only if known, and which actions it gates), date-specific schedule candidates, and unscheduled actions. A proposal is not a fact, commitment, or event.
6. Return a useful local plan even when Calendar is unavailable; no Calendar activity is needed for ordinary planning.

## Output Schema

Return exactly these 11 top-level fields in order. If nothing applies, use `None identified` or `Not provided`; do not invent content:

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

- **Project / Current phase/state:** Include only facts supported by direct evidence or clearly label user-provided information. Unknown state remains `Unknown` or `Not verified`.
- **Desired outcome:** Preserve Franck's requested outcome; ask if it cannot be identified.
- **Immediate next action:** Give one bounded, executable step and its deliverable, or ask for information if no justified action can be selected.
- **Why it is next:** Explain actual sequencing/prerequisite evidence, or mark the rationale as a recommendation when inferred. State which explicit acceptance/validation gate is outstanding when it blocks Git or delivery; do not treat implementation completion or structural checks as acceptance by themselves.
- **Dependencies:** State prerequisite/blocker, evidence status, owner only when known, and effect on action order. Keep suspected dependencies explicitly suspected; do not assert a hypothesis as a blocker.
- **Additional actions:** List only a small prioritized set of later actions, each with reason and known prerequisite.
- **Decisions required:** Name the decision, decider if known, and the actions it gates. Do not invent authority.
- **Schedule candidates:** Include only legitimate dated candidates; label user-provided details versus proposals, timezone, missing fields, and authorization state. A candidate is not a Calendar event.
- **Unscheduled actions:** Include actions with no justified date/time, including those awaiting a decision or deadline; explicitly say they remain unscheduled.
- **Completion/verification criteria:** Give observable evidence for each action and for any authorized event. Never state a planned result as already complete.

## Google Calendar: Preparation, Authorization, Execution, Verification

1. **Preparation:** Complete the local plan first. Show event candidate details separately, marking supplied facts, proposals, and unknowns. Planning alone creates no remote change.
2. **Authorization:** A plan/next-step request is not Calendar authorization. For an event request, show what will be created or modified, with title and relevant date/time/timezone when known. Obtain confirmation when the action is consequential, the request/design requires it, or material details are ambiguous. A proposed date is not authorized until Franck confirms it for the event.
3. **Execution:** Safely verify current availability of the existing approved Calendar tool only if an explicitly authorized event is actually to be created or modified. Do not turn optional Calendar or other integration support into a project acceptance requirement or prerequisite merely because the capability exists or appears in design context. Resolve required title, date, start time, timezone, duration/end time, and other tool-required fields. Clarify conflicts or omissions. Use only that existing connection; never add or reconfigure a service, API, OAuth, plugin, or credential. Create/modify only the specifically authorized event(s), not the plan as a whole.
4. **Verification:** Independently inspect the destination event and compare its existence and visible details with the authorized request: title, date, start/end time, timezone, and other requested fields. Report non-secret evidence and any unverified detail. Do not treat a tool response alone as success. On failure or uncertainty, do not blindly retry or create a duplicate.

## Failure Behavior

- If goal/state is too vague, ask a targeted question rather than manufacture a generic plan.
- If evidence for status/dependencies is incomplete, preserve uncertainty and, when helpful, propose a bounded discovery/verification action.
- If dates, event details, or authorization are absent or conflicting, leave work unscheduled, return the local plan/candidate, and ask only necessary clarifying questions; do not silently select a time.
- If Calendar is unavailable, execution fails, or verification cannot establish success, return the local plan and precisely report `Not verified`/unverified event status. Do not claim that no event exists if creation may have succeeded; avoid duplicates and offer a safe verification step.
- Protect private context and secrets. Do not present hypotheses as facts, proposals as commitments, plans as completed actions, or a tool success message as verified external state.

## Success Criteria

- All 11 schema fields appear in order; the plan is small, sequenced, specific to known state, and tied to the requested outcome.
- Immediate work, later work, blockers/dependencies, decisions, schedule candidates, and unscheduled actions are distinct.
- Relevant phase order is respected without mechanically including irrelevant stages; explicit acceptance/validation gates precede commit or delivery when required, and actions are concrete with observable completion criteria.
- Required project gates are derived from explicit acceptance criteria and evidenced dependencies; optional integrations are not made mandatory, and the smallest verification satisfying the requirement is preferred.
- No deadline/date is invented; absent a justified date, actions remain unscheduled unless a date is explicitly requested as a proposal.
- Planning-only requests cause no external effect.
- Any authorized Calendar action uses currently verified existing capability, contains resolved required details, is limited to approved events, and is independently verified. If any condition fails, report the limitation without claiming success.
