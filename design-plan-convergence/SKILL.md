---
name: design-plan-convergence
description: Audit plain or structured technical design documents, OpenSpec artifacts, generic or Superpowers implementation plans, and repaired planning documents for the current readiness gate. Use for read-only design or plan review, artifact-role normalization, re-audit, semantic-drift and scope-creep checks, plan-only review against an approved design, or risk-proportionate closure without expanding into implementation or release verification.
---

# Design and Plan Convergence Audit

## Core contract

Audit one current readiness gate from current evidence. Keep the audit read-only.
Treat old reports and modifier summaries as claims, never as facts.

1. Read applicable organization, project, methodology, repository, and artifact-format rules.
2. Freeze the charter, gate, artifact set, Non-Goals, decisions, accepted risks, evidence boundary, and snapshot.
3. Use the smallest evidence set that proves or disproves the gate, including complete affected decision chains and siblings.
4. Do not edit artifacts, implement future work, run release verification, or broaden the charter.
5. Route only `BLOCKER` and `REQUIRED` findings to remediation.

Choose one audit mode:

- **Audit**: inspect the current artifact without relying on an earlier verdict.
- **Re-audit**: verify claimed repairs on the current snapshot.
- **Convergence**: detect recurrence, drift, and repair-created mechanisms.

## Select the gate and risk profile

Use the explicit gate, otherwise infer it from the primary artifact:

| Gate | Current responsibility |
|---|---|
| `DESIGN READY` | OpenSpec or equivalent design contracts, decisions, compatibility, risks, and recovery principles are sufficient to write a plan. |
| `PLAN READY` | An existing implementation plan is aligned, ordered, feasible, and mechanically verifiable. |
| `IMPLEMENTATION READY` | Route only when the user explicitly asks to verify completed implementation or invokes the repository implementation-verification workflow. |
| `RELEASE READY` | Route only on an explicit release-readiness request after implementation evidence and a release candidate or target environment exist. |

Do not automatically advance from document convergence to implementation or release.
A missing plan at DESIGN is `NEXT STAGE NOT STARTED`, not a blocker.

Use `AUTO` unless the user selects `LEAN` or `ASSURANCE`:

- `LEAN`: cover the main flow, approved boundaries, credible single failures, and mandatory safety/data protections.
- `AUTO`: apply LEAN globally and a local `ASSURANCE SLICE` only to security/trust boundaries, money/quota, irreversible data changes, public compatibility, concurrency/idempotency/state machines, or production activation/rollback.
- `ASSURANCE`: deepen evidence for the selected gate; never change the gate or authorize implementation.

If assurance would add a capability, controller, persistent state, protocol, artifact, live-environment execution, or Non-Goal change, return `DECISION REQUIRED`.
Read [references/gate-risk-watch-contract.md](references/gate-risk-watch-contract.md) for classification, risk slices, test admission, and stopping rules.

For an implementation plan or plan-only request, read
[references/implementation-plan-profiles.md](references/implementation-plan-profiles.md).
For design artifacts that are not OpenSpec, read
[references/generic-design-profile.md](references/generic-design-profile.md).
For independent closure, re-audit, or multi-cycle work, read
[references/evidence-isolation-snapshots.md](references/evidence-isolation-snapshots.md).

## Freeze the boundary

Record:

```text
gate, mode, and LEAN/AUTO/ASSURANCE profile
charter, observable result, scope, Non-Goals, must-not-change behavior
primary artifacts and artifact generation
editable artifacts: none
read-only rules, code, configuration, tests, runtime, and external evidence
acceptance environment and gate completion criteria
frozen decisions, unresolved decisions, accepted residual risks
snapshot and boundary version
```

“Full boundary” means the frozen charter, applicable rules, and complete impact
chains, not the whole repository. Verify current facts from code, configuration,
tests, CI, or runtime evidence; do not infer them from target documents.

## Apply the gate lenses

At `DESIGN READY`, audit:

1. **Premise/value**: problem evidence, observable result, completion criteria.
2. **Scope/boundary**: scope, Non-Goals, must-not-change behavior, no hidden product expansion.
3. **Architecture/contracts**: one owner per behavior, decision trade-offs, compatibility, affected consumers, intermediate states.
4. **Delivery/operations**: dependencies, implementation ownership, rollout, backup/rollback or fail-closed principle, accepted residual risk, and automation proportional to repeat risk.

At `PLAN READY`, audit the plan against frozen upstream design through:

1. **Dependency**: outputs exist before use; environments, owners, and permissions are ordered.
2. **Granularity**: tasks split by behavioral result, compatibility phase, or failure boundary, not file or line count.
3. **Risk**: critical sequencing, destructive actions, compatibility, rollback, and current-gate assurance slices are explicit.
4. **Coverage**: every approved contract maps to work and verification; every task maps back to an approved contract.
5. **Feasibility**: current paths, symbols, interfaces, tools, cwd, prerequisites, commands, and oracles are credible.

Do not import a global “cover every edge case” objective. Optional UI, DX,
security, or operations lenses apply only when the frozen scope contains them.
Automate checks that are repeated, high-risk, cross-service, or prone to false
positives; do not add a script merely to make a one-time low-risk review look
complete.

Complete the frozen lens and risk-partition inventory before issuing the first
repair handoff. Time spent is not coverage. Stop an over-deep branch by applying
the risk, sufficiency, and routing contracts; do not stop the whole audit while
mandatory partitions remain incomplete.

## Close decision chains

Trace each relevant path:

```text
evidence -> requirement/scenario -> decision/trade-off -> implementation owner
         -> valid and rejected behavior -> verification responsibility
         -> rollout/acceptance -> rollback/recovery principle
```

Preserve artifact ownership:

- proposal: problem, value, scope, Non-Goals;
- spec: observable behavior and acceptance boundary;
- design: decisions, trade-offs, flows, compatibility, risks;
- tasks: implementation and verification responsibility;
- plan: order, paths, interfaces, commands, oracles, phase boundaries.

These are semantic roles, not required filenames. A generic document may own
several roles; audit its content rather than forcing it into an OpenSpec shape.

Use stable Requirement, Scenario, Decision, and Task identifiers where the
artifact format supports them. A downstream artifact must not silently rewrite
an upstream decision.

## Keep tests at the correct stage

- OpenSpec defines testable behavior, critical journeys, acceptance scenarios, and verification ownership; it does not need an exhaustive test-case library.
- A plan maps contracts to unit, integration, E2E, or release responsibility, prerequisites, environment, fixture category, and oracle; it does not implement the tests.
- Development creates detailed cases and fixtures.
- Integration and release workflows execute full E2E, smoke, deployment, rollback, and recovery checks.

At DESIGN and PLAN, future outcomes remain `PLANNED`. Do not fail an earlier gate
because later tests have not run. Require only gate-appropriate evidence:

| Gate | Prove now | May remain planned |
|---|---|---|
| DESIGN | charter, facts, contracts, decisions, impact surface, compatibility, risk and recovery principles | exact implementation and test execution |
| PLAN | paths, interfaces, dependencies, ordering, present command feasibility, stable oracles | future red/green, integration, deployment, runtime results |
| IMPLEMENTATION | implementation and required regression evidence | release-only execution |
| RELEASE | deployment, runtime acceptance, observability, rollback and recovery | nothing release-critical |

Use static contract audit, gate-appropriate safe rehearsal, and reverse
traceability. At DESIGN/PLAN, rehearsal validates current facts and feasibility;
it does not execute future implementation. Select adapters from artifact
identity and governing rules, not installed CLI availability. Never install
validators automatically or run one that writes, uses the network, or changes
the environment without authorization. After one safe unavailable or
unauthorized check, mark the evidence `UNVERIFIED`.

For an exact mandatory validator or action, equivalent evidence cannot
substitute; retain `REQUIRED` until it succeeds on the current snapshot or the
requirement is waived or amended. For a mandatory property or evidence outcome,
equivalent current evidence may satisfy its parent contract; otherwise retain
`REQUIRED`. Optional-validator unavailability alone creates no finding; continue
the semantic audit. Missing execution authorization alone does not create
`DECISION REQUIRED`. Route future-gate execution as `NEXT-STAGE NOTE`.

## Manage findings

Use project statuses when stricter. Otherwise:

- `BLOCKER`: the main flow cannot work, contracts conflict, or reachable security/data/irreversible harm lacks a safe decision.
- `REQUIRED`: necessary for the current gate or a proportionate prevention of major quality failure.
- `CONCERN`: confirmed non-blocking residual risk requiring authorized acceptance.
- `WATCH`: low-probability trigger-based observation; do not remediate now.
- `NEXT-STAGE NOTE`: work owned by development, integration, implementation verification, or release.
- `OUT OF SCOPE`: real, separately deliverable work outside the charter.
- `REJECTED`: disproved claim.
- `CLOSED`: independently verified root-cause closure.

For each `BLOCKER` or `REQUIRED`, record the violated current-gate contract,
evidence, root cause, siblings, minimum correction invariant, authorized
boundary, and closure evidence. A modifier may return a remediation-defined candidate outcome, but never `CLOSED` or READY.

Read [references/templates.md](references/templates.md) only when a formal
ledger, manifest, attestation, or report is required.

## Audit frequency and verdict

- Perform one initial full-boundary audit.
- If it finds no `BLOCKER` or `REQUIRED` and the snapshot stays unchanged, it is also the final audit; do not repeat it.
- After mutation, inspect the exact diff plus the complete affected decision chain, siblings, removed semantics, and new mechanisms.
- Upgrade an intermediate review to full after a gate, boundary, rule, profile, generation, or key mechanism changes.
- After all repairs, perform one blind full audit on the stable final snapshot. Do not repeat without mutation or new evidence.

Declare READY only when:

1. current-gate `BLOCKER = 0` and `REQUIRED = 0`;
2. no unresolved decision or upstream reopen affects the gate;
3. current evidence is reproducible and future checks have stable planned oracles;
4. concerns are authorized and watch/next-stage items are correctly routed;
5. no unapproved scope change or repair-created subsystem remains; and
6. the audited snapshot is unchanged.

Lead with the terminal defined by the selected gate profile. Use `<GATE> READY`,
`<GATE> NOT READY`, or `DECISION REQUIRED` only when the profile does not define
a more specific routed terminal. Report blockers and required findings first,
then accepted residuals, routed items, verified closures, and the next action.
