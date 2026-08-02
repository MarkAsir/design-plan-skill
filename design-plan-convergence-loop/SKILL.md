---
name: design-plan-convergence-loop
description: Run a bounded audit-remediation-audit loop for an authorized design set or implementation plan, whether produced as plain documents, OpenSpec artifacts, Superpowers plans, or equivalent formats. Use when one invocation should select DESIGN READY or PLAN READY, normalize artifact roles, apply risk-proportionate review, repair only confirmed current-gate findings, detect convergence, and stop on readiness, routing, or a real decision instead of reviewing until no conceivable issue exists.
---

# Design and Plan Convergence Loop

## Contract

Drive one artifact generation to one document readiness gate. Treat OpenSpec
and Superpowers as optional format adapters, not prerequisites:

- use `design-plan-convergence` for audit and independent closure;
- use `design-plan-remediation` for every authorized mutation;
- never let the modifier close its own findings or grant READY;
- never modify implementation, external systems, frozen decisions, or paths outside the allowlist;
- an audit-only request runs one audit and stops;
- default completion is `DESIGN READY` or `PLAN READY`, not implementation or release.

Read [references/modes-and-gates.md](references/modes-and-gates.md) to select the
artifact lifecycle and [references/orchestration-and-state.md](references/orchestration-and-state.md)
to manage snapshots, trend, context, and termination.
Read [references/runtime-budget-and-cleanup.md](references/runtime-budget-and-cleanup.md)
before estimating work, persisting a multi-cycle run, pausing, resuming, or
cleaning runtime state.

## Freeze the run

Record:

```text
mode, target gate, and LEAN/AUTO/ASSURANCE profile
artifact generation and editable path set
read-only evidence boundary
charter, scope, Non-Goals, must-not-change behavior
frozen decisions, accepted risks, unresolved decisions
applicable rules and artifact profile
completion criteria and current snapshot
complexity class, soft time budget, and mandatory coverage partitions
```

Gate priority:

1. explicit user-selected gate;
2. gate implied by the current artifact lifecycle;
3. `DECISION REQUIRED` when the difference changes scope or evidence burden.

Use `AUTO` by default: LEAN globally, local assurance only for high-risk slices.
A missing later artifact is not an earlier-gate blocker.

Elapsed time is a scheduling signal, never completion evidence. A clean
single-pass audit may keep state in context. Persist the minimum project-local
runtime state before remediation, multi-agent handoff, user-decision pause, or
any run likely to cross a context boundary.

## Run the bounded cycle

### 1. Initial full audit

Run one full-boundary read-only audit on current raw artifacts and the frozen
run state. Complete the mandatory lens and risk-partition inventory before the
first remediation handoff. Do not seed it with historical reports, expected
findings, modifier claims, or old counts.

If `BLOCKER = 0`, `REQUIRED = 0`, the snapshot is unchanged, concerns are
accepted, and watch/next-stage/out-of-scope items are correctly routed, this
audit is final. Return READY without a duplicate audit.

### 2. Admit and repair

Send only active `BLOCKER` and `REQUIRED` findings to remediation. The modifier
must recheck admission and edit root-cause batches, not blindly apply suggested
wording. Any mutation invalidates earlier readiness.

Route without mutation:

| Condition | Result |
|---|---|
| user decision, acceptance, or write authority missing | `DECISION REQUIRED` |
| environment unavailable after one safe check | mark evidence `UNVERIFIED`; keep `REQUIRED` when current-gate proof is missing, otherwise route `NEXT-STAGE NOTE` |
| low-probability contained observation | `WATCH` |
| work belongs to development, integration, verification, or release | `NEXT-STAGE NOTE` |
| separately deliverable work is outside the charter | `OUT OF SCOPE` |
| plan finding invalidates frozen design | `UPSTREAM DESIGN REOPEN REQUIRED` |

Do not rerun an unchanged blocker. Resume only after a decision, environment
change, artifact mutation, or new evidence.

If remediation disproves a finding without mutation, the auditor reconciles
that identity on the unchanged initial snapshot. When this leaves `B = 0` and
`R = 0`, the initial full audit plus reconciliation is sufficient; do not add a
duplicate final audit.

### 3. Intermediate impact audit and trend

After repair, audit the exact diff plus the complete affected decision chain,
siblings, removed semantics, phase invariants, and new mechanisms. Do not run a
full repository review on every cycle.

Let `B` be active current-gate BLOCKER count and `R` be REQUIRED count:

- **strong convergence**: `B1 <= B0`, `R1 <= R0`, at least one strictly
  decreases, no repair-introduced BLOCKER/REQUIRED identity, no unauthorized
  boundary/mechanism change, and no same-root recurrence;
- **converged**: `B1 = 0` and `R1 = 0`;
- **mixed**: one count decreases while the other increases, or old findings
  close while new current-gate identities appear; allow one root-cause
  correction cycle;
- **stalled**: artifacts changed but both counts remain unchanged;
- **divergent**: BLOCKER rises, REQUIRED rises in two consecutive cycles, the
  same root survives two cycles, or repair adds an unapproved or independently
  deliverable capability/control plane/state/protocol.

Also compare semantic complexity: tasks, phases, mechanisms, executable
examples, and verification burden. Falling counts with disproportionate growth
that adds no approved contract coverage is apparent, not strong, convergence;
recheck finding admission and the current-gate detail ceiling.

Compare active finding identities as well as counts; closures never offset
repair-introduced findings. Counts are diagnostic, not a reason to downgrade
severity. There is no fixed “two repairs maximum.” Continue while strong
convergence is demonstrated.
After repeated mixed, stalled, or divergent results, stop point patches:
reconstruct the charter and decision chain, then return `DECISION REQUIRED`,
reset the approach, split the change, or reopen upstream design.

Upgrade an intermediate audit to full only when the gate, boundary, charter,
rule, profile, generation, key mechanism, or high-severity root cause changes.

At the soft time budget, finish the current atomic partition, check for
over-depth, and re-estimate. If the session must stop while mandatory partitions
remain, save a validated checkpoint and return
`AUDIT INCOMPLETE — RESUME REQUIRED`; never manufacture READY or discard the
unfinished partitions. Resume only on a matching snapshot.

### 4. Final blind audit

After any mutation and convergence to `B = 0`, `R = 0`, run one blind
full-boundary audit on the stable final snapshot, then reconcile active finding
identities on that same snapshot. Do not repeat it without mutation or new
evidence.

Use fresh independent audit context for the initial and final audits when the
runtime supports it. A modifier should be a separate role. Do not create one
agent per lens. If isolated context is unavailable, clear historical reports
from the audit input, disclose the limitation, and continue without inventing
an independence guarantee.

Treat an auditor wait timeout as a polling event, not an audit failure. Poll a
running final auditor every 10 minutes and apply the profile deadlines in
[references/runtime-budget-and-cleanup.md](references/runtime-budget-and-cleanup.md).
At the soft deadline, persist a validated partition checkpoint and surface
`INCOMPLETE` progress and known findings first; continue until completion or
the hard deadline. At the hard deadline, preserve the latest valid checkpoint and return
`AUDIT INCOMPLETE — RESUME REQUIRED`; never terminate on a polling timeout or
substitute coordinator review for the unfinished blind audit.

If the auditor explicitly errors, replace it at most once on the unchanged
snapshot and resume from the latest valid checkpoint. A replacement reads only
the final-audit checkpoint, frozen state, current raw artifacts, and permitted
L1 facts. An agent timeout does not mean isolated context is unavailable. See
[references/orchestration-and-state.md](references/orchestration-and-state.md)
for checkpoint ownership, validation, and resume rules.

The final blind auditor may read frozen run state, current raw artifacts, and a
minimal source-bound L1 view of current user decisions and concern acceptances,
but not the active ledger, prior chain assessments, findings, or modifier
reports. The L1 view contains no finding identities or repair history. Reconcile
identities only after freezing its verdict on the unchanged snapshot.

## Completion

Return READY only when all hold on one unchanged snapshot:

1. `BLOCKER = 0` and `REQUIRED = 0`;
2. no unresolved decision, upstream reopen, or current-gate environment blocker remains;
3. every prior blocking finding is closed, rejected, or validly routed;
4. required current evidence is reproducible and later evidence is labeled `PLANNED`;
5. concerns are accepted, and watch/next-stage/out-of-scope items have a traceable disposition;
6. no unapproved scope change or repair-created subsystem remains;
7. when artifacts changed, the final blind audit and cross-partition synthesis
   completed on this snapshot.

Report only the active state: mode, gate, risk profile, final snapshot, modified
paths, closures, accepted residuals, routed items, evidence status, and next
stage. Do not replay historical repair reports.

On a terminal verdict, safely remove the current run's project-local runtime
directory. Preserve it only for an explicit pause such as `DECISION REQUIRED`
or `AUDIT INCOMPLETE`, and remove it when the user cancels. Do not retain chain
baselines or attestations after completion unless the user separately requests
an audit artifact.

If a durable goal was explicitly opened, mark it complete only at this point.
