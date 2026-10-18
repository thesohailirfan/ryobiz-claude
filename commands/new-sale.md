---
description: Start a Ryobiz quotation or invoice for a customer and items
argument-hint: "<customer> <items and quantities> [quote|invoice]"
---

Run the Ryobiz **shop-sales** skill workflow for this sale:

$ARGUMENTS

Resolve the workspace with `list_workspaces`. Find the customer with `list_shop_parties`
(offer to create it with `save_shop_party` if missing, after confirming). Resolve items
with `list_shop_products` and check stock with `list_shop_inventory`. If the user didn't
say quotation or invoice, ask. Show the full draft (lines, GST, total in ₹) and wait for
"yes" before calling `create_shop_quotation` or `create_shop_invoice`. Then confirm the
document number and offer to record payment or print.
