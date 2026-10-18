---
name: daily-brief
description: Use when the user asks "what's on my plate", "morning brief", "what's overdue", "what needs my attention", "what's pending today", or wants a daily summary across work, approvals, deliveries and rent dues in Ryobiz.
---

# Daily brief

A short, prioritised summary of what the user should act on today. It is read-only:
this skill never changes records.

## Workflow

1. **Workspace.** Call `list_workspaces`. If there is more than one, ask which one (or use
   the one named, or offer a brief for each). Pass `workspaceId` to every call.
2. **Collect.** Call these, skipping any whose feature is disabled or whose call is
   denied (do not retry a denial with another tool):
   - `get_my_day`: the user's priorities, waiting work, approvals, upcoming deadlines.
   - `get_attention_items`: company-level attention items (needs company read access).
   - `get_workspace_dashboard`: workspace-level counts and status.
   - `list_approvals`: approvals waiting on the user.
   - `list_blockers`: blocked work.
   - `get_shop_delivery_board`: today's trips (only if deliveries are enabled).
   - `get_rental_dashboard`: rent dues and collections (only if rentals are enabled).
3. **Compose** the brief in this exact order, including only sections with content:
   1. **Overdue and urgent**: overdue work items, urgent attention items, blockers.
   2. **Waiting on others**: items the user is waiting for, with who and since when.
   3. **Approvals**: requests awaiting the user's decision.
   4. **Today**: deliveries planned or in transit; rent dues and overdue rent.
   5. **Next 7 days**: milestones and due dates coming up.
4. **Close** by offering one sensible next step, such as "Want me to open the approval
   from Priya?" or "Shall I start this month's rent billing?"

## Writing the brief

- Lead with a one-line headline: "3 overdue items, 2 approvals waiting, ₹48,500 rent due."
- Use bullets, not tables, unless comparing several similar rows (e.g. 6 trips).
- For each item: what it is, who owns it, the due date (DD MMM YYYY), and why it matters.
- Convert amounts from paise to ₹ with Indian grouping (₹1,25,000).
- If a source returned nothing, leave the section out. Don't write "No items".
- If a section was skipped because of permissions, add one line at the end:
  "I couldn't read deliveries. Your account doesn't have access."
- Never paste raw JSON and never invent items the tools did not return.

## Safety

Read-only. If the user asks to act on an item (approve, reassign, record payment), hand
off to the matching skill and follow its confirmation rules. Never call
`restore_shop_backup`, `restore_rental_backup`, `restore_memory_backup` or
`delete_api_token` unless explicitly asked for that exact action.
