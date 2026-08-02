# Generic Design Audit Profile

Load when the design is not an OpenSpec artifact set or does not follow a known
schema. Normalize content for audit without rewriting its format.

## Role normalization

Map the available documents or sections to these semantic roles:

| Role | Typical content |
|---|---|
| charter | problem, value, scope, Non-Goals, observable outcome |
| behavior contract | externally observable requirements, rejection classes, acceptance boundary |
| decision record | architecture, owner, alternatives, trade-offs, compatibility |
| work ownership | components, phases, dependencies, implementation responsibility |
| acceptance responsibility | verification layer, environment, oracle, rollout/recovery owner |

One document may own several roles; several documents may share one role when
authority is clear. Do not require filenames, directory layout, document
splitting, OpenSpec IDs, delta semantics, or `openspec validate`.

## Contract units and identity

Treat a stable heading, numbered statement, acceptance criterion, interface,
state invariant, or explicit decision as a contract unit. Prefer author-provided
IDs. If absent, assign deterministic run-local IDs such as `GEN-REQ-001` and
`GEN-DEC-001` in the active ledger only; do not write them into artifacts
without authorization.

Missing format is not a defect. Missing content is a finding only when the
selected gate cannot be established from the complete normalized role set.
Do not demand that a generic design imitate OpenSpec when its current structure
unambiguously owns the required contracts.

## Baseline and readiness

Bind the normalized role map, artifact paths, section anchors, and byte hashes
to the design snapshot. Formal tool validation is optional; use the project's
own validator when one exists. DESIGN READY still requires the parent skill's
scope, contract, decision, compatibility, risk, recovery, and traceability
criteria.
