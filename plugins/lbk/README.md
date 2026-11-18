# Lutz Bath & Kitchen

For interior designers, architects and builders who work with Lutz Bath & Kitchen (LBK), San
Francisco's boutique bath and kitchen showroom. Ask Claude about your LBK quotes and products
instead of emailing the showroom.

## What it does

- **Your quotes and jobs**: read your own LBK jobs and published quotes, room by room: line items,
  the prices LBK quoted you, status, approvals, valid-until dates and earlier versions.
- **Export a quote**: as an LBK PDF, a Studio Designer import file, or an Excel file for Programa.
- **Products**: search bath and kitchen products by SKU, model number or description, with specs,
  finishes, spec sheets, manufacturer list prices and whether LBK carries the brand.
- **Add to quote**: when LBK turns it on for you, ask Claude to add a product to one of your quotes.
  LBK reviews every request before anything changes on your quote.

Product prices are manufacturer list prices. Your quotes show what LBK quoted you. Approvals,
orders, delivery dates and trade pricing go through the showroom: showroom@lbksf.com or
(415) 621-4292.

## Who can use it

Access is by invitation. Your LBK contact turns it on for your email address and chooses which jobs
you see. Setup guide: https://www.lbksf.com/connect/start

## Install

- **Claude** (claude.ai or the desktop app): **Customize** → **Plugins** → **Add** →
  **Add marketplace** → enter `hackeyzlife02/lbk-plugin` → add **lbk**, then turn on
  **Sync automatically** so you always have the latest version.
- **Claude Code**:

```
/plugin install lbk --marketplace hackeyzlife02/lbk-plugin
```

## What's in the plugin

- **Connector** `lbk`: LBK's remote MCP server at `https://connect.lbksf.com/mcp`. When you connect,
  LBK's sign-in opens: enter your email, type the 6-digit code LBK emails you, then choose
  **Allow**. There is no password.
- **Skill** `lbk-showroom`: tells Claude to use the LBK tools for your LBK quotes and for bath and
  kitchen product questions.
- **Commands** `/lbk:quotes` (summarize your quotes) and `/lbk:price` (look up a product and its
  list price).

The plugin runs nothing on your computer: no hooks, scripts or local servers. Everything it reads
comes from LBK's connector over HTTPS, and it sends nothing anywhere else.

## Your data

The connector keeps your access (email, name, firm and the jobs LBK shared with you), a record of
each request your assistant makes to it, your product searches, and short-lived sign-in records.
It never stores passwords, payment details or your conversations. LBK staff use this to support your
access and improve product recommendations. Full details: https://www.lbksf.com/connect/privacy

To disconnect, remove Lutz Bath & Kitchen in Claude's settings. To have your access removed, email
showroom@lbksf.com.

## License

Proprietary. Copyright Lutz Bath & Kitchen. All rights reserved.
