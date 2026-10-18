# Changelog

## 1.0.5 – 19 Oct 2026

- Removes the README logo and its image files; the directory listing icon is unchanged.
- Stops naming image files in the changelog, which held the plugin for review.

## 1.0.4 – 19 Oct 2026

- Points `documentationUrl` at the Ryobiz docs page, https://ryobiz.in/docs.

## 1.0.3 – 19 Oct 2026

- Adds directory listing links to `plugin.json`: documentation, support, privacy policy and terms of service.

## 1.0.2 – 19 Oct 2026

- Adds the directory listing icon (the Ryobiz logo on white, 1080 × 1080).

## 1.0.1 – 19 Oct 2026

- Commands now trigger from plain requests in claude.ai chat, where they load as skills, and
  ask for missing details when no arguments are passed.
- README shows the Cowork command names (`/ryobiz:brief`, `/ryobiz:month-end-rent`,
  `/ryobiz:new-sale`) and explains that chat uses plain requests instead.

## 1.0.0 – 18 Oct 2026

Initial release.

- Connects the Ryobiz remote MCP server (`https://api.ryobiz.in/mcp`, HTTP, OAuth sign-in).
- Skills: `ryobiz-getting-started`, `daily-brief`, `shop-sales`, `shop-purchases-inventory`,
  `shop-deliveries`, `shop-finance-gst` (with GST reference notes), `rental-billing`,
  `rental-tenancy-lifecycle`, `work-management`, `team-capacity`.
- Commands: `/brief`, `/month-end-rent`, `/new-sale`.
- Safe defaults: workspace resolution first, preview before commit, explicit confirmation
  before destructive or financial changes, no workaround on permission denials.
