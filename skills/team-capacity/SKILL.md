---
name: team-capacity
description: Use when the user asks who is overloaded, who has bandwidth, who should take a task, how loaded a person or team is, or wants to rebalance, reassign or delegate work across their team in Ryobiz.
---

# Team capacity and assignment

Load figures are estimates. Work without estimates is reported as "unknown", and you
should say so rather than treat it as zero.

## Start every time

`list_workspaces`. If there are several, ask which one. Pass `workspaceId` to every call.

## Reading load (read-only)

- **Organisation overview:** `get_capacity`. Total load, and how much work is unestimated.
- **A team:** `get_team_capacity`. **One person:** `get_user_capacity`.
- **Who's overloaded:** `get_overloaded_users`. Lead with names and how far over they are.
- **Who's free:** `find_available_people`, optionally filtered by skill or team.

Present people as a short table (name, load vs capacity, items, unknown estimates) when
comparing several people.

## "Who should take this?"

1. `get_assignment_recommendations` for the item(s). Give the top candidates with the
   reason for each, and any exclusions (on leave, not a member, over capacity).
2. Let the user choose, or accept the top recommendation.

## Reassigning (always preview first)

1. **Preview:**
   - Moving specific items to someone: `preview_delegation`.
   - Spreading an overloaded person's work: `preview_rebalancing`.
   Show each item, from → to, and the resulting load for everyone involved, plus any warnings.
2. **Confirm.** State the count of items and who gains or loses. If the preview had
   warnings, the user must explicitly accept them. Wait for "yes".
3. **Apply:** `bulk_assign_work_items` with the item IDs, a reason, and the expected
   versions from the preview. Items are applied one by one. If a conflict stops part
   way, report which ones moved and which didn't.
4. Confirm: "Moved 3 items from Anita to Rahul. Anita is now at 85%, Rahul at 70%."
   Note that ownership changes reset earlier workflow approvals on those items.

## Guardrails

- Never reassign without a preview and a clear "yes".
- If a call is denied (e.g. not a manager of that person), say so plainly. Never try
  another tool to get around it.
- Never paste raw JSON.
