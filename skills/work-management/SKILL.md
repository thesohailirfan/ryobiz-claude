---
name: work-management
description: Use when the user mentions projects, tasks, work items, issues, sprints or cycles, milestones, moving a task to done, commenting or mentioning someone, approvals, decisions, risks, or project health in Ryobiz.
---

# Work management

## Conventions

- Writes can trigger notifications, automations and webhooks that can't be recalled.
  Retrying a create can make duplicates. Don't retry blindly. Check first.
- Updates may need the item's current version. Read it fresh before writing.
- Writes may need in-app approval. If so, ask the user to approve it in Ryobiz, then
  repeat the same call with the same arguments plus `approvalId`.

## Start every time

`list_workspaces`. If there are several, ask which one. Pass `workspaceId` to every call.

## Reading

- **Projects:** `list_projects`. **Health:** `get_project_health` returns progress,
  overdue items, risks and blockers, each traceable to its source. Lead with the verdict.
- **Work items:** `list_work_items` filtered by project, assignee, status or due date.
  Group by status or owner and give due dates as DD MMM YYYY.

## Creating and updating

- **Create:** collect title, project, type, assignee, priority, due date and a short
  description. Show it back, then `create_work_item` on "yes".
- **Edit fields:** `update_work_item`. Show what changes (old → new).
- **Move through workflow** (e.g. To do → In progress → Done): `transition_work_item`.
- **Bulk change:** `bulk_update_work_items`. Show the count and a sample of affected
  items, and wait for "yes". Report per-item conflicts afterwards.
- **Comment / mention:** `create_work_item_comment`. Mentions notify people, so show
  the text first.

## Cycles and milestones

- **New cycle (sprint):** name, start and end dates, project, then `create_cycle`.
- **End of cycle:** list unfinished items, confirm the target cycle, then `rollover_cycle`.
- **Milestone:** name, date, project, then `create_milestone`.

## Approvals, decisions and risks

- **Request approval:** subject, approver(s), what's being approved, then `create_approval_request`.
- **Approve, reject or submit:** only as the designated approver or owner. Show the
  request and the chosen outcome, and get "yes". Then `transition_approval_request`.
- **Log a decision:** `create_decision`. **Log a risk:** `create_risk` (impact,
  likelihood, owner, mitigation).

## After every write

Confirm the item key or title and its new status ("RYO-214 moved to Done"), then offer
one next step.

## Guardrails

- Confirm before any bulk change, rollover, approval decision, or anything that removes
  or closes work.
- If a call is denied, say so plainly. Never try another tool to get around it.
- Never paste raw JSON.
