---
name: rental-tenancy-lifecycle
description: Use when the user mentions a new tenant, rent agreement, lease renewal or rent change, moving a tenant to another room, security deposit, notice period, move-out, deposit refund or deductions, or room repairs and maintenance in Ryobiz rentals.
---

# Tenancy lifecycle: new tenant → move-out

## Conventions

- Money in tool calls is **integer paise** (₹10,000 = 1000000). Show ₹ with Indian grouping.
- Deposits, deductions and refunds are bookkeeping records. They don't move money.
- Many writes need the record's current `expectedRevision`. Read it fresh before writing.
- Writes may need in-app approval. If so, ask the user to approve it in Ryobiz, then
  repeat the same call with the same arguments plus `approvalId`.

## Start every time

`list_workspaces`. If there are several, ask which one. Pass `workspaceId` to every call.

## New tenant

1. **Room.** `list_rental_rooms`. Confirm the room is vacant, or has space if shared.
2. **Tenant.** Collect name, phone, ID/KYC reference (if the user gives one), and an
   emergency contact. Show it, then call `save_rental_tenant` on "yes".
3. **Agreement.** Collect start date, monthly rent, deposit amount, billing day,
   utility terms, lock-in and notice period. Show the summary, then
   `create_rental_agreement` on "yes".
4. **Move in.** `admit_rental_occupant` with the move-in date. Confirm first.
5. **Deposit received.** Confirm ₹ amount, date, mode and reference, then
   `record_rental_deposit`.
6. Confirm: "Ravi Kumar admitted to Room 204 from 01 Nov 2026, rent ₹9,000, deposit ₹18,000 recorded."

## During the tenancy

- **Rent change or renewal:** show old → new terms and the effective date, confirm,
  then `amend_rental_agreement`.
- **Room transfer:** show from → to room, the date, and any rent change. Confirm, then
  `transfer_rental_occupant`.
- **Notice given:** record the notice date and intended move-out date with
  `record_rental_agreement_notice`.

## Repairs

- Log or update: `save_rental_repair` (room, issue, vendor, estimated cost).
- Progress: `set_rental_repair_status`.
- Charge the tenant for damage: show the ₹ amount and reason, confirm, then
  `recover_rental_repair_cost`.

## Move-out (settlement first, always)

1. Gather the deposit held, unpaid dues (including the final month and pending
   utilities), and any deductions (damage, cleaning, unrecovered repairs).
2. **Show the settlement before doing anything:**

   | | ₹ |
   | --- | --- |
   | Deposit held | 18,000 |
   | − Unpaid dues | 4,250 |
   | − Deductions (damage to wardrobe door) | 1,500 |
   | **= Refund due** | **12,250** |

   If the result is negative, show it as the amount the tenant still owes.
3. Wait for the user's "yes" to the whole settlement. Then, in order:
   - `move_out_rental_occupant` (move-out date).
   - `apply_rental_deposit` to offset unpaid dues.
   - `deduct_rental_deposit` for each deduction, with its reason.
   - `record_rental_deposit_refund` for the refund (date, ₹, mode, reference).
     This records the refund only. The user pays the tenant separately.
   - `end_rental_agreement`.
4. Confirm each step's result. If any step fails, stop and report what was done and what wasn't.

## Guardrails

- Never move out, end an agreement, deduct, apply or refund a deposit without the
  settlement shown and a clear "yes".
- If a call is denied, say so plainly. Never try another tool to get around it.
- Never paste raw JSON. Dates as DD MMM YYYY.
