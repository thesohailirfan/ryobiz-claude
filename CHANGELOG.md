# Changelog

## 1.0.0 – 18 Oct 2026

Initial release.

- Connects the Ryobiz remote MCP server (`https://api.ryobiz.in/mcp`, HTTP, OAuth sign-in).
- Skills: `ryobiz-getting-started`, `daily-brief`, `shop-sales`, `shop-purchases-inventory`,
  `shop-deliveries`, `shop-finance-gst` (with GST reference notes), `rental-billing`,
  `rental-tenancy-lifecycle`, `work-management`, `team-capacity`.
- Commands: `/brief`, `/month-end-rent`, `/new-sale`.
- Safe defaults: workspace resolution first, preview before commit, explicit confirmation
  before destructive or financial changes, no workaround on permission denials.
