# Generic Design Remediation Profile

Load when authorized design artifacts do not follow OpenSpec or another known
schema. Preserve their established structure and terminology.

Normalize sections to charter, behavior contract, decision record, work
ownership, and acceptance responsibility. One file may own several roles. Edit
the authoritative section first and propagate only affected references; do not
split documents or introduce OpenSpec filenames, IDs, delta sections, or
validators merely to satisfy the skill.

Prefer existing identifiers and headings. Run-local IDs such as `GEN-REQ-001`
belong only to the temporary ledger unless the user authorizes durable IDs.
Repair missing content required by the current gate, not missing framework
ceremony. Use the project's validator when one exists; absence of
`openspec validate` is not a defect for a generic design.
