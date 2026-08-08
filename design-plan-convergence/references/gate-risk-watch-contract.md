# Gate, Risk, Test, and Routing Contract

Load this reference when selecting review depth, classifying findings, admitting
test coverage, or deciding when to stop exploring failure paths.

## Risk profile

Use `AUTO` unless the user explicitly chooses:

- `LEAN`: approved main flow, contract boundaries, credible single failures, and mandatory security/data protections.
- `ASSURANCE`: deeper analysis for a high-consequence change, still limited to the current gate.
- `AUTO`: LEAN globally plus a local `ASSURANCE SLICE` for:
  - authentication, authorization, secrets, or trust boundaries;
  - money, billing, quota, or entitlement;
  - deletion, migration, schema change, or irreversible write;
  - public API, version, or compatibility contract;
  - concurrency, idempotency, distributed state, or key state machine;
  - production activation, publication, commit identity, rollback, or recovery.

A category match is only a screening signal. Admit an assurance slice only when
all hold: impact is high, one credible independent input/action/failure can
reach it, existing containment is insufficient, and the current artifact must
constrain the response. Multiple matches do not upgrade the whole change.
Select the smallest set of independent invariants that covers the admitted
slices; reuse one representative path for equivalent siblings.

Do not upgrade the whole change because one slice is high risk. A local slice
may deepen current-artifact analysis without approval. Require
`DECISION REQUIRED` if it adds a capability, controller, persistent state,
protocol, artifact, live-environment execution, material evidence cost, or
changes a Non-Goal. Explicit LEAN never removes mandatory trust-boundary,
security, data-loss, or irreversible-change safeguards.

## Sufficiency and executor discretion

At DESIGN or PLAN, classify a missing detail as `BLOCKER` or `REQUIRED` only
when at least one holds:

- it prevents ordering work, choosing an interface, or identifying an owner;
- two reasonable implementations could produce different approved behavior;
- it can violate a frozen contract or compatibility boundary;
- it controls security, data loss, irreversible action, or production activation;
- it prevents a stable mechanical acceptance oracle.

If a competent executor can decide the detail locally without changing an
approved contract, route it to development or the owning later stage. More
specific wording is not itself evidence of higher readiness.

## Contract and Scenario admission

Classify contract roots in audit state; do not require a new artifact syntax:

- `DIRECT` comes from user requirements, approved scope or decisions, external
  compatibility/regulatory contracts, or mandatory project rules. No new L1
  fact means no new DIRECT contract.
- `DERIVED` is logically necessary to implement or protect DIRECT roots. It may
  use intermediate proof steps, but record the ultimate DIRECT roots, derivation
  chain, reachable counterexample without it, observable invariant, owner,
  stable oracle, and duplication/subsumption check.

Do not create a Requirement for an example, internal code location, unchanged
service detail, repeated sibling wording, or one Scenario branch. Prefer an
existing Requirement or Scenario whenever it already owns the behavior.

Admit a new Scenario only for a previously uncovered equivalence key:

```text
DIRECT roots + precondition/state class + independent action/failure class
+ observable outcome/protected invariant + boundary semantics + oracle
```

Boundary semantics compare authority/trust, transaction or atomicity domain,
side-effect domain, and failure outcome rather than service names. Equivalent
sibling services share one representative Scenario plus an artifact-native
coverage matrix. If the format cannot express that validly, keep only the
minimum repeated Scenarios required by its validator.

## Classification

Ask:

1. Is the state reachable from a normal state by one independent input, action, ordering, or failure?
2. Does it violate an approved current-gate contract or core invariant?
3. Is the current gate responsible for choosing the response?
4. Is impact high even if probability is low?
5. Is it already observable, contained, fail-closed, or safely recoverable?

Route:

| Condition | Status |
|---|---|
| Main flow impossible, conflicting contract, or reachable security/data/irreversible harm without a safe decision | `BLOCKER` |
| Current-gate correction is necessary or proportionate to prevent major quality failure | `REQUIRED` |
| Confirmed non-blocking residual risk requiring acceptance | `CONCERN` |
| Low-probability contained observation with a trigger lifecycle | `WATCH` |
| Owned by a later readiness stage | `NEXT-STAGE NOTE` |
| Real but independently deliverable outside the charter | `OUT OF SCOPE` |
| Disproved by current evidence | `REJECTED` |

Do not route a missing architectural decision to a later stage if a safe plan
cannot be written without it.

## Failure stopping rule

Model now:

- the normal success path and approved rejection classes;
- first execution and repeat/retry when stateful;
- one credible failure at each trust, compatibility, data, or deployment boundary;
- one main failure-to-rollback or failure-to-fail-closed path when the contract requires it;
- realistic shared-state concurrency when it can violate the approved invariant.

Do not automatically model:

- several independent disasters occurring together;
- recovery-of-recovery chains;
- low-impact anomalies already detected and safely contained;
- implementation branches that do not change approved behavior;
- hypothetical failures whose solution needs a new subsystem.

Typical boundary:

```text
deploy fails -> rollback succeeds: include
deploy fails -> rollback fails -> fail closed: include for a high-risk slice
external backup is also corrupt: WATCH, manual drill, or separate resilience work
```

Stop when every approved contract and independent risk partition has at least
one defect-detecting check or an explicit later-stage owner.

Build finite coverage obligations from:

- each contract root and its applicable defect classes above;
- each explicit shared semantic resource and its protected invariant; and
- global terminology, traceability, scope, ordering, security, and data
  invariants.

A shared resource is an actor/authority, action, state variable, persistence
object, compatibility phase, output, transaction boundary, or rollback path
named or necessarily implied by the approved artifacts. Group all roots sharing
that resource in one obligation instead of creating pairwise Requirement
combinations. Do not add recovery-of-recovery or a global Cartesian product.

## Test case admission

Detailed test cases are not a DESIGN READY requirement. When evaluating whether
a case belongs in a plan or later test inventory, include it only when all five
conditions hold:

1. it maps to a DIRECT or DERIVED approved contract;
2. it represents an independent defect class;
3. it has a stable mechanical oracle;
4. it can be routed to the correct test stage; and
5. it verifies an explicit MUST/SHALL, is reachable from normal state by one
   independent input/action/failure, or has high failure impact.

- `DIRECT` and `DERIVED` use the contract-root definitions above.
- Independent defect: removing the case leaves no other case that detects the same defect.
- Stable oracle: exit code, response, state, data, log, or deployment fact.
- Stage: unit, integration, E2E, implementation verification, release, periodic, or isolated drill.

Use equivalence classes and pairwise combinations, not a Cartesian product.
For multiple independent safeguard failures, default to WATCH. In a
security/data assurance slice, test the final fail-closed invariant rather than
every failure combination. Expensive critical cases are routed to a later
environment; they are not silently dropped.

## WATCH lifecycle

Use WATCH only when the current gate holds, occurrence is low probability or
unproven, existing behavior detects and contains it, impact is not an
unmitigated security/data/irreversible failure, and fixing it now is
disproportionate.

Record risk/evidence, why the gate holds, containment/detection, owner,
trigger, review/expiry, action after trigger, and source snapshot. A WATCH is
not CLOSED. When triggered, reclassify it.

## CONCERN, NEXT STAGE, and OUT OF SCOPE

- CONCERN records risk, containment, acceptor/authority, accepted snapshot, and revisit trigger.
- NEXT-STAGE NOTE records source/target stage, owner, upstream contract, source snapshot, and activation rule.
- OUT OF SCOPE records why it is real, why it is independently deliverable, and its issue/change/owner disposition.

Do not expand the current change merely because a problem is reasonable.
