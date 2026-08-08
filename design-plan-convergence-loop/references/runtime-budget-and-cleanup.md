# Runtime Budget and Cleanup

Load before estimating, persisting, pausing, resuming, or cleaning a convergence
run. Time budgets predict work; coverage determines completeness.

## Complexity and soft budget

Score each dimension `0/1/2`:

| Dimension | 0 | 1 | 2 |
|---|---|---|---|
| planning artifacts | 1-2 | 3-5 | more than 5 |
| DIRECT/DERIVED roots + unique Scenario classes + plan tasks | up to 25 | 26-75 | more than 75 |
| independent systems/environments | 1 | 2-3 | 4 or more |
| admitted assurance slices | 0 | 1 | 2 or more |
| external/runtime evidence | none | static external facts | dynamic or remote facts |

Use `S=0-2`, `M=3-5`, `L=6-7`, `XL=8-10`. Initial per-full-audit soft
budgets are `S 15-30`, `M 30-60`, `L 60-120`, and `XL 120-240` minutes.
Calibrate them from observed runs. Estimate the loop again after the initial
finding inventory; never use elapsed time as a READY criterion.

At the soft budget, finish the current lens, risk partition, evidence chain, or
repair batch; identify over-depth and re-estimate. If a host/session boundary
forces a stop, persist the checkpoint and return
`AUDIT INCOMPLETE — RESUME REQUIRED`. Never interrupt an atomic partition or
grant a verdict with mandatory coverage incomplete.

## Blind discovery and certification budgets

The final blind discovery audit uses profile deadlines independent of the complexity
estimate above:

| Profile | Soft deadline | Hard deadline |
|---|---:|---:|
| LEAN | 20 minutes | 120 minutes |
| AUTO | 30 minutes | 120 minutes |
| ASSURANCE | 120 minutes | 240 minutes |

Poll the auditor every 10 minutes. A poll expiry never stops the auditor. At
the soft deadline, surface `INCOMPLETE` progress and known findings first,
atomically persist the latest completed partition, and continue. At the hard
deadline, stop even if the
current partition is incomplete, preserve the last completed partition, and
restart that partition from its beginning on resume. This hard-deadline rule is
the sole exception to the atomic-partition rule above.

An explicit auditor error permits one replacement on the unchanged snapshot.
A second error or the hard deadline returns
`AUDIT INCOMPLETE — RESUME REQUIRED`; do not start another auditor in the same
invocation and do not grant READY from coordinator review or structural
validation alone.

Repeat whole-artifact certification uses the current complexity estimate, not
a fresh blind-discovery deadline. Time is still not completion evidence. At a
forced session boundary, persist completed certification partitions and resume
on the matching snapshot; do not reopen blind discovery for a root-preserving
repair.

## Coverage ledger

Freeze the mandatory partitions and their contract, shared-resource, and global
invariant obligations for the selected gate. Audit partitions use
`NOT_STARTED`, `IN_PROGRESS`, `DONE`, `ROUTED`, or `UNVERIFIED`; certification
obligations use `NOT_CHECKED`, `PASS`, `FAIL`, `ROUTED`, or `INVALIDATED`.
Typical partitions are charter/scope, contract coverage, dependency/phase
ordering, compatibility chains, admitted assurance slices, bidirectional
traceability, fact/command feasibility, and rollback or fail-closed ownership.

Completeness means every mandatory partition is `DONE` or validly `ROUTED`, not
that the run consumed its estimate. Stop an over-deep branch with the
sufficiency and failure-path rules while continuing other partitions.

## Project-local runtime state

Use only when remediation, multiple agents, user-decision pause, interruption,
or context rollover is likely. Default root:

```text
<project>/.design-plan-convergence-runtime/<run-id>/
  .run-marker.json
  frozen-run.json
  active-ledger.json
  final-audit-checkpoint.json
```

Keep the root out of Git. Do not silently edit a tracked ignore file without
authority. If persistence spans a handoff and the root is not already ignored,
use an authorized repository-local exclusion or disclose the temporary
untracked state.

`frozen-run.json` contains run/repository identity, mode, gate, profile,
charter, Non-Goals, frozen decisions, Acceptance Kernel, artifact generation,
DIRECT roots, approved boundary semantics, applicable defect classes,
initial artifact hashes, loaded rule/profile hashes, path boundaries, complexity
estimate, and mandatory partitions. Freeze it after initialization. Do not
store mutable write authorization, concern acceptance, or newly resolved
decisions in this file.

`active-ledger.json` contains coverage and certification status, DERIVED proofs,
Scenario equivalence keys, active finding identities and closure contracts,
accepted/routed items, current chain IDs with source IDs, anchors, invariants,
owners, oracles and evidence snapshots, current artifact snapshot, mutation
hashes, discovery lineage, current L1 write authorizations and risk acceptances
with their source/revision, trend, phase, and resume point. Store no raw
artifacts, credentials, long reports, superseded findings, or modifier
narratives. The coordinator is the only writer; replace the file atomically.

`final-audit-checkpoint.json` is written only during final blind discovery and
only through `.final-audit-checkpoint.json.tmp`. It records the final-audit
lineage, current snapshot and minimal L1-view digests,
completed/current/pending partitions, its own findings, evidence status,
attempt, and update time. It contains no active ledger or earlier audit and
repair history. While the auditor runs, only it writes the checkpoint; after
the attempt stops, the coordinator may atomically change only its status to
`INCOMPLETE`.

Persisted chain entries stabilize handoffs, not truth. Preserve authority
levels: user decisions are L1, current verified evidence L2, auditor findings
L3, and modifier claims L4. Recheck claims on the current snapshot.

The final blind discovery auditor receives `frozen-run.json`, raw current artifacts,
and a minimal source-bound L1 view of current user decisions and concern
acceptances, not `active-ledger.json`. A resumed or replacement final auditor
also receives the validated `final-audit-checkpoint.json` from the same lineage.
Generate the L1 view in context; do not persist another runtime file or include
earlier finding identities, counts, or repair history. Reconcile the blind
verdict with the ledger only after freezing the verdict on the unchanged
snapshot.

## Resume and cleanup

Resume an incomplete blind discovery checkpoint only when run ID, repository,
boundary, generation, profile, gate, loaded rule/profile hashes, snapshot
digest, and minimal L1-view digest still match. Otherwise discard only that
checkpoint. Start a new generation only when the Acceptance Kernel, boundary,
rule/profile, or key mechanism changed; otherwise restart final blind discovery
on the current snapshot in the same generation.

After discovery is complete, resume certification from the active ledger when
run, repository, generation, kernel, and boundary identities match and the
artifacts match its latest snapshot. A coordinator-recorded root-preserving
mutation advances that snapshot and invalidates certification but preserves
discovery lineage. An unexpected mutation stops resume for boundary review.

A new write authorization, concern acceptance, or justified DERIVED contract
that leaves the Acceptance Kernel, boundary, key mechanisms, and evidence
burden unchanged updates only the active ledger and does not repeat discovery.
A decision or repair that changes a frozen kernel field starts a new generation
and a new initial full audit.

Preserve runtime state for `DECISION REQUIRED`, `AUDIT INCOMPLETE`, or an
unexpected interruption. On any terminal verdict, or when the user cancels,
delete the current run state. Do not retain chain baselines or final
attestations unless explicitly requested.

Before deletion, resolve and verify that the target is a strict descendant of
the fixed runtime root, is not the root/project/user/drive directory, matches
the current run ID, contains a matching marker and repository identity, and is
not a symlink, junction, or reparse point. Delete only the known state files
and fixed atomic temporary filenames, then remove the directory only when
empty. Do not use globs, unresolved variables, or recursive deletion. On any
failed check, preserve the state and report the exact path.
