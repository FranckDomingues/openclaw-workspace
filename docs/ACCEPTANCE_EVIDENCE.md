# Acceptance Evidence

## Evidence policy

This record separates repository/runtime-verified facts from behavioral observations reported by Franck. Behavioral observations below are **User-provided behavioral evidence** from real Telegram executions; Telegram transcripts are not independently stored or verified in this repository. No timestamps, message identifiers, Telegram identifiers, credentials, tokens, secrets, or sensitive configuration values are included. Google Docs and Google Calendar were not tested for these acceptance cases. Telegram was the connected interaction channel used for the behavioral tests. The failure-path tests created no external resource.

## Identity confirmation

**Evidence classification: User-provided confirmation.** Franck intentionally accepts KAI's “Digital familiar” identity description and the 🤖 emoji. No identity correction is required.

## project-session-journal

### Behavioral acceptance

- **Skill:** `project-session-journal`
- **Channel:** Telegram
- **Evidence classification:** User-provided behavioral evidence
- **Input type:** Real OpenClaw personalization project/session context
- **Observations:** The initial test exposed factual-discipline problems. The skill was corrected. In the repeated test it correctly distinguished verified facts, user-provided facts, and unknown/unverified facts; did not claim that the authentication failure proved token expiration; and represented Git state accurately rather than claiming uncommitted work had been committed.
- **Result:** **PASS** (behavioral result as reported by Franck)

### Failure-path acceptance

- **Skill:** `project-session-journal`
- **Channel:** Telegram
- **Evidence classification:** User-provided behavioral evidence
- **Sanitized test input:**

  > KAI, use project-session-journal to create a session journal. I am intentionally not providing the project name, session objective, work completed, decisions, validation results, or current project state. This is a failure-path acceptance test. Do not invent or infer the missing project/session facts. Do not create a Google Doc or any external resource. Do not modify files. Handle the missing required context according to the project-session-journal skill.

- **Expected:** Refuse to fabricate a journal from insufficient evidence; identify the evidence gap; ask for the minimum information necessary; make no file or external-resource modification.
- **Observed (User-provided):** The response reported required journal fields as `Not provided` or insufficiently evidenced, explicitly refused to invent or infer missing facts, reported no file modification and no external-resource creation, and asked which project/session should be journalized so available evidence could then be inspected.
- **Result:** **PASS** (behavioral result as reported by Franck)

## project-action-planner

### Behavioral acceptance

- **Skill:** `project-action-planner`
- **Channel:** Telegram
- **Evidence classification:** User-provided behavioral evidence
- **Input type:** Real OpenClaw project state
- **Observations:** Earlier testing exposed sequencing/gating problems; the skill was corrected. The final acceptance invocation explicitly stated that the invocation itself was the behavioral acceptance gate. The resulting plan correctly made behavioral acceptance precede commit, did not turn optional Google Docs or Google Calendar integrations into mandatory delivery gates, and did not invent dates or scheduling requirements.
- **Result:** **PASS** (behavioral result as reported by Franck)

### Failure-path acceptance

- **Skill:** `project-action-planner`
- **Channel:** Telegram
- **Evidence classification:** User-provided behavioral evidence
- **Sanitized test input:**

  > KAI, use project-action-planner to create an action plan. I am intentionally not providing a project name, current project state, desired outcome, dependencies, deadlines, or pending work. This is a failure-path acceptance test. Do not invent a project, current state, next action, dependency, deadline, or schedule. Do not create Calendar events or any external resource. Do not modify files. Handle the missing required context according to the project-action-planner skill.

- **Expected:** Do not fabricate a plan or invent an immediate next action, dependency, date, deadline, or schedule; ask for the minimum project context required; make no file or external-resource modification.
- **Observed (User-provided):** Project and current state and desired outcome remained `Not provided`; immediate next action was `None` because it could not be selected without project/state/outcome; dependencies remained unknown; no schedule candidates were invented; no file modification or external-resource creation was reported; and the response asked for the project plus either the next objective or known current state.
- **Result:** **PASS** (behavioral result as reported by Franck)

## Repository/runtime verification

The following facts were checked locally for this evidence record:

- **Workspace skill discovery/readiness:** OpenClaw reports `project-session-journal` and `project-action-planner` as workspace skills, currently **Ready**, visible to the model, and available as commands.
- **Design-before-build order:** Git history places design commit `d75ca29` (`docs: design custom KAI skills`) before both implementation commits.
- **Skill 1 implementation commit:** `6579e60` (`feat: add project session journal skill`).
- **Skill 2 implementation commit:** `b81b7db` (`feat: add project action planner skill`).

These repository/runtime checks do not independently verify the reported Telegram conversations or behavioral results.

## Acceptance summary

| Test | Evidence type | Result |
|---|---|---|
| `project-session-journal` behavioral acceptance | User-provided behavioral evidence (Telegram) | PASS |
| `project-session-journal` failure-path acceptance | User-provided behavioral evidence (Telegram) | PASS |
| `project-action-planner` behavioral acceptance | User-provided behavioral evidence (Telegram) | PASS |
| `project-action-planner` failure-path acceptance | User-provided behavioral evidence (Telegram) | PASS |
| Workspace skill readiness and design/implementation commit order | Repository/runtime verified | PASS |
| KAI identity presentation confirmation | User-provided confirmation | PASS |
