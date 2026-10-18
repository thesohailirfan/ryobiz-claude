# Verified Ryobiz MCP tools

Every MCP tool name referenced by this plugin's skills and commands. Each name was checked
against the Ryobiz server's published tool catalog (560 tools at `https://api.ryobiz.in/mcp`)
on 18 Oct 2026. Skills must not reference any tool that is not listed here.

Conventions shared by these tools: money is integer paise, shop quantities are thousandths
of a selling unit, and writes can require in-app approval (then the same call is repeated with
`approvalId`). `get_feature_catalog` is the only tool that works without sign-in.

## Account and workspace
- `list_workspaces`
- `get_profile`
- `get_workspace_feature_catalog`
- `get_feature_catalog`

## Daily brief
- `get_my_day`
- `get_attention_items`
- `get_workspace_dashboard`
- `list_approvals`
- `list_blockers`
- `get_shop_delivery_board`
- `get_rental_dashboard`

## Shop: sales
- `list_shop_parties`
- `save_shop_party`
- `list_shop_products`
- `list_shop_inventory`
- `create_shop_quotation`
- `create_shop_order`
- `convert_shop_document`
- `create_shop_invoice`
- `record_shop_payment`
- `record_shop_advance`
- `allocate_shop_advance`
- `return_shop_invoice_goods`
- `cancel_shop_document`
- `get_shop_document_print`
- `print_shop_document`
- `calculate_shop_room_quantity`

## Shop: purchases and inventory
- `create_shop_purchase_order`
- `receive_shop_purchase_order`
- `create_shop_purchase`
- `cancel_shop_purchase_remainder`
- `list_shop_purchases`
- `adjust_shop_stock`
- `list_shop_movements`
- `save_shop_product`

## Shop: deliveries
- `list_shop_vehicles`
- `save_shop_vehicle`
- `create_shop_trip`
- `preview_shop_trip_load`
- `dispatch_shop_trip`
- `set_shop_trip_location`
- `deliver_shop_trip`
- `fail_shop_trip`
- `reassign_shop_trip`
- `cancel_shop_trip`
- `upload_shop_delivery_proof`
- `record_shop_trip_payment`
- `record_shop_driver_cash_handover`
- `resolve_shop_delivery_locations`
- `create_shop_transport_receipt`
- `get_shop_transport_receipt_print`

## Shop: finance and GST
- `get_shop_finance_overview`
- `get_shop_reports`
- `list_shop_finance_report_rows`
- `get_shop_gst_register`
- `list_shop_gst_register_rows`
- `reconcile_shop_gst_statement`
- `reconcile_shop_gst_statement_page`
- `reconcile_shop_bank_entry`
- `record_shop_expense`
- `record_shop_journal`
- `record_shop_opening_capital`
- `list_shop_accounts`

## Rentals: billing and collections
- `list_rental_meters`
- `record_rental_meter_reading`
- `correct_rental_meter_reading`
- `preview_rental_billing`
- `generate_rental_billing_drafts`
- `review_rental_invoice`
- `issue_rental_invoice_batch`
- `issue_rental_invoice`
- `record_rental_payment`
- `allocate_rental_payment`
- `unallocate_rental_payment`
- `reverse_rental_payment`
- `credit_rental_invoice`
- `void_rental_invoice`
- `get_rental_receipt_print`
- `get_rental_tenant_statement_print`
- `get_rental_reports`

## Rentals: tenancy lifecycle and repairs
- `save_rental_tenant`
- `list_rental_rooms`
- `create_rental_agreement`
- `amend_rental_agreement`
- `admit_rental_occupant`
- `transfer_rental_occupant`
- `record_rental_deposit`
- `record_rental_agreement_notice`
- `move_out_rental_occupant`
- `deduct_rental_deposit`
- `apply_rental_deposit`
- `record_rental_deposit_refund`
- `end_rental_agreement`
- `save_rental_repair`
- `set_rental_repair_status`
- `recover_rental_repair_cost`

## Work management
- `list_projects`
- `get_project_health`
- `list_work_items`
- `create_work_item`
- `update_work_item`
- `transition_work_item`
- `bulk_update_work_items`
- `create_work_item_comment`
- `create_cycle`
- `rollover_cycle`
- `create_milestone`
- `create_approval_request`
- `transition_approval_request`
- `create_decision`
- `create_risk`

## Team capacity
- `get_capacity`
- `get_team_capacity`
- `get_user_capacity`
- `get_overloaded_users`
- `find_available_people`
- `get_assignment_recommendations`
- `preview_rebalancing`
- `preview_delegation`
- `bulk_assign_work_items`

## Named only to forbid unprompted use
Skills mention these solely to say they must never be called unless the user explicitly asks
for that exact action.
- `restore_shop_backup`
- `restore_rental_backup`
- `restore_memory_backup`
- `delete_api_token`
