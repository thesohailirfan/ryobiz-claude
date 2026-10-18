---
name: shop-deliveries
description: Use when the user mentions deliveries, trips, dispatch, vehicles, drivers, loading a truck, delivery proof, failed delivery, cash collected by the driver (COD), driver cash handover, or transporter receipts in their Ryobiz shop.
---

# Shop deliveries and fleet

## Conventions

- Money in tool calls is **integer paise**. Quantities are thousandths of a selling unit.
  Weights and dimensions follow the units the tools return. Show ₹ and units.
- Cash collection and handovers are bookkeeping records. They don't move money.
- Writes may need in-app approval. If so, ask the user to approve it in Ryobiz, then
  repeat the same call with the same arguments plus `approvalId`.

## Start every time

`list_workspaces`. If there are several, ask which one. Pass `workspaceId` to every call.
Then `get_shop_delivery_board` for the current picture: planned, in transit, delivered,
failed trips, and driver cash. Driver-only accounts see only their own trips.

## Plan and load a trip

1. Pick the vehicle: `list_shop_vehicles`. To add or edit one, show the details and call
   `save_shop_vehicle` on "yes".
2. Choose invoices or orders to deliver and their addresses. If addresses need geocoding
   or cleanup, use `resolve_shop_delivery_locations` and show what was resolved.
3. **Preview the load first:** `preview_shop_trip_load`. Report weight against payload,
   rack/slab fit, and any warnings. It reserves nothing.
4. Summarise vehicle, driver, stops and load %, then on "yes" call `create_shop_trip`.

## Run the trip

- **Dispatch:** confirm, then `dispatch_shop_trip`.
- **Location update:** `set_shop_trip_location`.
- **Delivered:** `deliver_shop_trip`. Attach proof with `upload_shop_delivery_proof`
  (signature must be PNG; images up to 1 MiB).
- **Failed:** ask for the reason, confirm, then `fail_shop_trip`.
- **Change driver/vehicle:** show from → to, confirm, then `reassign_shop_trip`.
- **Cancel:** show the trip, stops and goods affected, wait for "yes", then `cancel_shop_trip`.

## Cash on delivery

1. **Collected from customer:** confirm invoice, amount in ₹, mode, then
   `record_shop_trip_payment`. It updates the invoice balance and the driver's cash in hand.
2. **Driver hands cash to office:** show the driver's cash balance from the board,
   confirm the amount handed over, then `record_shop_driver_cash_handover`.

## Transporter receipts (hired vehicles)

1. Collect vehicle number, transporter, base charge, any additions (loading, toll) and
   deductions, and any advance paid. Show the net total, then on "yes" call
   `create_shop_transport_receipt`.
2. `get_shop_transport_receipt_print` returns a printable receipt.

## After every write

Confirm the trip or receipt identifier and its new status ("Trip T-031 dispatched with
WB-02-AB-1234, 4 stops"), then offer one next step.

## Guardrails

- Always preview the load before creating a trip.
- Confirm before cancel, fail, reassign, and any cash entry.
- If a call is denied, say so plainly. Never try another tool to get around it.
- Never paste raw JSON. Use ₹ with Indian grouping (₹1,25,000) and dates as DD MMM YYYY.
