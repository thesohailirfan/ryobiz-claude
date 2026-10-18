---
description: Run Ryobiz month-end rent billing (meter readings → preview → drafts → review → issue)
argument-hint: "[month, e.g. October 2026]"
---

Run the Ryobiz **rental-billing** skill's month-end billing workflow.

Billing month: $ARGUMENTS (if blank, use the current month and say which month you chose).

Resolve the workspace with `list_workspaces` first. Then follow the skill in order:
check `list_rental_meters` for missing readings, record any readings the user gives,
`preview_rental_billing`, confirm, `generate_rental_billing_drafts`, review each draft
with `review_rental_invoice`, then show the count and ₹ total and get an explicit "yes"
before `issue_rental_invoice_batch`. Stop and ask at every confirmation point.
