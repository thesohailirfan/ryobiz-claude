# Ryobiz – Run your own business

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/ryobiz-logo-white.png">
  <img src="assets/ryobiz-logo-black.png" alt="Ryobiz logo" width="120">
</picture>

## What it does

Ryobiz is a business operating system for small and mid-size businesses in India. This
plugin connects Claude to your Ryobiz account so you can run your business from a
conversation. You can make quotations and GST invoices, check stock, plan deliveries,
read profit and loss or the GST register, run month-end rent billing for your properties,
settle tenant deposits, and keep your team's projects and workload on track. Claude
follows Ryobiz's own permissions and approval rules, and it asks before changing anything.

## Setup

1. Install the **Ryobiz** plugin in Claude on **claude.ai** or **Cowork**.
2. Open the plugin's **Connectors** tab and connect the **Ryobiz** connector.
3. Sign in with your Ryobiz account and choose the workspaces and access to grant.
4. Ask "What can Ryobiz do in my workspace?" to get started.

> **Claude Code:** signing in to Ryobiz from Claude Code isn't supported yet. The plugin
> installs, but the Ryobiz sign-in step won't complete. Use claude.ai or Cowork for now.

You need an existing Ryobiz account. What Claude can do depends on the features enabled
in your workspace, your role there, and the access you granted at sign-in.

## Example prompts

- "Give me my morning brief for Sharma Tiles."
- "Make a quotation for Gupta Constructions: 40 boxes of Ivory Matt 600x600 and 10 bags of tile adhesive."
- "Which products are below reorder level, and what did we buy from Shree Ceramics last month?"
- "Plan tomorrow's deliveries on the Tata 407 and check the load fits."
- "Show this month's P&L and who owes us more than ₹50,000."
- "Reconcile my purchase register against this GSTR-2B file and list the mismatches."
- "Start October rent billing. Here are the meter readings for rooms 101 to 108."
- "Ravi in room 204 is moving out on 30 Nov. Work out his deposit refund."
- "Who on my team is overloaded this week, and who can take RYO-214?"

## Skills and commands

Skills (loaded automatically when relevant):

- **ryobiz-getting-started**: workspaces, enabled features, what you can do.
- **daily-brief**: overdue work, approvals, today's deliveries and rent dues.
- **shop-sales**: quotation → order → invoice → payment → print, returns and cancellations.
- **shop-purchases-inventory**: purchase orders, receiving goods, stock checks and adjustments.
- **shop-deliveries**: trips, load planning, dispatch, proof of delivery, driver cash.
- **shop-finance-gst**: P&L, receivables and payables, expenses, journals, GST and bank reconciliation.
- **rental-billing**: meter readings, monthly billing run, rent collection, credits and voids.
- **rental-tenancy-lifecycle**: new tenants, agreements, transfers, repairs, move-out and deposit settlement.
- **work-management**: projects, work items, cycles, milestones, approvals, decisions and risks.
- **team-capacity**: workload, assignment recommendations, rebalancing.

Commands:

- `/brief`: run the daily brief.
- `/month-end-rent [month]`: run the month-end rental billing workflow.
- `/new-sale <customer> <items>`: start a quotation or invoice.

## Data & privacy

The plugin connects to Ryobiz's server at `https://api.ryobiz.in/mcp`. When you ask Claude
to read or change something, only the business records involved in that request (for
example parties, products, invoices, tenants, work items) are exchanged with Ryobiz. Access
is limited to the workspaces and permissions you granted when you signed in, and you can
revoke it at any time from your Ryobiz account. The plugin itself is only instructions and
configuration. It stores no data and contains no credentials. Sign-in uses Ryobiz's own
OAuth flow.

- Privacy policy: <https://ryobiz.in/privacy>
- Terms: <https://ryobiz.in/terms>
- Support: <info@ryobiz.in> · <https://ryobiz.in/support>

## Safety

- Claude reads before it writes, and it shows a preview wherever Ryobiz offers one
  (billing, trip loads, reassignments).
- Before anything destructive or financial, such as issuing invoices, recording
  payments or journals, cancelling, voiding, reversing, adjusting stock, or moving out
  a tenant, Claude summarises exactly what will change (counts, ₹ amounts, parties)
  and waits for your "yes".
- Payments, refunds and deposits in Ryobiz are bookkeeping records. Claude never moves real money.
- If Ryobiz denies an action because of your permissions, Claude tells you and does not
  look for a workaround. Actions that need approval in Ryobiz stay pending until someone
  approves them there.
- Backup restores and API token deletion are never run unless you ask for them explicitly.
- Claude presents financial and GST figures but does not give tax or legal advice.
