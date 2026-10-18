---
name: rental-billing
description: Use when the user mentions rent, month-end billing, rent invoices, electricity or water meter readings, utility charges, rent collection, who hasn't paid, receipts, tenant statements, credit notes, or reversing a rent payment in Ryobiz.
---

# Rental billing and collections

## Conventions

- Money in tool calls is **integer paise** (₹8,500 = 850000). Show ₹ with Indian grouping.
- Billing periods are months. Say "October 2026" to the user, and use the period format
  the tool expects.
- Payments, credits and voids are bookkeeping records. They don't move money.
- Many writes need the record's current `expectedRevision`. Read it fresh before writing.
- Writes may need in-app approval. If so, ask the user to approve it in Ryobiz, then
  repeat the same call with the same arguments plus `approvalId`.

## Start every time

`list_workspaces`. If there are several, ask which one. Pass `workspaceId` to every call.
`get_rental_dashboard` gives the current picture: dues, collections, pending drafts.

## Month-end billing run

1. **Meters.** `list_rental_meters`. List meters missing a reading for the period.
2. **Readings.** For each reading the user gives, check it isn't below the previous
   reading, then `record_rental_meter_reading`. If a saved reading was wrong, show
   old → new and confirm, then `correct_rental_meter_reading`.
3. **Preview.** `preview_rental_billing` for the period. Summarise the number of
   invoices, total rent, total utilities, grand total in ₹, and any warnings (missing
   readings, vacant rooms, unusual consumption). Nothing is saved yet.
4. **Confirm**, then `generate_rental_billing_drafts`. Warn first that regenerating
   replaces existing drafts for that period.
5. **Review.** Show the drafts (tenant, room, rent, utilities, total). Fix warnings first.
   Then, on "yes", call `review_rental_invoice` for each draft the user approves.
   Reviewing does not issue or send anything.
6. **Issue (posts to the books).** Show the count and ₹ total of reviewed invoices about
   to be issued and wait for a clear "yes". Then call `issue_rental_invoice_batch` for
   several, or `issue_rental_invoice` for one. Issued invoices can only be changed
   with a credit or void.
7. **Wrap up.** "Issued 18 invoices for October 2026 totalling ₹2,46,300." Offer to
   share tenant statements or check who's unpaid.

## Collections

- **Record payment:** confirm tenant, ₹ amount, date, mode and reference, then
  `record_rental_payment`.
- **Allocate** to invoices: show which invoices and how much each, then
  `allocate_rental_payment`. To undo an allocation, confirm and `unallocate_rental_payment`.
- **Reverse a payment** (e.g. bounced cheque): show the payment and its allocations,
  wait for "yes", then `reverse_rental_payment`.
- **Receipt / statement:** `get_rental_receipt_print`, `get_rental_tenant_statement_print`.
- **Reports** (dues, collections, ageing): `get_rental_reports`.

## Correcting issued invoices (always confirm)

- **Partial correction:** `credit_rental_invoice`. Show the invoice, ₹ credit and reason.
- **Cancel entirely:** `void_rental_invoice`. Show the invoice, total and any linked
  payments, and warn that it cannot be undone. Wait for "yes".

## Guardrails

- Never generate, issue, credit, void or reverse without showing the summary and getting "yes".
- If a call is denied, say so plainly. Never try another tool to get around it.
- Never call `restore_rental_backup` unless the user explicitly asks for that exact action.
- Never paste raw JSON. Dates as DD MMM YYYY.
