# Orchestration and State

## Roles

Use the minimum topology that preserves separation:

```text
coordinator
|- read-only discovery auditor/certifier
`- authorized modifier
```

When isolated agents or contexts are available, use fresh context for each
open discovery audit. A read-only certifier may be reused for repeat
certification because certification reads the active coverage state and is not
blind. Never let the modifier certify its own repair. Do not create one agent
per lens or nested supervisors.

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
Acceptance Kernel and artifact generation
DIRECT roots, DERIVED proofs, and Scenario equivalence keys
coverage obligations and global invariant status
active BLOCKER and REQUIRED findings with closure contracts
independently closed identities
accepted concerns and routed watch/next-stage/out-of-scope items
discovery lineage, certification state, and last mutation/audit snapshots
```

For a persisted run, the coordinator is the only writer. Store frozen boundary
state separately from the mutable ledger. Auditors and modifiers return claims
to the coordinator rather than editing shared state directly. Key decision
chains may be cached in the mutable ledger only for this run.

The final blind discovery auditor is the sole exception: it may atomically replace only
`final-audit-checkpoint.json` through the fixed temporary file
`.final-audit-checkpoint.json.tmp`. It remains read-only to its allowed
final-audit inputs and has no access to the active ledger. The coordinator
validates but never rewrites this file while that auditor is running. After the
attempt stops, the coordinator may atomically change only its status to
`INCOMPLETE`.

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

Any mutation invalidates readiness and certification on the old snapshot. Mark
the affected decision-chain obligations and all global invariants
`INVALIDATED`. A root-preserving mutation does not erase completed blind
discovery for the current generation. A change to the Acceptance Kernel,
artifact boundary, applicable rules/profile, or a key mechanism starts a new
generation and requires new discovery.

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
           OR no obligation or broken edge closes
           OR same root recurs after canonicalization
           OR repair adds an unapproved or independently deliverable
              capability/control plane/state/protocol
```

Compare active finding identities as well as counts; a closure never offsets a
repair-introduced finding. Track failed/invalidated obligations, broken trace
edges, duplicate Requirement/Scenario keys, and semantic growth beside counts.
New contracts, tasks, phases, mechanisms, or executable examples must pass the
growth admission gate. Otherwise classify the trend as apparent convergence
and consolidate or replace the local patches.

After a mixed result, reconstruct the affected root before another mutation.
Continue only while each cycle closes a known obligation or contradiction.
Same-root recurrence requires canonicalization of the root and its shared
semantic resources before another repair. Split/reset, reopen upstream, or
return `DECISION REQUIRED` only when that reconstruction reveals the
corresponding boundary condition.

## Audit cadence

1. One initial full discovery audit.
2. If clean and unchanged, finish.
3. After mutation, exact-diff and affected-chain audit.
4. One final blind discovery audit for the current generation.
5. After blind findings and every later mutation, repeat whole-artifact
   certification until it passes on an unchanged snapshot.

Upgrade an intermediate review to full after a gate, boundary, rule, risk
profile, generation, unexpected path, key mechanism, or high-severity root
change. Never repeat a full audit on an unchanged snapshot without new evidence.

Do not stop a whole audit merely because one branch appears over-deep. Route or
stop that branch under the gate-risk contract and finish every mandatory
coverage partition. A forced session boundary returns a resumable incomplete
state, never READY.

## Final blind discovery lifecycle

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
Seal the discovery checkpoint as lineage evidence. A later artifact mutation
prevents using its verdict as readiness evidence but does not reopen discovery
for the same generation. A change to the Acceptance Kernel, boundary,
rule/profile, or key mechanism starts a new generation and requires a new
discovery audit. A minimal L1-view revision that only records authorized concern
acceptance or write authority and leaves the kernel and evidence burden
unchanged updates the active ledger without reopening discovery.

## Whole-artifact certification lifecycle

Store certification state in `active-ledger.json`; do not add another state
file. The certifier reads the current artifacts, Acceptance Kernel, active
findings, coverage obligations, and validator evidence. It checks every global
invariant across the full artifact set and deeply rechecks invalidated
obligations and affected decision chains.

Resume interrupted certification from the active ledger's latest artifact
snapshot when run, repository, generation, kernel, and boundary identities
match. Do not apply the blind checkpoint's snapshot or minimal L1-view digest
to certification resume.

Bind `PASS`, `FAIL_KNOWN_OBLIGATION`, or `COVERAGE_ESCAPE` to the current
snapshot. A repair invalidates only that certification and affected coverage.
For an escape, record the reachable counterexample, DIRECT roots, and shared
semantic resources; canonicalize the affected contracts and add only a missing
obligation from the frozen applicable defect classes. Change generation only
when re-saturation changes the kernel or proves its frozen applicability wrong.
Complete re-saturation only after all affected obligations are `PASS`, `FAIL`,
or `ROUTED`, duplicate keys are removed, and the canonical set digest is stored.

## Durable goal

Use a host goal only when explicitly requested and supported. Keep it scoped to
the current artifact mode and gate. Design-only completion does not wait for a
future plan; plan-only completion does not silently reopen design.

Resume from raw artifacts and a recomputed snapshot, not an old modifier report.
Do not set a token budget unless the user requests one.
