# Remediation Templates

Use only the fields needed for the repair.

## Intake

```markdown
Target gate:
Risk profile / assurance slices:
Input snapshot or PlanAuditKey:
Editable artifact allowlist:
Read-only evidence set:
Charter, scope, and Non-Goals:
Frozen decisions and accepted risks:
Approved findings and closure contracts:
Unresolved decisions:
Completion criteria:
```

## Finding admission

| Finding | Snapshot current | Evidence holds | In gate | In scope | Authorized | Decided | Lifecycle | Result |
|---|---|---|---|---|---|---|---|---|
|  | yes/no | yes/no | yes/no | yes/no | yes/no | yes/no | blocker/required/concern/watch/next/out |  |

No-mutation results must have an empty diff.

## PLAN-ONLY identity

```markdown
DesignBaselineAttestation:
  upstream artifacts and hashes:
  approved gate:
  approval evidence:

PlanTargetSet:
  plans:
  lifecycle state:
  upstream mapping:
  cross-plan ordering:

PlanAuditKey:
  plan generation:
  upstream snapshot:
  applicable rule snapshot:
  target gate:
```

## Root-cause repair batch

| Batch | Admitted findings | Root cause | Invariant | Editable paths | Sibling evidence | Removed semantics | New mechanism |
|---|---|---|---|---|---|---|---|
| R-01 |  |  |  |  |  |  | none |

## Impact chain

| Layer | Authoritative location | Required change | Must remain unchanged | Verification |
|---|---|---|---|---|
| Requirement/scenario |  |  |  |  |
| Decision |  |  |  |  |
| Task/interface/command |  |  |  |  |
| Test/acceptance |  |  |  |  |
| Rollout/recovery |  |  |  |  |
| Risk/mapping |  |  |  |  |

## Mechanism evidence

| Claim | Level | Source or exact command | Environment | Expected | Actual | Result |
|---|---|---|---|---|---|---|
|  | STATIC/PLANNED/EXECUTED/UNVERIFIED |  |  |  |  |  |

A command copied into a plan is `PLANNED`, not `EXECUTED`.

## Candidate handoff

```markdown
# Result: CANDIDATE CLOSED | OPEN | DECISION REQUIRED | REJECTED | UPSTREAM DESIGN REOPEN REQUIRED

Target gate and input/output snapshot:
Admission result for every finding:
Root cause and edited paths:
Preserved decisions and removed semantics:
New mechanisms and risks:
Executed versus planned evidence:
Residual WATCH and NEXT-STAGE items:
Independent re-audit boundary:
```
