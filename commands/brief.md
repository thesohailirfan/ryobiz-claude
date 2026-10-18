---
description: Get your Ryobiz daily brief. Use when the user asks for a morning brief, what's on their plate today, or what's overdue across work, approvals, deliveries and rent dues.
argument-hint: "[workspace name]"
---

Run the Ryobiz **daily-brief** skill workflow.

Workspace (optional): $ARGUMENTS

If no workspace was given above (blank or an unfilled placeholder), take it from the
conversation. Resolve the workspace with `list_workspaces`, and ask if there is more than
one and none was named. Then build the brief in the skill's order:
overdue and urgent → waiting on others → approvals → today's deliveries and rent dues →
milestones in the next 7 days. Include only sections whose features are enabled. This is
read-only. Don't change any records.
