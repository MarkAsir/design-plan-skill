# Implementation Plan Audit Profile

Apply when the primary artifact is an implementation plan, the user requests
PLAN READY, or the path/header identifies a plan.

## Plan-only boundary

Audit an existing plan against a frozen approved design. Record:

```text
plan path set and generation ID
editable plan set: none during audit
ordered upstream design path/hash set and snapshot
plan-to-design ownership
applicable plan-format rules
read-only repository fact boundary
PlanAuditKey
```

Default to one plan for one design. Do not infer a multi-plan set. Plan creation
or regeneration belongs to the repository plan-writing workflow. A missing plan
at DESIGN is `NEXT STAGE NOT STARTED`; at PLAN it is
`PLAN NOT READY: GENERATION REQUIRED`, without repeated waiting.

## Design baseline

Prefer an unchanged trustworthy DESIGN READY attestation. If absent, perform one
read-only upstream design audit when practical. If current facts expose an
upstream contradiction or missing decision, return
`UPSTREAM DESIGN REOPEN REQUIRED`; do not edit the plan to hide it.

An unchanged attestation avoids re-auditing the entire design, but current
paths, symbols, dependencies, interfaces, tools, and commands referenced by the
plan still require validation.

Use:

```text
PlanAuditKey =
  upstream design snapshot
  + plan generation snapshot
  + applicable rule/profile snapshot
  + repository fact baseline
  + external fact revision when used
```

A regenerated plan, changed upstream decision, selected path-set change, or material rule
change invalidates the key and starts a new full audit generation.

## Five plan lenses

### Dependency

- Every output exists before use.
- Environment, permission, owner, generation, migration, publication, and deployment order is explicit where relevant.
- A phase relies only on current or earlier outputs.
- Intermediate states build/import and remain contract-compatible where the design requires staged delivery.

### Granularity

A task should have one primary behavioral objective, one coherent change chain,
an explicit stable result, and one verification oracle.

Split when any is true:

- two independently committable or revertible behavioral outputs;
- different owners, permissions, or environments;
- separate expand/switch/contract or release phases;
- the first part yields a stable state and the rest can occur later;
- independent failure or recovery boundaries.

Merge when a fragment has no independent observable result, oracle, safe
commit/revert boundary, or meaning without its neighbor. Split by behavior,
compatibility phase, or failure boundary, not by files, functions, or lines.

#### Commit-state coherence

Every declared commit or handoff boundary must leave affected implementation,
tests, contracts, and executable owners mutually consistent, and the affected
surface buildable or importable and minimally verifiable. An incoherent boundary
is `REQUIRED` unless it independently meets the parent `BLOCKER` definition.

- A TDD red state may exist inside an uncommitted task, but must not be declared
  as a commit or handoff boundary.
- Move an implementation owner, executable entry point, and affected contract
  tests atomically unless an approved staged compatibility boundary keeps every
  intermediate state valid.
- When work is staged across tasks without commit, identify the staged scope,
  why it remains uncommitted, and the task that completes the atomic commit.
- Run the smallest relevant gate at each boundary; require the full suite only
  when governing rules or affected risk require it.

### Risk

- Destructive actions, compatibility boundaries, trust boundaries, and production activation have prerequisites and safe recovery/fail-closed ownership.
- AUTO assurance applies only to high-risk slices.
- Hypothetical compound failures and new subsystems are not plan requirements without an upstream decision.

### Coverage

- Every approved requirement maps to implementation responsibility and a verification layer.
- Every task, file, interface, and check maps back to an approved requirement.
- The plan identifies the critical E2E journey, environment, owner, and oracle where one exists.
- Detailed test source and exhaustive case matrices remain development work.
- Map critical scenarios individually. Equivalent supporting scenarios may map
  as a family only when they share the same owner, implementation path, and
  oracle; enumerate exceptions separately.

### Feasibility

- Modify targets and required symbols exist now or are produced earlier.
- Create targets have coherent parent/module boundaries.
- Commands identify cwd, target, prerequisites, expected result, and failure interpretation.
- Tool and shell semantics are credible on the target environment.
- Future implementation outcomes stay PLANNED.

## Superpowers plan adaptation

Select this adapter only from an authoritative signal: the user names
Superpowers, the file is under a repository-designated Superpowers plan path,
or the document carries explicit Superpowers-specific metadata or execution
handoff required by project rules. An `Implementation Plan` heading and
checkbox tasks alone are insufficient because generic plans commonly use them.

Preserve the repository's required plan header, execution handoff, exact file
map, consistent interfaces, ordered tasks, commands, and commit boundaries.
Apply project conventions over generic examples.

Do not require every task to contain a large source listing. Require the minimum
detail a competent executor needs: behavioral objective, exact location,
interface or stable anchor, dependencies, approved constraints, smallest
relevant check, and oracle. A task may leave local algorithm, helper shape,
fixture body, and ordinary error branches to development when that discretion
cannot change an approved contract.

Require exact commands or code only when the governing format requires them,
ambiguity would change behavior, or ordering/tool semantics are themselves a
safety or compatibility control. Do not turn PLAN READY into an implementation
harness, exhaustive negative-fixture library, or release runbook. If
verification prose or code grows without adding approved contract coverage,
recheck the finding admission and detail boundary before asking for more.

Use red/green sequencing for behavioral code when the project workflow requires
TDD. Documentation, mechanical configuration, or generated-artifact tasks need
only the smallest appropriate validation. A red command must fail for the
intended missing behavior, not a missing tool, path, syntax, or unrelated import.

Ban unresolved placeholders that transfer a design decision to the executor.
Do not treat concise references to an authoritative contract as placeholders.

## Generic plan adaptation

Use when no known plan format applies. Normalize headings, tables, checklists,
or prose into ordered behavioral tasks without requiring a Superpowers header,
code-block cadence, TDD wording, or per-task commit boundary unless project
rules require them.

Require each task to identify its behavioral result, stable location or owner,
dependencies, approved constraints, smallest relevant verification, and oracle.
A plain design approved by the user or project workflow is a valid upstream
baseline when its exact path set and hashes are frozen. If it lacks stable IDs,
use deterministic run-local IDs for traceability and do not write them back
without authorization.

This is a fallback minimum, not an override. Apply stricter organization,
methodology, project, or artifact-format rules when present, including required
positive/negative verification, phase or commit boundaries, and exact command
fields.

## PLAN READY closure

Require a stable upstream design baseline and PlanAuditKey, bidirectional
traceability, credible present-day facts and commands, `BLOCKER = 0`,
`REQUIRED = 0`, no upstream reopen, routed residuals, and the audit-frequency
contract from the parent skill.
