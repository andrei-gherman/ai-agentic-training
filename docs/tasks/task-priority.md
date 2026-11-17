# Task priority — implementation task list

**Status:** Approved  
**Specification:** `docs/specs/task-priority.md`  
**Requirements:** `docs/requirements/task-priority.md`  
**Approval owner:** AG  
**Approved by:** AG on 2026-11-17 (source of authority: explicit approval record from AG)

These are approved tasks under the AG-approved requirements and specification. Open decisions remain tracked for implementation confirmation.

| Task ID | Reviewable result | Acceptance criteria | Planned verification evidence | Dependencies / gate | Status |
|---|---|---|---|---|---|
| TP-1 | Task creation accepts only an integer priority from 1 through 5, with 5 most critical; missing and invalid values are rejected. | AC-1 | Planned focused API tests for priorities 1 and 5, missing priority, non-integer input, and values below 1 or above 5. Assert the rejection status after the validation-status decision is confirmed. | None | Approved |
| TP-2 | A successful task creation response includes the submitted priority alongside existing task fields and retains the `201` status. | AC-2 | Planned focused API test asserting the create response status, priority, and existing task fields. | None | Approved |
| TP-3 | Task list results include each task's priority, retain current ordering, and support optional `completed` and `priority` filters. Either filter works alone; when both are supplied, results match both. | AC-3 | Planned focused API tests for priority-only and `completed`-only filtering, combined intersection across matching and non-matching task combinations, and stable list order. | Filter parameter and exact-match interpretation are inferred; confirm before implementation. | Approved |
| TP-4 | After creation, a task's priority can be changed and the new value is returned and visible in a subsequent list result without reordering. | AC-4 | Conditional planned API tests for a valid change, subsequent list state and order, invalid priority, and unknown task ID. | Proceed only after the proposed update method, path, request shape, and unknown-ID behavior are approved. | Approved |
| TP-5 | Superseded: the proposed priority/completed filter-combinability work is now included in TP-3 under approved AC-3. | P-AC-1 (superseded by AC-3 on 2026-11-17) | Verification is covered by TP-3. | None | Approved |

## Traceability check

| Acceptance criterion | Task IDs | Verification evidence | Coverage |
|---|---|---|---|
| AC-1 | TP-1 | Planned focused API validation tests; rejection status awaits confirmation. | Covered; validation status unresolved. |
| AC-2 | TP-2 | Planned focused create-response API test. | Covered. |
| AC-3 | TP-3 | Planned focused list API test for priority, order, and `completed` filtering. | Covered. |
| AC-4 | TP-4 | Conditional planned update API tests; contract must first be approved. | Conditionally covered; blocked on an open decision. |

No acceptance criteria are omitted from the task mapping. No unrelated tasks are included.

## Scope changes

Keep this aligned with the scope-change record in `docs/specs/task-priority.md`.

- **SC-1 — Approved by the requester (2026-11-17):** completed and priority filters must be combinable. This updates AC-3 / S-3; TP-3 now covers priority-only, completed-only, combined intersection, and order-preservation verification. P-AC-1 / TP-5 is superseded by AC-3 / TP-3.

The scope change is approved under AG authority, and the requirements, specification, and task list are approved as a coordinated package.
