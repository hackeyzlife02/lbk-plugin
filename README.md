# Lutz Bath & Kitchen: assistant plugin for clients

Ask your AI assistant about your Lutz Bath & Kitchen quotes and products instead of emailing the
showroom:

- **Quotes**: every quote on your jobs, with line items, status and valid-until dates
- **Jobs**: what's quoted on each job, room by room, and what still needs your approval
- **Products**: search by SKU, model number or description, with specs and manufacturer list price

Access is read-only and by invitation. Your LBK contact turns it on for your email and chooses which
jobs you see.

---

## Connect

The connector address is the same everywhere:

```
https://connect.lbksf.com/mcp
```

When your assistant connects, a Lutz Bath & Kitchen sign-in window opens. Enter your email, type
the 6-digit code we email you, then choose **Allow**.

### Claude (claude.ai, Claude Desktop, mobile)

Settings → **Customize** → **Connectors** → **+ Add** → **Add custom connector** → name it
*Lutz Bath & Kitchen*, paste the address above → **Continue** → **Connect**.

If Claude says it couldn't check the server, choose **Continue anyway** → **Sign in now** →
**Register automatically** (not Claude's published identity).

### Claude Code

```
claude mcp add --transport http lbk https://connect.lbksf.com/mcp
```

Then run `/mcp` and choose **lbk → Authenticate**. For the plugin instead (it adds `/lbk:quotes`
and `/lbk:price`), see *Add the LBK plugin* below.

### ChatGPT

On the **chatgpt.com** website (the ChatGPT app can't add it; on a phone, use Safari or your
browser): Settings → **Plugins** → **Browse directory** → **Add** → **Add custom MCP server** →
paste the address above, and sign in. After that it works in the ChatGPT app too. Needs ChatGPT
Plus or above.

### Try it

> "Show me my latest quote for the Union Street job."
> "List price and spec sheet for the CEA FRE41 in Black Diamond?"
> "Do you carry Vola? What's close to it in brushed brass?"
> "Find a 60 by 30 alcove tub with a left-hand drain."
> "Which items on my latest quote still need my approval?"

A step-by-step setup guide is at https://www.lbksf.com/connect/start.

Anything that changes a quote or an order still goes through the showroom: showroom@lbksf.com ·
(415) 621-4292. Your assistant can draft that email for you.

**Search record:** Lutz Bath & Kitchen keeps a record of product searches made through this connector to improve recommendations.

To disconnect, remove the connector in your assistant's settings. LBK can also turn access off at
any time.

### Add the LBK plugin (Claude, optional)

The connector address above is all you need, and it always gives you LBK's latest: how your
assistant should answer comes from LBK each time you start a conversation. The plugin adds a skill
that reminds Claude to check LBK first for any bath or kitchen product question, plus the
`/lbk:quotes` and `/lbk:price` commands. Add it from our marketplace so it keeps itself up to date:

- **Claude** (claude.ai or the desktop app): **Customize** → **Plugins** → **Add** →
  **Add marketplace** → enter `hackeyzlife02/lbk-plugin` → add **lbk**, then turn on
  **Sync automatically** for the marketplace. Connect and sign in from the plugin's
  **Connectors** tab. It's also available in Claude Code on the same account.
- **Claude Code only:**

  ```
  /plugin marketplace add hackeyzlife02/lbk-plugin
  /plugin install lbk@lbk
  ```

  Then turn on auto-update for the marketplace in `/plugin` → **Marketplaces**.

If you already added the connector by address, remove it after adding the plugin so your assistant
doesn't see LBK twice. Please don't install the plugin from an uploaded file: an uploaded copy
never updates. For ChatGPT, use the address above.

---

## For LBK

- **Server:** the `lbk-connect` repo (this plugin's parent folder). It's a separate Cloud Run
  service on its own database with its own credentials. See `../README.md` and
  `../docs/2026-11-07-lbk-client-plugin-spec.md`.
- **Who has access:** LBKSF Admin → Access → **Client Connector**.
- **Plugin files:** `plugins/lbk` is the single source. `bash client-plugin/build.sh` writes two
  uploadable archives to `client-plugin/dist/` (not committed):
  - `lbk-chatgpt-plugin-<version>.zip`: `plugin.json` + `mcp.json` (Agent Plugins format, OpenAI
    extensions: display info, starter prompts, onboarding skill, OAuth with DCR), `skills/`, `assets/`.
  - `lbk-claude-plugin-<version>.zip`: `.claude-plugin/plugin.json` + `.mcp.json`, `skills/`,
    `commands/`, `assets/`.

  Both carry the same skill (`skills/lbk-showroom`). Keep the version the same in `plugin.json`,
  `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` (a test checks). We don't
  hand out the archives: an uploaded plugin never updates. The ChatGPT one is for a future
  ChatGPT directory listing (the only ChatGPT path where updates reach clients automatically).
- **Icon:** `brand/lbk-icon.svg` is the source (gold Cormorant Garamond "LBK" on a full-bleed
  navy square, glyphs baked to paths). `assets/icon.png` (512) and `assets/logo.png` (1024) are
  rendered from it as opaque RGB: no rounded or see-through corners, since each assistant rounds
  the icon itself (see-through corners showed up white). A test checks size and opacity.
- **Keep the plugin thin.** Every client's plugin is a frozen copy, so the skill and commands only
  say "use LBK first, call `get_my_account`, follow its `guidance`". All the rules for assistants
  (what to use when, pricing, the showroom, voice) live in `src/mcp/guidance.ts`, are served live
  in `get_my_account`, and reach every client on the next connector deploy. Change rules there,
  not here.
- **When clients need to update:** only when the plugin's own files change (say, a new command or
  a new connector address). `src/__tests__/plugin-release.test.ts` fails on any such change until
  the version is bumped and the change is logged in `releases.json`. Marketplace installs with sync
  on then update by themselves after the next publish.
- **Publishing** (owner-gated): this directory is a self-contained plugin marketplace with no
  secrets, only the public connector URL, a skill and two commands. `client-plugin/publish.sh`
  copies it to the public repo `hackeyzlife02/lbk-plugin`. Run it from an up-to-date `main`, after
  the connector deploy that goes with the release:

  ```bash
  bash client-plugin/publish.sh                  # dry run: shows what would change
  YES=1 bash client-plugin/publish.sh            # publish
  CREATE=1 YES=1 bash client-plugin/publish.sh   # first time only: creates the public repo
  ```

  It refuses to change the files of a version that's already published.
