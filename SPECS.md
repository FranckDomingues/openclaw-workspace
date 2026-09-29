# Mi Agente, A Mi Manera — Project Specification

## 1. Project Objective

Personalize the existing OpenClaw assistant **KAI** for Francisco, then design, implement, and validate at least two genuine OpenClaw custom skills that embody that personalization. At least one skill must create a real, independently verifiable result through a service already connected to this workspace. Completion means the five personalization files express confirmed, useful context and behavior; the skills pass real-input tests; project health and unrelated baseline warnings are reported accurately; and a reviewed, secret-free project history and evidence package are delivered to the existing GitHub repository.

This specification is the contract for later implementation phases. This iteration creates the contract only; it does not authorize skill implementation, configuration changes, or repository delivery actions.

## 2. Existing Baseline

### Verified in the workspace during this audit

- Workspace root: `/root/.openclaw/workspace`.
- OpenClaw CLI reports version `2026.7.1-2` (build `0790d9f`).
- Git branch is `main`; `origin` points to `git@github.com:FranckDomingues/openclaw-workspace.git`.
- The root repository has existing history. Its tracked files include `.gitignore`, the five personalization files, `HEARTBEAT.md`, `docs/terminal-history-sanitized.txt`, and `openclaw-workspace-state.json`.
- The root Git status before this specification was created contained untracked `config/`, `notes.md`, `openclaw-connection/`, and `openclaw-connection-FranckDomingues/`. The latter includes its own Git metadata and connection evidence. These are pre-existing user data and must not be indiscriminately staged, modified, or removed.
- `IDENTITY.md` currently says `Kai` and contains starter identity text; the project requirement is to retain the agent name as **KAI**.
- `SOUL.md` and `AGENTS.md` contain substantial baseline OpenClaw guidance, but not yet the required Francisco-specific operating agreement. `USER.md` has undiscovered/unknown template fields. `TOOLS.md` is example/template content rather than an inventory of this deployment.
- Root `notes.md` records a VPS deployment, a Telegram bot connection, Zapier MCP through MCPorter using OAuth, and Google Docs and Google Calendar as the enabled services in the previous milestone. It says that a Telegram request created a Google Doc and a Calendar review event, and that KAI requested missing information before an incomplete request was executed.
- The untracked connection evidence manifest lists screenshots for Telegram, Zapier MCP, Google Docs, Google Calendar, and the clarification-to-completion flow. These records establish prior milestone evidence, not a fresh live connectivity check in this specification phase.
- `config/mcporter.json` has a top-level `mcpServers` key and a `zapier` server entry. Its contents were not printed or copied into this specification.
- Sanitized command history and root Git history were inspected. The root repository has history that must be preserved. The separate nested connection archive has its own history; neither repository's history is to be rewritten.

### Reported diagnostic baseline to verify in a later phase

The assignment reports that `openclaw doctor` currently identifies: (a) a Telegram/message-tool warning for agent `main`; (b) OpenAI Memory Search configured without an API key; (c) plaintext secret-bearing fields in `openclaw.json`; and (d) optional skills whose runtime requirements are missing. These are recorded as **reported baseline findings**, not findings reproduced by running `openclaw doctor` in this specification-only audit. Later QA must re-check current output safely, distinguish project-blocking failures from unrelated environment warnings, and must not expose or copy secret values. None of these findings is authorization to repair or alter credentials, integrations, or OpenClaw configuration without separate approval.

### Facts not established by available evidence

Francisco's preferred form of address, pronouns, timezone, language preference, role/employer, work domain, active project details, and communication preferences beyond those explicitly set in this assignment are **TBD**. Do not infer or invent them. Gather only useful context and confirm it with Francisco before adding it to the personalization files.

## 3. Scope

The complete project scope, for later authorized phases, is to:

1. Personalize `IDENTITY.md`, `SOUL.md`, `AGENTS.md`, `USER.md`, and `TOOLS.md` for KAI and Francisco using verified facts and confirmed decisions.
2. Run and review `openclaw doctor` after the personalization changes, recording relevant results without confusing pre-existing, unrelated warnings with project regressions.
3. Create a root-level `SKILLS_DESIGN.md` that answers all design-gate questions for every proposed skill and explains rationale before implementation.
4. Commit `SKILLS_DESIGN.md` in its own bounded design-gate commit before any skill implementation is committed.
5. Implement at least two native OpenClaw custom skills, selected only after the design gate, and validate each with realistic Francisco/project input.
6. Verify at least one skill's successful real output in a confirmed, pre-existing connected service, without adding a connection or service.
7. Retain reviewable, secret-free evidence of personalization, design-before-implementation, skill behavior, connected-service output, final QA, and skill design rationale.
8. Preserve the existing repository and connected infrastructure; deliver through the existing GitHub remote only after validation and explicit phase completion.

Only the creation and review of `SPECS.md` are authorized in this iteration.

## 4. Out of Scope

- Implementing, installing, enabling, or invoking a custom skill in this specification iteration.
- Editing OpenClaw configuration, `openclaw.json`, `config/mcporter.json`, gateway/service settings, or any credentials.
- Editing any file other than this `SPECS.md` in this iteration; specifically, do not modify `IDENTITY.md`, `SOUL.md`, `AGENTS.md`, `USER.md`, `TOOLS.md`, `HEARTBEAT.md`, or `notes.md` now.
- Running `openclaw doctor --fix` or attempting to clear warnings as part of this phase.
- Installing packages/runtimes or configuring APIs, OAuth, MCP servers, plugins, external services, or new integrations.
- Assuming that every doctor warning or missing optional runtime is in assignment scope or must be fixed.
- Choosing final skills before the design stage has reviewed candidate suitability and service/tool availability.
- Replacing, removing, or disrupting the existing Telegram, Zapier/MCPorter, Google Docs, or Google Calendar setup.
- Rewriting Git history, modifying/deleting pre-existing untracked items, or including secrets or raw sensitive configuration in repository evidence.
- Staging, committing, or pushing during this specification iteration.

## 5. KAI Personalization Requirements

All changes to the five files in the later personalization phase require review before use. They must be specific to KAI and Francisco, distinguish confirmed facts from TBD items, and avoid redundant personal data. Unknown facts must remain explicitly unknown until Francisco confirms them.

### IDENTITY.md

- **Purpose:** Define KAI's stable identity and presentation.
- **Required personalization:** Retain the name **KAI**; establish a clear, appropriate identity/presentation for this assistant using details confirmed by Francisco. Do not presume that current starter “digital familiar” or emoji text is an agreed identity. Leave an avatar unset unless Francisco selects one.
- **Observable acceptance criteria:** The name field is exactly KAI; no conflicting agent name appears as the identity; presentation details are intentional rather than unreviewed template defaults; unsupported identity facts and avatar choices are absent or marked TBD.

### SOUL.md

- **Purpose:** Set KAI's personality, judgment, communication, and working style.
- **Required personalization:** State directness; when KAI acts first on safe/reversible internal work versus asks first for ambiguity, sensitive decisions, or external effects; values and priorities; and practical communication/work conventions. Make the rules compatible with Francisco's confirmed preferences and assignment safety constraints.
- **Observable acceptance criteria:** A reviewer can locate explicit guidance for directness, ask-versus-act decisions, values, and communication/work style. The guidance is actionable, consistent with `AGENTS.md`, and does not present unconfirmed preferences as facts.

### AGENTS.md

- **Purpose:** Provide durable operational rules, workspace conventions, safety boundaries, and continuity behavior.
- **Required personalization:** Preserve useful applicable OpenClaw guidance while making KAI's durable rules specific to this workspace: inspect before changing configuration; protect private data and secrets; confirm appropriate external/destructive actions; use connected services only for their intended purpose; honor skill instructions; and keep repository changes bounded and reviewable. Avoid contradictory, unsafe, or irrelevant template instructions.
- **Observable acceptance criteria:** Concrete, non-contradictory operating rules cover safe internal work, configuration inspection, external action/confirmation, privacy, destructive operations, and project/repository work. The file does not authorize disclosure or bypass confirmation boundaries.

### USER.md

- **Purpose:** Give KAI the minimum useful, durable context needed to help Francisco.
- **Required personalization:** Identify Francisco as the user because this project brief names him. Add his preferred form of address, pronouns, timezone, language, role/work domain, active project context, and communication preferences only when confirmed and useful. Keep unsupported items marked TBD or omit them; do not build an unnecessary personal dossier.
- **Observable acceptance criteria:** The file contains no invented biography or preferences; confirmed project-relevant facts can be traced to Francisco's confirmation or repository evidence; unresolved fields are accurately TBD/unknown; retained personal context is relevant and proportionate.

### TOOLS.md

- **Purpose:** Document actual environment-specific connected tools and safe usage conventions, not generic skill descriptions or credentials.
- **Required personalization:** Describe only services/environment verified by current safe inspection or prior evidence with its status clearly qualified: VPS OpenClaw, Telegram, Zapier MCP via MCPorter, and Google Docs/Calendar have prior milestone evidence. State use conventions, verification expectations, and confirmation boundaries. Identify unknown tool details as TBD. Never include tokens, OAuth material, secret values, or sensitive endpoints.
- **Observable acceptance criteria:** Each claimed current connection has a cited/retained non-secret verification source and status/date qualification; prior milestone evidence is not misrepresented as a present live check; no credentials or secret-bearing values appear; conventions explain when/how to use relevant tools and when to ask.

## 6. Skill Design Gate

Before implementing **any** custom skill, a root-level `SKILLS_DESIGN.md` must be reviewed and committed. Its commit must precede all skill implementation commits in the root repository's history. The design document must address each candidate and each approved skill individually, and answer all of these mandatory questions:

1. **What does this skill do?** State its purpose in one sentence.
2. **What input does the agent need?** Specify the input format and identify which required information is already available from `IDENTITY.md`, `SOUL.md`, `AGENTS.md`, `USER.md`, and `TOOLS.md` versus which information must be supplied or clarified for each run.
3. **What does good output look like?** Specify output format, destination, and independently testable success criteria, including whether an external/connected-service action is involved and what confirmation it needs.

The design must also state rationale, safety/failure behavior, relevant connected-tool dependency, and a test approach for each skill. A candidate is not implementation approval: final selection depends on documented design, available verified tools, and confirmation of required personal/project details. The design-gate commit must contain the design document only or otherwise be tightly scoped so its precedence is unambiguous.

## 7. Custom Skill Requirements

At least two distinct, properly packaged **OpenClaw custom skills** are required in later implementation phases. They must use the native OpenClaw skill mechanism and documented skill structure/metadata for the installed version; a prompt pasted into a terminal, a one-off instruction, or an ordinary script without native skill discovery/use is insufficient. Confirm the exact supported structure and invocation method during design/implementation rather than guessing.

Each skill must:

- Have one clear purpose, bounded inputs/outputs, and a documented design in the committed `SKILLS_DESIGN.md`.
- Use and visibly reflect KAI's finalized identity, personality/work style, durable rules, Francisco-specific confirmed context, and real tool conventions as relevant. Evidence must show behavior/output tied to those file contents, not merely mention the filenames.
- Avoid asking again for information already reliably available in the personalization files; ask Francisco for missing or ambiguous information needed to proceed.
- Include meaningful success and failure paths, input validation, and safe handling of external side effects.
- Be tested with realistic personal/project input supplied or confirmed by Francisco, not placeholders or fabricated profile facts.
- Have its changes and tests independently reviewable and scoped to that skill.

**Candidates only—not approved skill selections:** A Google Docs drafting/creation workflow based on a real Francisco project brief, and a Google Calendar planning/scheduling workflow based on a real confirmed event request are plausible because prior milestone notes record those services and outputs. These examples are not authorization, and must not be selected unless design review confirms their usefulness, input requirements, native OpenClaw feasibility, and current safe tool availability. Other suitable candidates may be proposed in `SKILLS_DESIGN.md`. No skill is selected or implemented by this specification.

## 8. Connected-Service Requirement

At least one approved skill must produce a real output through an already-connected service confirmed to be available during the implementation/verification phase. Prior records support Telegram and Zapier MCP/MCPorter use with Google Docs and Google Calendar, but do not establish that those connections are currently healthy. Re-check availability without exposing credentials; do not configure a replacement or new connection. Use a real, Francisco-approved project request. Capture non-secret evidence such as the resulting document/event identifier or title, destination confirmation, timestamp, and skill invocation/test record; redact personal or sensitive content where appropriate. Verify the result in the destination service and verify the response back to the user. Do not treat a success message alone as proof of a created artifact. Obtain confirmation before consequential external actions, and do not create duplicate or unwanted outputs during testing.

## 9. Testing Strategy

Later phases must use a written, reproducible test record with date, version/context, sanitized inputs, expected result, observed result, and pass/fail status.

1. **Baseline and configuration:** Record safe `openclaw doctor` output before/after personalization as appropriate; inspect relevant config state before any authorized change. Check that all five files are readable, consistent, contain no unsupported personal claims, and contain no credentials. Do not invoke `--fix` under this contract.
2. **Personalization:** Review each file against its file-specific observable criteria. Use a traceability check from confirmed user facts to `USER.md` and from requirements to the corresponding operational rules; verify KAI remains KAI.
3. **Skill discovery and behavior:** Validate each skill's native OpenClaw packaging/discovery and invocation using the installed version's supported mechanism. Exercise one realistic happy path and relevant missing-input/invalid-input or refusal/failure path per skill. Confirm the skill follows all five personalization files where relevant and does not reveal them inappropriately.
4. **Connected service:** Execute a user-approved real request through an existing verified connection for at least one skill. Independently inspect the resulting destination artifact and cross-check it against the request and skill output. Record a safe proof, not a secret or a success assertion alone.
5. **Safety and regression:** Check that no new API/OAuth/MCP/plugin/service is introduced; existing connections/configuration were preserved; external actions follow confirmation requirements; no duplicate/destructive output is produced; unrelated doctor warnings are categorized rather than misreported as skill failures.
6. **Final QA:** Review diffs, repository status, commits/order, evidence, and secret exposure before delivery. Any failure or unresolved dependency is reported explicitly; do not claim completion while a mandatory criterion is unverified.

## 10. Security and Safety Constraints

- No secrets, tokens, private keys, OAuth data, credential-bearing URLs, or raw secret-bearing config may enter Git, documentation, screenshots, logs, or evidence.
- Never copy credential values into notes or use tools in a way that prints them. Redact sensitive evidence before retention.
- No new API, OAuth configuration, MCP server, plugin, package, runtime, or external service may be introduced for this assignment without a separately approved scope change; this project requires reuse of existing connected tools.
- Preserve existing working connections. Before any authorized configuration change, inspect the current state, identify precisely what must change, and preserve/merge existing settings by default. This specification phase authorizes no such change.
- External side effects (for example creating or modifying a remote document or calendar event, sending a message, or publishing) require the appropriate user confirmation and must be narrowly limited to the approved test request.
- Do not use destructive commands or delete/overwrite existing data as a convenience. Obtain explicit approval before irreversible actions.
- Treat reported plaintext-secret configuration as a security risk to handle only under a separately approved, carefully scoped remediation; do not expose or “fix” it during this project by silently changing credentials or configuration.

## 11. Git and Repository Rules

- Preserve existing root and nested repository histories; do not rewrite history or alter nested repository contents as part of root delivery.
- Inspect `git status --short` before every staging/commit operation and after each bounded change.
- Never use `git add .` or another broad staging operation. Stage only explicitly reviewed, intentional project files by path.
- Treat the pre-existing untracked `config/`, `notes.md`, `openclaw-connection/`, and `openclaw-connection-FranckDomingues/` as protected user data. Do not modify/delete or indiscriminately stage them; inspect any proposed inclusion and secret exposure first.
- Use bounded commits by project phase, with clear messages and content. Preserve the existing commit history.
- Commit `SKILLS_DESIGN.md` before any custom skill implementation is committed. Verify that ordering in `git log` before proceeding to implementation.
- Do not push until the relevant phase is validated, reviewed, and authorized for delivery. Do not push during this specification iteration.
- This iteration: create only `SPECS.md`; do not stage, commit, or push it.

## 12. Project Phases

| Phase | Description | Status at this specification iteration |
|---|---|---|
| 0 — Assignment understanding | Extract deliverables, constraints, and required sequencing from the assignment. | **Complete** |
| 1 — Environment recovery and baseline audit | Confirm workspace/repository/version, inspect existing documentation and evidence, record known diagnostics without exposing credentials. No recovery/fixes are authorized here. | **Audit complete from available evidence; reported doctor findings not rerun in this iteration** |
| 2 — SPECS.md | Create and review this executable project contract; confirm requirement traceability and Git status. | **Complete; this file only** |
| 3 — Personalize five configuration files | After approval, confirm Francisco-specific details and update IDENTITY/SOUL/AGENTS/USER/TOOLS. | **Not started** |
| 4 — Validate configuration/OpenClaw | Review personalized files and run `openclaw doctor`; classify baseline warnings and regressions without `--fix`. | **Not started** |
| 5 — SKILLS_DESIGN.md | Design and compare candidate skills; answer the mandatory questions for each and record rationale/tests. | **Not started** |
| 6 — Commit design gate | Review and commit `SKILLS_DESIGN.md` before implementing any skill. | **Not started** |
| 7 — Skill 1 implementation and validation | Implement one approved native OpenClaw skill and test with realistic input. | **Not started** |
| 8 — Skill 2 implementation and validation | Implement a second approved native OpenClaw skill and test with realistic input. | **Not started** |
| 9 — Connected-service integration verification | Verify at least one skill's actual output in an existing connected service. | **Not started** |
| 10 — Final QA | Run acceptance tests, security/repository review, and retain sanitized evidence. | **Not started** |
| 11 — GitHub delivery | Make bounded commits and push reviewed, validated project changes to the existing remote. | **Not started** |

## 13. Acceptance Criteria

Each criterion is independently verifiable. `PASS` requires retained non-secret evidence or a reviewable repository artifact; an unverified assertion is not a pass.

- **REQ-001 — Specification-only scope:** In this iteration, `SPECS.md` is the only file created or modified; no implementation, configuration change, installation, staging, commit, or push occurs.
- **REQ-002 — Baseline accuracy:** `SPECS.md` records the verified workspace path, OpenClaw version, branch/remote, tracked/untracked-state caution, five-file baseline, and prior service-evidence qualification without publishing credentials.
- **REQ-003 — Diagnostic qualification:** The four reported diagnostic findings (Telegram/message-tool warning for `main`, OpenAI Memory Search without key, secret-bearing plaintext fields in `openclaw.json`, and optional skill runtime gaps) are listed as reported baseline constraints, not falsely claimed as freshly reproduced or automatically in scope to fix.
- **REQ-004 — No invented profile:** Any Francisco-specific fact not supported by repository evidence or explicit confirmation is marked TBD/unknown or omitted; the specification explicitly identifies the key unknown profile fields.
- **REQ-005 — KAI identity:** The later identity acceptance criteria preserve the exact agent name **KAI** and prohibit an unconfirmed replacement identity.
- **REQ-006 — SOUL coverage:** The later SOUL criteria explicitly require directness, ask-first/act-first rules, values, and communication/work style, with observable review checks.
- **REQ-007 — AGENTS coverage:** The later AGENTS criteria require durable operational conventions, privacy, configuration inspection, external/destructive-action boundaries, and repository safety.
- **REQ-008 — USER coverage:** The later USER criteria require useful, confirmed Francisco/work/project context, proportionate collection, and no unnecessary dossier or invented details.
- **REQ-009 — TOOLS accuracy:** The later TOOLS criteria distinguish prior milestone evidence from current live connection verification and require real environment conventions without credentials.
- **REQ-010 — Design-before-build:** A root `SKILLS_DESIGN.md` must be reviewed and committed before any custom skill implementation is committed; Git history must demonstrate the ordering.
- **REQ-011 — Required design answers:** For every proposed skill, `SKILLS_DESIGN.md` answers in writing: one-sentence purpose; input format and information supplied by each of the five config files versus run-time input; output format, destination, and measurable success criteria.
- **REQ-012 — Design rationale and tests:** Each skill's design records rationale, relevant dependencies, safety/failure behavior, and a test approach.
- **REQ-013 — Native skill count:** At least two distinct skills are discoverable/invocable using the documented native OpenClaw custom-skill mechanism for the installed version; raw prompts/ad-hoc instructions alone do not qualify.
- **REQ-014 — Personalization reflected in skills:** For each implemented skill, its real-input test evidence demonstrates relevant behavior tied to the finalized contents of IDENTITY.md, SOUL.md, AGENTS.md, USER.md, and TOOLS.md, not merely filename references.
- **REQ-015 — Real input:** Both skills have a documented test using realistic Francisco/project input that Francisco supplied or confirmed; placeholder-only tests do not pass.
- **REQ-016 — Existing service reuse:** At least one skill creates a real output through an already-connected service whose availability is re-confirmed at test time; no new integration is configured.
- **REQ-017 — Output verification:** The connected-service test independently verifies the destination artifact and matches its relevant properties to the user-approved request; a chat success message alone does not pass.
- **REQ-018 — Service safety:** External test actions are appropriately confirmed, narrowly scoped, and checked for duplicate or unintended side effects.
- **REQ-019 — Configuration health review:** After personalization, `openclaw doctor` is run and its relevant output is retained safely; no `--fix` is run under this specification.
- **REQ-020 — Warning classification:** Final QA distinguishes project regressions/blockers from the reported unrelated/environment findings and does not claim that all warnings must be cleared.
- **REQ-021 — Existing connections preserved:** Reviewable before/after evidence shows that no existing working integration was unnecessarily replaced or disrupted.
- **REQ-022 — No new services/secrets:** Repository and change review shows no newly configured API/OAuth/MCP/plugin/service and no credentials or secret-bearing values in project files/evidence.
- **REQ-023 — Existing history preserved:** Root and nested repository histories remain intact; no history rewrite or unauthorized untracked-file modification/deletion occurs.
- **REQ-024 — Bounded staging:** Every project commit contains only explicitly reviewed intended files, supported by pre-stage and post-commit status/diff review; no broad `git add .` is used.
- **REQ-025 — Phase sequencing:** Commits and phase evidence show personalization/health review before skill implementation, design commit before both implementation commits, and validation before GitHub push.
- **REQ-026 — Evidence completeness:** Final retained evidence covers all five personalized files, the committed design and rationale, both skills and real-input tests, at least one independently verified connected-service result, health/final QA, and GitHub delivery without credentials.
- **REQ-027 — Skill failure behavior:** Each skill's test record covers at least one relevant invalid/missing-input or refusal/failure case and shows safe, comprehensible behavior without unintended external action.
- **REQ-028 — Final delivery:** Only after all mandatory criteria pass and changes are reviewed, the intended project commits are pushed to the existing GitHub remote and the resulting commit/repository state is recorded.

## 14. Definition of Done

The project is complete and ready for submission only when all of the following are true:

1. The five personalization files have been reviewed and accepted; KAI remains the name; unknown personal details were not invented.
2. `openclaw doctor` was run after personalization, relevant output was safely recorded, and warnings/regressions were accurately classified without unauthorized fixes.
3. `SKILLS_DESIGN.md` answers all required design questions for every skill and is committed before any skill implementation commit.
4. At least two distinct native OpenClaw skills are implemented, discoverable, invoked, and validated with real Francisco/project input and relevant failure cases.
5. Skill evidence demonstrates the intended personalization across the five files, as relevant to each behavior.
6. At least one skill produces an independently verified real output through an existing, currently confirmed connected service, using user-approved input and appropriate external-action confirmation.
7. No new integration/service was configured; existing working connections were preserved; no secrets were committed or included in evidence; unrelated baseline constraints are disclosed accurately.
8. Git changes are bounded and reviewed, existing history/untracked user data is preserved, all required evidence is retained, and validated project commits are delivered to the existing GitHub repository.
9. Every mandatory acceptance criterion is marked pass with evidence, or the project is explicitly reported as incomplete with blockers; no unverified criterion is represented as complete.

## 15. Evidence Checklist

Retain only evidence that is necessary, reviewable, and sanitized. Do not retain credentials, tokens, private keys, OAuth data, secret-bearing config, or unnecessary personal content.

- [ ] Baseline record: date, workspace/repository, OpenClaw version, branch, sanitized starting status, and source/qualification of prior connection evidence.
- [ ] Before/after diffs or reviewed copies of all five personalized files, showing KAI identity and confirmed—not invented—Francisco context.
- [ ] Sanitized `openclaw doctor` result after personalization, with baseline warnings and project-specific failures classified.
- [ ] `SKILLS_DESIGN.md` content, design rationale, review record, and commit identifier/time proving design was committed before implementation.
- [ ] For each of two skills: native skill path/metadata (excluding secrets), invocation/discovery proof, approved realistic input (redacted as needed), expected/observed result, failure-path test, and pass/fail review.
- [ ] For at least one service-backed skill: safe proof of the destination artifact (for example non-sensitive title/identifier and timestamp), cross-checked against the skill result and approved request; no credential-bearing screenshot or raw message history.
- [ ] Final QA record mapping each REQ to its evidence, including integration-preservation and no-new-service checks.
- [ ] Git status/diff and bounded commit identifiers, including design-gate-before-skill ordering and final push/remote result.
- [ ] Explicit list of unresolved TBD items, warnings, limitations, or accepted risks at delivery.
