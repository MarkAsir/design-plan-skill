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

Treat disproportionate document growth as an admission warning. A repair that
adds tasks, phases, mechanisms, or large executable examples without new
approved contract coverage is not strong convergence; recheck scope and route
implementation-owned detail to its later stage.
