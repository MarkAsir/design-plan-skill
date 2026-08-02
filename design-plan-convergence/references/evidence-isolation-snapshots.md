# Evidence Isolation and Snapshot Contract

Load for re-audit, independent closure, external facts, or multi-cycle work.

## Authority

| Level | Content | Treatment |
|---|---|---|
| L1 | user decisions, frozen scope, authorized acceptance | fact |
| L2 | current-snapshot evidence verified by the auditor | fact |
| L3 | auditor finding tied to a snapshot | diagnosis to recheck |
| L4 | modifier report or candidate closure | claim only |

Repetition or summarization never promotes L3 or L4.

## Blind audit and reconciliation

An audit that can grant closure receives current raw artifacts, applicable
rules, frozen charter, gate, risk profile, snapshot, and a minimal source-bound
L1 view of current user decisions and concern acceptances. Do not seed it with
finding identities, historical counts, old reports, modifier prose, expected
answers, or candidate verdicts.

Freeze the blind result, then reconcile prior finding identities on the same
unchanged snapshot. If the snapshot changes, discard the verdict.

When isolated agent context exists, use it for the initial and final audits,
not for every lens. If it does not exist, remove history from the audit input
and disclose that independence was approximated rather than guaranteed.

An initial full audit completes its frozen coverage inventory before handing
findings to remediation. Do not stop after the first discovered batch. Record
each required partition as `NOT_STARTED`, `IN_PROGRESS`, `DONE`, `ROUTED`, or
`UNVERIFIED` so elapsed time cannot masquerade as completeness.

A new `BLOCKER` or `REQUIRED` discovered after the initial audit must name the
violated frozen contract, reachable counterexample, current-gate ownership,
minimum correction, whether the repair introduced it, and why the initial or
impact boundary did not expose it. Severity still follows current evidence:
never downgrade a proved current-gate problem because the auditor cannot explain
the earlier miss. If the contract, counterexample, or gate ownership is not
proved, route the claim normally. If those are proved but the missed boundary
cannot be bounded, invalidate audit saturation and reopen the affected mandatory
partition or the full audit before remediation.

## Active state versus history

Keep only:

```text
mode, gate, risk profile, charter, current snapshot
frozen decisions and unresolved decisions
active BLOCKER and REQUIRED findings with closure contracts
independently closed finding identities
accepted concerns
active watch, next-stage, and out-of-scope routes
last mutation and audit snapshots
```

Archive old reports, superseded snapshots, rejected approaches, and modifier
explanations outside the default context. Do not create a repository ledger
unless authorized. During compaction, retain current raw-state identities, not
the modifier's narrative.

## Snapshot

Record sorted normalized boundary paths, PRESENT/MISSING state, byte SHA-256,
applicable rule/profile hashes, external revision identity when used, boundary
version, target gate, and artifact generation.

Git HEAD alone is insufficient because dirty, untracked, missing, non-Git, rule,
or profile changes may alter the review. Bind every finding, repair handoff, and
verdict to its source snapshot.

## Audit frequency

- One initial full-boundary audit.
- If it is clean and unchanged, it is final.
- After mutation, exact diff plus complete affected decision chain, siblings, removed semantics, and new mechanisms.
- One blind final full-boundary audit after the last mutation.

Upgrade an intermediate review to full after boundary, charter, Non-Goal, gate,
risk profile, rule, plan generation, unexpected path, key mechanism,
high-severity root cause, or external revision changes.

Repeating an audit on an unchanged snapshot without new evidence adds no
assurance.
