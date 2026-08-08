# Finding Admission Contract

Load before any edit.

## Admission formula

Admit only:

```text
finding exists on the current snapshot
AND status is BLOCKER or REQUIRED at the selected gate
AND inside the frozen charter and not a Non-Goal
AND every required write path is authorized
AND no unresolved user or product decision is made
AND lifecycle is active now
AND minimum correct repair does not create an independent subsystem
AND the missing detail cannot be left to a competent executor without changing
    approved behavior, safety, compatibility, ordering, ownership, or oracle
AND every new Requirement or Scenario passes the contract-growth gate
```

Record identity, source snapshot, violated contract, evidence, root cause,
siblings, gate, scope, authority, decision state, lifecycle, minimum invariant,
and closure evidence.

## Fixed routing

| Condition | Result | Mutation |
|---|---|---|
| stale, contradicted, disproved | `REJECTED` | none |
| already repaired | independent re-audit | none |
| confirmed non-blocking residual risk | `CONCERN` | none unless acceptance text is authorized and missing |
| low-probability contained observation | `WATCH` | none |
| later-stage work | `NEXT-STAGE NOTE` | none |
| independently deliverable outside charter | `OUT OF SCOPE` | none |
| missing authority, acceptance, or decision | `DECISION REQUIRED` | none |
| plan invalidates frozen design | `UPSTREAM DESIGN REOPEN REQUIRED` | none |
| all predicates pass | root-cause repair batch | allowlisted planning artifacts only |

A no-mutation route with a diff is an admission failure.

A new current-gate finding does not by itself require user intervention. Repair
violations of existing DIRECT/DERIVED contracts, repair-introduced regressions,
and missing constraints uniquely derivable from the frozen Acceptance Kernel.
Ask only when multiple product/architecture outcomes remain valid, scope or a
Non-Goal changes, an independent mechanism or material evidence burden is
needed, or write/acceptance authority is missing.

## Contract-growth gate

Before adding a Requirement, classify it in audit state:

- a new `DIRECT` contract requires a new source-bound L1 fact;
- a new `DERIVED` contract records its ultimate DIRECT roots, derivation chain,
  reachable counterexample, independent observable invariant, owner, stable
  oracle, and duplication/subsumption check.

Before adding a Scenario, record its equivalence key from contract roots,
precondition/state, independent action/failure, observable outcome/invariant,
boundary semantics, and oracle. Reject a duplicate key. Use one representative
Scenario and an artifact-native coverage matrix for equivalent siblings unless
the artifact validator requires minimal repetition.

For each repair batch, compare DIRECT/DERIVED counts, unique Scenario keys,
tasks, decisions, states, actors, mechanisms, and protocols before and after.
Reject net growth that neither closes a failed/invalidated obligation nor adds
one justified missing obligation. Consolidate or replace overlapping local
patches before candidate closure.

## Boundary checks

Stop before editing if:

- the source snapshot changed;
- current behavior or claimed evidence cannot be established;
- the fix must choose among materially different product or architecture outcomes;
- a guard's actor, authority, atomicity, or fail-closed behavior is undefined;
- recovery owner, eligibility, retention, clock, or expiry is required but undefined;
- the repair depends on an unproven controller, storage primitive, protocol, external capability, or live environment;
- detailed implementation or test code would be needed to make the planning document appear complete.

Do not hide missing facts behind “preserve current behavior,” “appropriate,” or
similar prose.

## Mechanism and scope budget

Explicitly revalidate scope before adding persistent state, controller,
authority, coordination protocol, mutable artifact, fallback/retry path,
deployment action, or recovery channel.

If it can ship independently, changes a Non-Goal, materially increases the
evidence surface, or creates a new failure domain, return `DECISION REQUIRED`
or split it into a separate change. Otherwise name the owner, activation order,
valid/rejected transitions, fail-closed behavior, and verification.

Treat disproportionate document growth as an admission failure, not a warning.
A repair that adds tasks, phases, mechanisms, or executable examples without
new approved contract coverage cannot reach candidate closure; simplify it and
route implementation-owned detail to its later stage.
