---
name: ryobiz-getting-started
description: Use when the user is new to Ryobiz, has just connected it, or asks "what can Ryobiz do", "which workspaces do I have", "what's enabled", or how to get started with shop, rental or team features.
---

# Ryobiz: getting started

Ryobiz ("Run your own business") is a business operating system for Indian small and
mid-size businesses. It covers three areas, each switched on per workspace:

- **Shop / trading**: parties, products, inventory, quotations, orders, invoices,
  purchases, deliveries and fleet, GST and finance.
- **Rentals**: rooms, tenants, agreements, meter readings, monthly billing, deposits, repairs.
- **Team and work**: projects, work items, cycles, milestones, approvals, capacity.

What a user can do depends on (a) the features enabled in the workspace, (b) their role
and permissions there, and (c) the access they granted when signing in. Never promise a
capability before checking.

## Workflow

1. **Who and where.** Call `list_workspaces`. It returns every workspace the user can
   access, with their profile, responsibilities and any personal AI role.
   - If an AI role prompt is active for the chosen workspace, follow it as context. It
     never grants extra authority.
   - Call `get_profile` if you need the user's name or account details.
2. **Pick a workspace.** If there is more than one, ask which one (or use the one the
   user named). Pass its `workspaceId` to every later call.
3. **What's switched on.** Call `get_workspace_feature_catalog` with the `workspaceId`.
   Summarise enabled areas in plain language, grouped as Shop, Rentals and Work.
4. **Not signed in yet?** `get_feature_catalog` works without sign-in and lists Ryobiz's
   product features and scope presets. Use it to explain the product, then ask the user
   to connect the Ryobiz connector and sign in.
5. **Suggest next steps** that match the enabled features, for example:
   - Shop: "Show today's sales and stock running low", "Make a quotation for Sharma Traders".
   - Rentals: "Start this month's rent billing", "Who hasn't paid rent?"
   - Work: "What's on my plate today?", "Who on my team is overloaded?"

## Ground rules (apply across all Ryobiz skills)

- **Read before write, preview before commit.** If a `preview_*` tool exists, use it first.
- **Confirm before any change that is destructive or moves money.** State exactly what
  will change (counts, ₹ amounts, parties) and wait for a clear "yes".
- **Money is integer paise** in tool calls (₹1,250.50 = 125050). Quantities are in
  thousandths of a selling unit (2.5 boxes = 2500). Convert both ways and never show
  paise to the user.
- Payments, refunds and deposits are **bookkeeping records**. They never move real money.
- If a tool says approval is required, tell the user to approve it in the Ryobiz app,
  then repeat the same call with the same arguments plus the `approvalId`.
- If a call is denied, say so plainly. Never try another tool to get around a denial.
- Never call `restore_shop_backup`, `restore_rental_backup`, `restore_memory_backup` or
  `delete_api_token` unless the user explicitly asks for that exact action.

## Output style

- Lead with the answer. Keep it short.
- Use ₹ with Indian grouping (₹1,25,000) and dates as DD MMM YYYY (05 Oct 2026).
- Use a table only to compare several rows. Never paste raw JSON.
