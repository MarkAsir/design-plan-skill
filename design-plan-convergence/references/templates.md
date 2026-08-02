# Design and Plan Audit Templates

Use only the sections required by the task. Delete unused fields instead of inventing content.

## Contents

- Audit boundary
- Design baseline attestation
- Plan target set
- Evidence ledger
- Finding ledger
- Watch and next-stage records
- Final report

## Audit boundary

```markdown
Target gate and mode:
Risk profile / assurance slices:
Charter and observable result:
Scope / Non-Goals:
Must-not-change behavior:
Primary artifacts:
Editable artifacts:
Read-only evidence and runtime surfaces:
Acceptance environment:
Completion criteria:
Snapshot ID / boundary version:
Unresolved decisions:
Accepted concerns:
```

## Design baseline attestation

```markdown
Verdict: DESIGN READY
Upstream lifecycle/design-set identity:
Required/optional artifact profile:
Artifact paths, presence, and hashes:
Applicable rule/profile hashes:
Audit boundary:
Critical repository fact baseline:
Accepted concerns (acceptor, authority, snapshot):
Watch items:
Next-stage notes:
Auditor identity and independence:
Issued at:
```

## Plan target set

```markdown
Mode: PLAN-ONLY
Target gate: PLAN READY
Plan path set:
Generation ID:
Supersedes:
Editable plan set:
Ordered upstream design set:
Required/optional upstream artifacts:
Plan-to-design ownership:
Read-only evidence set:
Multi-plan responsibilities/dependencies:
PlanAuditKey:
```

## Evidence ledger

| ID | Claim | Current/Target/Assumption | STATIC/PLANNED/EXECUTED/UNVERIFIED | Evidence/command | Environment/result | Remaining verification |
|---|---|---|---|---|---|---|
| E-01 |  |  |  |  |  |  |

A command written in a plan is PLANNED, not EXECUTED.

## Finding ledger

| ID | Status/severity | Source snapshot | Gate/contract | Evidence/root cause | Impact chain | Frozen boundary | Correction/owner | Closure evidence |
|---|---|---|---|---|---|---|---|---|
| F-01 | BLOCKER |  |  |  |  |  |  |  |

Use stable identity from the gate, violated contract, and impact chain. A new numeric ID does not hide a reopened root cause.

## Watch record

```markdown
Status: WATCH
Risk/evidence:
Why current gate still holds:
Containment/detection:
Owner or responsible workflow:
Trigger:
Review/expiry:
Action after trigger:
Source snapshot:
```

## Next-stage record

```markdown
Status: NEXT-STAGE NOTE
Source gate:
Target gate:
Owner:
Upstream contract:
Source snapshot:
Activation rule:
```

## Final report

```markdown
# Verdict: <GATE> READY | <GATE> NOT READY | DECISION REQUIRED

Snapshot / PlanAuditKey:

## Blockers and required findings
## Concerns and acceptances
## Watch and next-stage items
## Decisions required
## Independently verified closures
## Gate-appropriate evidence
## Remediation contracts
## Next audit boundary
```
