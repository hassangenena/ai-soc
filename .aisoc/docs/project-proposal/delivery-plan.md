# AISOC Farm — Delivery Plan (Selectable Timelines)

**Companion to:** [`proposal.md`](./proposal.md) (timeline-neutral) and
[`test-worksheet.md`](./test-worksheet.md) (per-agent pass bar)
**Purpose:** Hold the **variable** part of the project — the schedule,
checkpoints, deliverable sizes, per-phase grade split, submission workflow,
onboarding window, and budget guideline — separately from the proposal so a
cohort can be run on a different calendar without forking the proposal or the
worksheet.

The proposal defines the three fixed phases (**P1** Specification, **P2**
Cross-Environment Testing & Hardening, **P3** Farm Integration & Defense). This
document maps those phases onto a concrete timeline. Two variants ship today;
[adding a new one](#adding-a-new-variant) is a copy-paste, not a fork.

---

## Choosing a variant

| If your cohort has… | Use |
| --- | --- |
| A full half-semester (≈6 weeks) with weekly contact | [**6-Week / 3-Milestone Plan**](#6-week--3-milestone-plan) — more iteration, deeper artefacts, milestone-distributed grade |
| A block course, summer module, or late-semester capstone (≈3 weeks) | [**3-Week / Single-Milestone Plan**](#3-week--single-milestone-plan) — intensive sprint, formative weekly checkpoints, single consolidated grade |

**Scope is identical at the agent level in every variant** — one student ships
one fully specified, tested, portable, integrated, and defended agent. A
shorter timeline trims iteration count, artefact depth, peer-parseability
breadth, and live-exercise size — never the agent contract.

### Variant comparison

| Item | 6-Week / 3-Milestone | 3-Week / Single-Milestone |
| --- | --- | --- |
| Calendar | 3 milestones over 6 weeks | 1 milestone, 3 weekly checkpoints over 3 weeks |
| Grade split | 20% / 40% / 40% across milestones | 100% on one milestone (checkpoints formative) |
| Onboarding window | Day 1 session, before P1 | Day 0 pre-session, the week before Week 1 |
| Personal test scenarios | 3 (positive / negative / ambiguous) | 2 (positive + negative; ambiguous encouraged, ungraded) |
| Peer-parseability requirement | Output consumed by **2** peer agents | **1** peer agent (instructor-paired) |
| Adversarial input testing | Student-authored injection samples | One common injection sample, instructor-provided |
| Evaluation note (end of P2) | ≤ 2 pages | ≤ 1 page |
| Final report | 4–6 pages | 2–3 pages |
| Defense | ≤ 10 min + slides | ≤ 5 min + 3-min Q&A, no slides |
| Live walk-through | Full multi-stage attack narrative | Single reference scenario |
| Submission workflow | One PR per milestone | One draft PR, labelled per checkpoint |
| Budget guideline | ~€25 / student / month | ~€15 / student over the sprint |
| Reserve agent #19 | Allowed | Strongly discouraged unless prior Sigma + KQL exposure |

> The **evaluation dimension weights** (RICTOC quality 25%, portability 15%,
> detection 20%, integration 15%, explainability & safety 15%, documentation
> & defense 10%) are identical in both variants — see
> [`proposal.md`](./proposal.md) § 6. Only the **per-phase split** of the
> project total differs, and that split is given in each plan below.

---

## 6-Week / 3-Milestone Plan

The full project weight is distributed **20 / 40 / 40** across three milestones,
giving the prompt time to settle between iterations and producing deeper
written artefacts.

**Onboarding window.** The five-step bring-up in [`proposal.md`](./proposal.md)
§ 3.5 runs as a **Day 1 session** before Milestone 1 begins; the two captured
transcripts are the Day 1 readiness deliverable.

**Budget guideline.** Sustained usage above **~€25 / student / month** should
be flagged in office hours so the instructor can adjust.

### Submission workflow

- Each student works on a dedicated branch named
  `student/<NN>-<short-name>` off the classroom repository.
- Every milestone closes with a pull request to `main` containing
  exactly that milestone's deliverables.
- The grader leaves milestone-scoped review comments on the PR; the
  student addresses them in follow-up commits before the next
  milestone opens.
- Late submissions follow the course's standard late policy (see the
  syllabus).

### Milestone 1 — Agent Specification & First Prompt (Weeks 1–2) — *covers phase P1*

#### Activities — Weeks 1–2

- Instructor delivers: the proposal, the RICTOC skeleton, the shared
  finding schema, the Orchestrator reference prompt, three sample scenarios.
- Each student:
  - Selects a function from the catalogue ([`proposal.md`](./proposal.md) § 7).
  - Writes a **1-page Agent Specification**: function, inputs, expected
    outputs, MITRE ATT&CK coverage, false-positive strategy.
  - Produces the **v1 RICTOC prompt** for their agent.
  - Drafts **at least 3 test scenarios** (positive / negative / ambiguous).

#### Deliverables (end of Week 2)

- Signed-off Agent Specification (`spec.md`).
- `.aisoc/agents/<NN>-<short-name>.agent.md` containing the RICTOC v1 prompt
  (replaces the stub shipped by the instructor).
- 3 personal test scenarios under `tests/<NN>-scenarios/` (additive to the
  shared `.aisoc/scenarios/` reference set).

**Assessment weight:** 20%

### Milestone 2 — Cross-Environment Testing & Hardening (Weeks 3–4) — *covers phase P2*

#### Activities — Weeks 3–4

- Run each test scenario through the agent in **both** Copilot Chat **and**
  Claude Code; capture the raw outputs.
- Iterate the prompt until outputs are:
  - Schema-compliant in both environments.
  - Materially consistent across environments (severities and core findings
    must not diverge).
  - Robust to obvious adversarial inputs (prompt injection in pasted logs,
    misleading field names, missing fields).
- Add an **agent-internal self-check** step (the agent re-reads its output
  against the constraints before returning).

#### Deliverables (end of Week 4)

- `.aisoc/agents/<NN>-<short-name>.agent.md` v2 (hardened prompt).
- `tests/<NN>-…/` folder with paired Copilot / Claude Code transcripts per
  scenario.
- Short evaluation note (≤ 2 pages): consistency observations, weaknesses,
  prompt-injection resistance.
- The per-agent Pass checks from [`test-worksheet.md`](./test-worksheet.md)
  marked Pass / Fail for both environments.

**Assessment weight:** 40%

### Milestone 3 — Farm Integration & Final Exercise (Weeks 5–6) — *covers phase P3*

#### Activities — Weeks 5–6

- Wire the agent into the Orchestrator: confirm that the Orchestrator can
  invoke the agent within Plan-and-Approve and that the output is parseable
  by at least **two peer agents**.
- Participate in a live **instructor-run scenario walk-through** in Week 6:
  a multi-stage attack narrative (initial access → C2 → lateral movement →
  exfil) is fed to the Orchestrator, which plans, gets approval, calls
  agents, and produces the final report.
- Each student presents (≤ 10 minutes) and defends their agent's design,
  trade-offs, and cross-environment behaviour.

#### Deliverables (end of Week 6)

- Final `.aisoc/agents/<NN>-<short-name>.agent.md` (v3) committed to the
  shared repository of prompts.
- Final 4–6 page report (`report.md`) covering: design, RICTOC walk-through,
  test results, integration evidence, ATT&CK mapping, ethics & limitations.
- Live demo + Q&A.

**Assessment weight:** 40%

---

## 3-Week / Single-Milestone Plan

This variant compresses the project into an intensive sprint suitable for a
block course, a summer module, or a late-semester capstone. It collapses the
three milestones into one **single milestone with three weekly checkpoints**.
The full project weight (100%) sits on this milestone; weekly checkpoints exist
for formative feedback and to make it impossible to leave everything to Week 3.

**Onboarding window.** Because the sprint is only three weeks, onboarding moves
to a **Day 0 pre-session** in the week *before* Week 1 starts (typically the
Friday before kickoff). The five-step bring-up in [`proposal.md`](./proposal.md)
§ 3.5 is the **Day 0 readiness deliverable**; students who fail to submit it by
Monday morning of Week 1 forfeit their first checkpoint.

**Budget guideline.** Sustained usage above **~€15 / student over the
three-week sprint** should be flagged in office hours so the instructor can
adjust.

### Submission workflow

- Each student works on a dedicated branch named
  `student/<NN>-<short-name>` off the classroom repository.
- The student opens a **single pull request** to `main` at the start of
  Week 1 and keeps it as a draft until Week 3.
- At each weekly checkpoint the student tags the PR with the matching
  label (`week-1-checkpoint`, `week-2-checkpoint`, `final`) and requests
  review. The grader leaves checkpoint-scoped comments; the student
  addresses them in follow-up commits on the same PR.
- The PR is merged after the Week 3 oral defense.
- Late submissions follow the course's standard late policy (see the
  syllabus).

### Week 1 — Specification & First Prompt — *covers phase P1*

#### Activities — Week 1

- Instructor delivers: the proposal, the RICTOC skeleton, the shared
  finding schema, the Orchestrator reference prompt, three sample scenarios.
- Each student:
  - Selects a function from the catalogue ([`proposal.md`](./proposal.md) § 7)
    — allocation closes end of day 2.
  - Writes a **1-page Agent Specification**: function, inputs, expected
    outputs, MITRE ATT&CK coverage, false-positive strategy.
  - Produces the **v1 RICTOC prompt** for their agent.
  - Drafts **at least 2 test scenarios** (one positive, one negative;
    an ambiguous third is encouraged but not graded).

#### Week 1 checkpoint (end of Week 1)

- Signed-off Agent Specification (`spec.md`).
- `.aisoc/agents/<NN>-<short-name>.agent.md` containing the RICTOC v1
  prompt (replaces the stub shipped by the instructor).
- 2 personal test scenarios under `tests/<NN>-scenarios/` (additive to
  the shared `.aisoc/scenarios/` reference set).
- One run of the v1 prompt against the worked-example scenario, in
  **at least one** of the two target environments, with the transcript
  committed to the PR.

> **Checkpoint gate.** A student who has not committed both `spec.md`
> and a runnable v1 prompt by end of Week 1 enters Week 2 on a
> remediation track — they get a one-day extension and a 5%-of-final
> penalty. There is no second remediation; failure of the Week 2
> checkpoint after remediation drops the student to a partial-credit
> path (see [Partial-credit path](#partial-credit-path)).

### Week 2 — Cross-Environment Testing & Hardening — *covers phase P2*

#### Activities — Week 2

- Run each test scenario through the agent in **both** Copilot Chat
  **and** Claude Code; capture the raw outputs.
- Iterate the prompt until outputs are:
  - Schema-compliant in both environments.
  - Materially consistent across environments (severities and core
    findings must not diverge — see [`proposal.md`](./proposal.md) § 3.3).
  - Robust to a **single, instructor-provided adversarial input**
    (prompt injection embedded in pasted logs). The injection sample
    is published at the start of Week 2 so every student tests the
    same one; idiosyncratic adversarial inputs are out of scope for
    this variant.
- Add an **agent-internal self-check** step (the agent re-reads its
  output against the Constraints section before returning).

#### Week 2 checkpoint (end of Week 2)

- `.aisoc/agents/<NN>-<short-name>.agent.md` v2 (hardened prompt).
- `tests/<NN>-…/` folder with paired Copilot / Claude Code transcripts
  per scenario (at least two scenarios, four transcripts total).
- A **1-page evaluation note** (not 2): consistency observations,
  weaknesses, prompt-injection result.
- The per-agent Pass checks from [`test-worksheet.md`](./test-worksheet.md)
  marked Pass / Fail for both environments.

### Week 3 — Farm Integration & Defense — *covers phase P3*

#### Activities — Week 3

- Wire the agent into the Orchestrator: confirm that the Orchestrator
  can invoke the agent within Plan-and-Approve and that the output is
  parseable by at least **one peer agent** (down from two in the
  6-week version — paired up by the instructor on Monday of Week 3).
- Participate in a single **instructor-run integration session** in
  the second half of Week 3: one of the three reference scenarios is
  fed to the Orchestrator, which plans, gets approval, calls agents,
  and produces the final report. Students are present and respond
  when their agent is dispatched.
- Each student gives a **short oral defense (≤ 5 minutes)** of their
  agent's design, trade-offs, and cross-environment behaviour, plus
  a 3-minute Q&A. No slides required; the PR diff is the slide deck.

#### Final deliverables (end of Week 3)

- Final `.aisoc/agents/<NN>-<short-name>.agent.md` (v3) committed to
  the shared repository of prompts via the merged PR.
- A **2–3 page report** (`report.md`) covering: design,
  RICTOC walk-through, test results, peer-pairing evidence, ATT&CK
  mapping, ethics & limitations. (Down from 4–6 pages in the 6-week
  variant.)
- Oral defense + Q&A attended.

### Assessment weight

All of the evaluation dimensions in [`proposal.md`](./proposal.md) § 6 are
graded **once, at the end of Week 3 — 100% of the project total**. The weekly
checkpoints are formative, not summative; they exist to catch problems early,
not to distribute the grade.

### Partial-credit path

A student who fails the Week 2 checkpoint after remediation may
complete the project on a **single-environment partial-credit path**:
all deliverables are still required, but cross-environment portability
is graded as zero. Maximum achievable grade on this path is 70%. This
exists to keep struggling students engaged through Week 3 rather than
walking away; it must be requested explicitly and approved by the
instructor.

### Why three weeks is enough (and where it strains)

**Sufficient because:** the worked example, the schema, the catalogue,
and the test worksheet are all instructor-provided; the student does
not design the contract, only the agent body. RICTOC and the
8-key schema collapse most decision-making into local choices about
heuristics and severity rules.

**Strains visible to expect:**

- Less time for the prompt to "settle" between iterations — students
  may submit a v3 that still has rough edges.
- Cross-environment debugging consumes Week 2 disproportionately if a
  student picked an agent whose function maps poorly to one of the
  IDEs (rare, but watch out for #19).
- The oral defense rewards verbal fluency more than the 6-week version
  does; budget extra time in office hours for students who write well
  but present poorly.

---

## Adding a new variant

To add another timeline (e.g. a 4-week intensive, a semester-long track, or a
6-week block course), copy the skeleton below as a new `## N-…` section, add a
matching column to the [Variant comparison](#variant-comparison) table, and
fill in the blanks. **Do not touch [`proposal.md`](./proposal.md) or
[`test-worksheet.md`](./test-worksheet.md)** — they are timeline-neutral by
design, and any variant that needs to change them is changing the agent
contract, not the schedule.

```markdown
## <N>-… Plan

<One-paragraph framing: who this fits, how the grade is structured.>

**Onboarding window.** <When the § 3.5 bring-up runs.>
**Budget guideline.** <Per-student budget for this timeline.>

### Submission workflow
<Branch + PR conventions for this timeline.>

### <Period 1> — Specification & First Prompt — *covers phase P1*
- Activities …
- Checkpoint / deliverables …

### <Period 2> — Cross-Environment Testing & Hardening — *covers phase P2*
- Activities …
- Checkpoint / deliverables …

### <Period 3> — Farm Integration & Defense — *covers phase P3*
- Activities …
- Final deliverables …

### Assessment weight
<Per-phase split of the project total. Dimension weights stay as in proposal § 6.>

### What this variant trims vs. the 6-week baseline
<List reductions: scenario count, peer-parseability, report length, etc.>
```

Each variant **must** cover all three phases (P1–P3) and **must not** change
the per-agent pass bar in [`test-worksheet.md`](./test-worksheet.md) — only
*when* it is graded and *how much* of the total it carries.
