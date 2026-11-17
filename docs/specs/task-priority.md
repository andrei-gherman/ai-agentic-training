# Task priority — feature specification

**Status:** Approved  
**Requirements source:** [`docs/requirements/task-priority.md`](../requirements/task-priority.md)  
**Approval owner:** AG  
**Approved by:** AG on 2026-11-17 (source of authority: explicit approval record from AG)

## Outcome

Users can identify task urgency from priority information returned by the Task API. Priority can also be changed after task creation, subject to approval of the provisional update contract. [AC-1–AC-4]

## Scope and behavior

| Spec ID | Behavior or constraint | Source criterion |
|---|---|---|
| S-1 | Task creation requires an integer priority from 1 through 5, where 5 is most critical; missing or out-of-range values are rejected. The exact validation status remains unresolved. | AC-1 |
| S-2 | Successful creation returns `201` and includes priority with the existing task fields. | AC-2 |
| S-3 | Task listing includes priority, preserves current list order, and supports optional `completed` and `priority` filters. Either filter can be used alone; when both are supplied, returned tasks match both. The priority filter uses the `priority` query parameter with an integer from 1 through 5. | AC-3 |
| S-4 | Provisionally, priority can be changed through the proposed `PATCH /tasks/{task_id}` contract. A successful change returns the updated task and is reflected by a later list request without reordering; invalid priority is rejected and an unknown ID returns `404`. This behavior depends on approval of the proposed contract. | AC-4 |

## Constraints and exclusions

- Preserve existing task fields and the `completed` filter; do not reorder tasks by priority. [AC-2, AC-3]
- Keep the existing in-memory task lifetime; no database or persistence change is specified. [AC-2–AC-4]
- Current implementation has no priority field or task-update route; current tests do not cover priority or updates. These are proposed behaviors, not existing contract. [AC-1–AC-4]
- A separate user interface is not specified; whether API responses are sufficient remains open. [AC-1–AC-4]

## Verification intent

Planned focused API tests should verify each behavior below. Existing tests provide coverage patterns for creation, listing, title validation, and completion filtering; they do not yet provide evidence for this feature.

| Spec ID | Planned evidence | Evidence status |
|---|---|---|
| S-1 | Test valid priority values and rejection of missing, non-integer, and out-of-range priorities; assert the response status after that decision is resolved. | Planned |
| S-2 | Test that creation returns `201` and includes submitted priority with existing task fields. | Planned |
| S-3 | Test priority presence, priority-only and `completed`-only filtering, the combined filter's intersection, and unchanged list ordering. | Planned |
| S-4 | If the proposed update contract is approved, test valid update response and subsequent list state, invalid priority, unknown task ID, and order preservation. | Conditional |

## Assumptions and unresolved decisions

- The requirements and this specification are approved by AG; open decisions remain tracked for final confirmation but do not invalidate the approved scope.
- Decide whether to use the proposed `PATCH /tasks/{task_id}` method, path, request shape, and `404` behavior. [AC-4]
- Decide whether API visibility is sufficient or a UI is also in scope. [AC-1–AC-4]
- Confirm whether missing or invalid priority should return `422`; this is proposed from current validation behavior, not confirmed. [AC-1]
- Confirm the inferred `priority` query parameter and exact-match behavior before implementation. [AC-3]

## Scope change record

| ID | Change, source, and approval | Impact and planned verification |
|---|---|---|
| SC-1 | Requester/trainer instruction on 2026-11-17: completed and priority filters must be combinable. **Approved by the requester on 2026-11-17.** | Affects AC-3 / S-3. AC-3 now specifies the `priority` query parameter (integer 1–5), either filter alone, and intersection when both are supplied, preserving list order. Planned verification: test priority-only, completed-only, and combined requests against tasks spanning matching and non-matching combinations; assert the combined result is the intersection in the original order. Previous proposal P-AC-1 is superseded by AC-3. |

The scope change is approved under AG authority, and the requirements and specification are approved as a coordinated package. Other open decisions are listed above.
