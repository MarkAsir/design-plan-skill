# Orchestration and State

## Roles

Use the minimum topology that preserves separation:

```text
coordinator
|- initial/final read-only auditor
`- authorized modifier
```

When isolated agents or contexts are available, use a fresh auditor for the
initial and final full audits. Reuse one auditor for intermediate impact checks
only if it did not mutate artifacts. Do not create one agent per lens or nested
supervisors.

If isolation is unavailable, reconstruct a blind input from raw current
artifacts, omit historical reports, disclose the limitation, and do not claim
stronger independence than exists.

## Evidence authority and context

| Level | Content | Authority |
|---|---|---|
| L1 | user decisions, frozen scope, authorized acceptance | fact |
| L2 | evidence verified on current snapshot | fact |
| L3 | auditor finding | diagnosis to recheck |
| L4 | modifier report/candidate closure | claim only |

Keep only the active ledger:

```text
mode, gate, risk profile, charter, snapshot
frozen and unresolved decisions
active BLOCKER and REQUIRED findings with closure contracts
independently closed identities
accepted concerns and routed watch/next-stage/out-of-scope items
last mutation and audit snapshots
```

For a persisted run, the coordinator is the only writer. Store frozen boundary
state separately from the mutable ledger. Auditors and modifiers return claims
to the coordinator rather than editing shared state directly. Key decision
chains may be cached in the mutable ledger only for this run.

The final blind auditor is the sole exception: it may atomically replace only
`final-audit-checkpoint.json` through the fixed temporary file
`.final-audit-checkpoint.json.tmp`. It remains read-only to artifacts, frozen
state, and the active ledger. The coordinator validates but never rewrites this
file while that auditor is running. After the attempt stops, the coordinator
may atomically change only its status to `INCOMPLETE`.

Archive historical reports, superseded snapshots, rejected approaches, and
modifier explanations outside the default context. Intermediate handoffs
contain the active ledger, exact diff, affected chain, and new mechanisms only.

## Snapshot and mutation control

Compute the snapshot from normalized paths, PRESENT/MISSING state, byte hashes,
loaded rule/profile hashes, external revision identities when used, mode, gate,
and artifact generation. Git HEAD alone is insufficient.

Before and after a modifier, independently inventory and hash editable paths and
existing changed paths. Derive the mutation diff; do not trust the modifier's
path report. Unexpected paths stop remediation and trigger boundary review.

## Convergence trend

Compare active current-gate counts and root identities:

```text
strong: B1 <= B0 AND R1 <= R0
        AND at least one strict decrease
        AND no repair-introduced BLOCKER/REQUIRED identity
        AND no boundary/mechanism drift
        AND no same-root recurrence

converged: B1 = 0 AND R1 = 0
mixed: one count decreases and the other increases
       OR old findings close while new current-gate identities appear
stalled: artifacts changed, B and R unchanged
divergent: B rises
           OR R rises in two consecutive cycles
           OR same root survives two cycles
           OR repair adds an unapproved or independently deliverable
              capability/control plane/state/protocol
```

Compare active finding identities as well as counts; a closure never offsets a
repair-introduced finding. Track semantic growth beside counts. New tasks,
phases, mechanisms, or large executable examples must trace to admitted
findings and approved contracts.
Otherwise classify the trend as apparent convergence and recheck the boundary.

Allow one root-cause correction after a mixed result. Continue beyond two
repairs only while strong convergence is proven. Repeated mixed, stalled, or
divergent results require reconstruction, split/reset, upstream reopen, or
`DECISION REQUIRED`; do not keep patching wording.

## Audit cadence

1. One initial full audit.
2. If clean and unchanged, finish.
3. After mutation, exact-diff and affected-chain audit.
4. One final blind full audit after the last mutation.

Upgrade an intermediate review to full after a gate, boundary, rule, risk
profile, generation, unexpected path, key mechanism, or high-severity root
change. Never repeat a full audit on an unchanged snapshot without new evidence.

Do not stop a whole audit merely because one branch appears over-deep. Route or
stop that branch under the gate-risk contract and finish every mandatory
coverage partition. A forced session boundary returns a resumable incomplete
state, never READY.

## Final blind audit lifecycle

Use the mandatory coverage partitions frozen for the run. One blind auditor
processes them sequentially; do not create one agent per partition. After each
partition, atomically persist a checkpoint with:

```text
schema_version, run_id, snapshot digest, L1 digest, gate, profile, status, attempt
completed partitions, current partition, pending partitions
current final-audit findings with source anchors, evidence status, updated_at
```

Use only `RUNNING`, `SOFT_LIMIT`, `INCOMPLETE`, or `COMPLETE` as checkpoint
status. The checkpoint belongs to this final-audit lineage and may contain its
own findings, but never the active ledger, earlier findings, modifier reports,
repair history, credentials, or raw artifacts.

A 10-minute wait expiry is status polling. If the agent remains running,
report progress and wait again. At the soft deadline, set `SOFT_LIMIT`, persist
the latest completed partition, and surface `INCOMPLETE` progress and known
findings before optional detail; this is not a final verdict.
At the hard deadline, stop the attempt, mark the current partition incomplete,
preserve the last valid checkpoint, and return
`AUDIT INCOMPLETE — RESUME REQUIRED`.

After an explicit auditor error, replace the auditor once and resume on the
same snapshot. A second error returns `AUDIT INCOMPLETE`. A hard deadline never
starts another auditor in the same invocation. The next invocation may resume
when run identity, gate, profile, boundaries, generation, snapshot digest, and
the minimal L1-view digest still match. Restart the current incomplete
partition from its beginning; a model's unreported reasoning is not resumable.

When all partitions are complete, run one cross-partition synthesis over the
current raw artifacts and checkpoint findings before freezing the verdict.
Any artifact mutation or L1-view revision invalidates the checkpoint and
requires a new final blind audit on the new inputs.

## Durable goal

Use a host goal only when explicitly requested and supported. Keep it scoped to
the current artifact mode and gate. Design-only completion does not wait for a
future plan; plan-only completion does not silently reopen design.

Resume from raw artifacts and a recomputed snapshot, not an old modifier report.
Do not set a token budget unless the user requests one.
