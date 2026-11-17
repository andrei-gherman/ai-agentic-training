# Task priority — requirements worksheet

**Status:** Draft  
**Approval owner:** Unassigned  
**Last reviewed:** 2026-11-16 (draft; not approved)

## Request

> We need a way for the team to prioritize work. Add priority to tasks so users can see what is urgent. Keep it simple.
>
> Source: [Issue #4](https://github.com/andrei-gherman/ai-agentic-training/issues/4).

## User outcome

Let users identify task urgency through priority information in the Task API, and allow priority to change after task creation.

## Sources reviewed

| Source | What it establishes | Limits or notes |
|---|---|---|
| [Issue #4](https://github.com/andrei-gherman/ai-agentic-training/issues/4) | The request is to add simple task priority so users can identify urgent work. | It does not define values, defaults, endpoints, or update behavior. |
| [app/main.py](../../app/main.py) | Current `TaskCreate` and `Task` models; `GET /tasks` and `POST /tasks`; in-memory storage. | No priority field or update endpoint currently exists. |
| [tests/test_api.py](../../tests/test_api.py), [tests/conftest.py](../../tests/conftest.py) | Existing endpoint coverage and per-test in-memory reset. | No priority or task-update tests. |
| [README.md](../../README.md) | Documents the current API, in-memory lifetime, and absence of a user interface. | Describes current state, not whether a future UI is in scope. |
| Requester clarifications in this requirements review | Priority is an integer 1–5, 5 most critical; required on create; included in create and list responses; list order unchanged; priority may change after creation. | The update mechanism and some API edge behavior remain unspecified. |

## Confirmed facts

- Issue #4 asks for a simple way to prioritize tasks so users can see what is urgent.
- The current API has `title` and `completed` in `TaskCreate`; `Task` adds `id`. It has `GET /tasks` with an optional `completed` filter and `POST /tasks` returning `201`. Tasks are held in memory. Source: [app/main.py](../../app/main.py).
- Current tests cover health, an empty task list, the default `completed` value, title validation (`422`), and a created task appearing in the list. They do not cover priority or updates. Source: [tests/test_api.py](../../tests/test_api.py).
- The requester specified priority as an integer from 1 through 5, with 5 most critical; priority is required when creating a task; create and list responses include it; listing retains its current order; and priority can change after creation.

## Assumptions and inferences

| Statement | Evidence | Confirmation needed? |
|---|---|---|
| **Proposed update contract:** add `PATCH /tasks/{task_id}` accepting a body containing only `priority`; return the updated task with `200`; use `404` for an unknown task ID and `422` for invalid priority. | The requester authorized defining the update mechanism; current API has no update route. `422` follows the existing FastAPI/Pydantic validation pattern. | Yes. This is a proposal, not an existing API behavior or an approved requirement. |
| The request is API-scoped, with no separate UI change. | The requester specified create/list API responses; the README says this starter has no UI. | Yes; the README describes current state, not future scope. |

## Acceptance criteria

1. **Create validation:** `POST /tasks` requires `priority` as an integer from 1 through 5; 5 represents the most critical priority. Missing or out-of-range values are rejected. A `422` response is proposed, following the current validation pattern.  
   **Trace:** requester clarification; `TaskCreate` in [app/main.py](../../app/main.py); existing validation assertion in [tests/test_api.py](../../tests/test_api.py).

2. **Create response:** a successful `POST /tasks` returns `201` and includes the submitted `priority` in the created task, alongside the existing task fields.  
   **Trace:** requester clarification; `create_task` and `Task` in [app/main.py](../../app/main.py).

3. **List response and ordering:** `GET /tasks` includes `priority` for each task and retains the current list order; the existing `completed` filter continues to work.  
   **Trace:** requester clarification; `list_tasks` in [app/main.py](../../app/main.py).

4. **Change after creation — provisional:** using the proposed `PATCH /tasks/{task_id}` contract, a valid priority change returns the updated task, and a subsequent `GET /tasks` shows the new priority without changing the list order. Invalid priority is rejected; an unknown task ID returns `404`.  
   **Trace:** requester clarification that priority can change; proposed update contract above. This criterion depends on approval of that contract.

## Non-goals and constraints

- Do not reorder tasks by priority; the requester specified that listing keeps its current order.
- **Provisional scope boundary:** priority-based filtering is not included in these criteria pending the open question below.
- The current starter has no UI and stores tasks in memory. No UI, database, or persistence change is specified. Sources: [README.md](../../README.md), [app/main.py](../../app/main.py).
- Preserve existing task fields and the `completed` filter unless separately approved.

## Open questions

- Should the proposed `PATCH /tasks/{task_id}` method, path, request shape, and `404` behavior be used, or should the update contract differ?
- Should `GET /tasks` support filtering by priority? The requester specified that list order remains unchanged but did not address filtering.
- Is API response visibility sufficient, or is a separate user interface also in scope?
- Should missing or out-of-range priority use the existing `422` validation response? This is proposed from current behavior, not specified by the requester.

## Review record

- **Decision:** Draft; not approved.
- **Reviewed by:** Unassigned.
- **Notes:** Update this record only after the responsible person reviews the requirements and resolves the open decisions.
