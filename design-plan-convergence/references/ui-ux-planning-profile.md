# UI/UX Planning Audit Profile

Load only when the user, governing rules, or primary artifacts make
user-visible UI/UX behavior part of the frozen scope. Frontend technology,
component names, or UI-related keywords alone are not an authoritative signal.
For an implementation plan, also load `implementation-plan-profiles.md` and
treat its upstream design as frozen.

## Responsibility boundary

Audit whether in-scope UI/UX behavior is sufficiently defined for the selected
document gate. Do not produce or score visual design, generate or compare
mockups, prescribe an implementation structure, or execute visual QA.

Existing design systems, prototypes, screenshots, component catalogs, and code
may be read-only evidence when their authority and snapshot are established.
They do not silently add requirements or replace an approved contract.

## DESIGN READY lens

Inspect only the user-visible behavior inside the frozen charter:

1. **Critical journey**: the intended user action and observable result are
   sufficient to write a plan. When a credible in-scope rejection or recovery
   condition changes approved behavior or the acceptance oracle, the design
   also defines its contracted response.
2. **Relevant states**: loading, empty, error, success, partial, permission, or
   interrupted states are specified only when they change in-scope behavior,
   compatibility, ownership, or the acceptance oracle. Do not require a full
   state matrix by default.
3. **Decision sufficiency**: an unresolved UI/UX choice is current-gate work
   only when two reasonable choices could produce different in-scope behavior
   or prevent a stable oracle. Leave local details to a competent executor.
4. **Governing constraints**: apply existing design-system, accessibility,
   responsive, input-method, or internationalization constraints only when
   governing organization, platform, or project rules, or frozen decisions,
   make them applicable.
5. **Ownership and acceptance**: identify the behavior owner, affected
   consumers, verification responsibility, and later-stage acceptance owner.

Admit a finding only through the parent sufficiency and classification rules.
Missing specificity alone is not evidence that DESIGN READY fails.

## PLAN READY lens

Audit the existing plan against its frozen UI/UX design baseline:

- map each approved UI/UX contract to an implementation responsibility, stable
  location or owner, dependencies, appropriate verification layer, and oracle;
- validate existing design-system or component facts only when the plan relies
  on them; do not require a separate inventory artifact;
- keep task and handoff boundaries compatible with approved user-visible
  behavior; and
- require a component structure, state-management choice, viewport or browser
  matrix, or specific test type only when upstream contracts or governing rules
  make that detail necessary.

If the plan exposes a missing upstream UI/UX decision, use the existing
`UPSTREAM DESIGN REOPEN REQUIRED` route. Do not rewrite frozen design through
plan detail.

## Detail ceiling and routing

Use only the parent finding statuses and routing rules. Do not add a UI-specific
score, decision taxonomy, manifest, or readiness gate.

Do not require by default:

- mockups, prototypes, visual comparisons, or aesthetic ratings;
- exhaustive state, viewport, browser, device, or test matrices;
- emotional-arc narratives, exact design tokens, or exact component trees;
- implementation, visual-regression execution, usability testing, or release
  evidence.

Route later-stage work only when an approved contract gives it an owner and
activation condition. If no current-gate defect remains, report no profile
finding and create no additional artifact.

Record the profile's applicability and authoritative trigger in the existing
frozen lens inventory. Do not create a separate scope or completion manifest.
