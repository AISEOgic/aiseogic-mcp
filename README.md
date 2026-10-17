# AISEOgic MCP connector

Run **AISEOgic** website audits (SEO, AI SEO, GEO, AEO, LLMO) from inside Claude, ChatGPT and other AI apps — then ask the assistant to explain the results and give you the code to fix them.

This repository contains **documentation and ready-to-paste configuration only**. The AISEOgic service itself is a hosted product; there is nothing to install or run.

| | |
|---|---|
| **Connector URL** | `https://app.aiseogic.com/mcp` |
| **Transport** | Remote MCP over HTTPS (Streamable HTTP, stateless) |
| **Sign-in** | OAuth 2.1 — authorization code + PKCE, dynamic client registration |
| **Website** | https://aiseogic.com |
| **Privacy policy** | https://aiseogic.com/privacy-policy/ |
| **Terms** | https://aiseogic.com/terms/ |
| **Support** | info@aiseogic.com |

## Connect in one step

You need an AISEOgic account (free is enough to try it — sign up at https://app.aiseogic.com).

### Claude (claude.ai and the desktop app)
1. Open **Settings → Connectors → Add custom connector**.
2. Name: `AISEOgic`. URL: `https://app.aiseogic.com/mcp`.
3. Click **Connect**, sign in to AISEOgic, review the permissions and click **Allow**.
4. In a new chat, switch the **AISEOgic** connector on and try:
   > Use AISEOgic to audit https://example.com and show me the top fixes.

### Claude Code
```bash
claude mcp add --transport http aiseogic https://app.aiseogic.com/mcp
```
Then run `/mcp` inside Claude Code and choose **Authenticate** for `aiseogic`. See [`examples/claude-code.md`](examples/claude-code.md).

### ChatGPT and other MCP-capable apps
Add a custom/remote MCP server with the URL `https://app.aiseogic.com/mcp` and choose OAuth when asked (menu names differ between apps and change over time). The URL is all that is needed — the app discovers the sign-in settings itself.

### VS Code, Cursor and other editors
Copy the matching file from [`examples/`](examples/).

## What the assistant can do

**Audit and understand**

| Tool | What it does | Changes anything? |
|---|---|---|
| `start_audit` | Starts an audit of a public website (uses one audit from your allowance). `scope`: a single page (default) or the full site (Pro/Agency) | Yes — starts an audit |
| `get_audit` | Status (`queued` · `running` · `done` · `failed`), scores and top issues | No |
| `list_my_audits` | Your recent audits | No |
| `compare_audits` | Score changes, issues fixed, new and unchanged between two audits of a site | No |
| `list_domains` · `get_domain_history` | The websites you have audited and how a site's scores changed over time | No |

**Fix**

| Tool | What it does | Changes anything? |
|---|---|---|
| `get_fix_plan` | Prioritised fixes with evidence and the detected platform | No |
| `get_quick_wins` | The fixes that give the most score for the least effort | No |
| `get_fix_details` | Where one fix goes and the code, for the detected platform | No |
| `get_fix_patch` | Validated, ready-to-paste code for **WordPress, Next.js, Shopify or plain HTML** | No |
| `recheck_fix` | Re-tests one issue on the live page (about a second, no audit used) | No |
| `mark_fix_status` | Records a fix as todo · in progress · done · skipped | Yes — saves your progress |
| `verify_fix` | "Did my fixes work?" — compares with a newer audit, or starts a re-check | Yes — may start an audit |

**AI visibility and page checks**

| Tool | What it does | Changes anything? |
|---|---|---|
| `check_ai_visibility` | Asks AI engines your customers' questions: are you mentioned or cited, at what position, who else is named, share of voice. Paid plans; only engines set up on the service are used and the rest are reported as not available | No |
| `test_ai_bot_access` | Which AI crawlers (ChatGPT, Perplexity, Claude, Gemini, Apple…) your robots.txt lets in | No |
| `analyze_answer_blocks` | Question headings and the answers under them — short and direct enough for answer engines? | No |
| `check_entity_consistency` | Is your brand name used consistently on the page (title, schema, logo, footer, profiles)? | No |
| `check_citation_readiness` | Seven content signals that make a page easier for AI assistants to cite | No |
| `generate_schema` | JSON-LD built only from details really found on the page; anything missing is listed, never invented | No |

**Account**

| Tool | What it does | Changes anything? |
|---|---|---|
| `get_account` | Your plan and how many audits you have left | No |
| `get_usage` | Credits used and left, limits and when the daily allowance resets | No |
| `estimate_audit_cost` | What an audit would cost and whether you can run it now — free, starts nothing | No |

The connector also offers ready-made **prompts** (commands in apps that support them): *Audit a website and plan the fixes*, *Weekly SEO check*, *Check whether my fixes worked*, *Fastest improvements for an audit*, *Am I visible in AI answers?*, *Audit, fix and verify* and *Check the cost first*. Results are returned as readable text and as structured data. A health check is available at `/mcp/health`.

Details, permissions and example prompts: [`TOOLS.md`](TOOLS.md).

## Fixing the issues

AISEOgic tells the assistant **what** to fix and gives ready-made, validated code for common problems (security headers, `llms.txt`, `robots.txt` rules, schema markup, meta tags, performance) for WordPress, Next.js, Shopify and plain HTML. After you apply a change, the assistant can re-test just that fix. The assistant cannot reach your server on its own:

- In **Claude.ai / ChatGPT chat** it gives you the snippet and tells you where to put it.
- In **Claude Code, Cursor or another coding agent opened in your project**, it can apply the change to your files directly, using your own access.
- **WordPress**: every suggestion works without paid plugins (WordPress core, file edits, or the free versions of Yoast SEO, Rank Math and Code Snippets).

AISEOgic never asks for, receives or stores your server, SSH or hosting credentials.

## Privacy and security in short

- The connector can: start audits of public websites you give it, read your audit results, check public pages you point it at, and read your plan and audit allowance.
- It **cannot**: change your password, billing, or any account setting, or read other people's data. The only things it can save are your own fix-progress notes.
- You can disconnect at any time from **Dashboard → Connect to AI** in the app.
- Access tokens are short-lived and stored hashed; every sign-in needs your explicit approval on the AISEOgic consent screen.

Full policy: https://aiseogic.com/privacy-policy/. To report a security problem see [`SECURITY.md`](SECURITY.md).

## Troubleshooting

| Symptom | What to try |
|---|---|
| "Couldn't connect" while adding the connector | Check the URL is exactly `https://app.aiseogic.com/mcp` (no trailing slash or query). Remove any old failed entry first. |
| Sign-in page says the request expired | Start the connection again from your AI app; approval links last a few minutes. |
| "No audits left" | Free accounts get a limited allowance; see https://app.aiseogic.com/pricing. |
| Anything else | Email info@aiseogic.com with the time of the attempt and the app you used. |

## License

The documentation and examples in this repository are released under the [MIT License](LICENSE). The AISEOgic name, logo and service are not covered by that license.
