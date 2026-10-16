# Tools

All tools act on **your own** AISEOgic account. Access is granted per scope on the consent screen.

| Tool | Scope | Read-only | Notes |
|---|---|---|---|
| `start_audit` | `audits:run` | No | Starts an audit of a public website (`url`, optional `scope`: `single_page` (default, the page at the URL) or `full_site` (Pro/Agency)). Starting the same URL again while it is running returns the running audit. Private/internal addresses are refused. |
| `get_audit` | `audits:read` | Yes | `audit_id`. Returns status/progress, then scores (0–100: overall, seo, speed, security, accessibility, ai_seo, geo, aeo, llmo) and the top issues. |
| `get_quick_wins` | `audits:read` | Yes | `audit_id`, optional `limit` (1–15). Issues ranked by score points per minute of estimated effort, with the time and points for each. |
| `get_fix_plan` | `audits:read` | Yes | `audit_id`, optional `platform`. Fixes ordered by impact and effort; each has an `issue_id`, evidence, and whether ready-made code exists. The site platform is detected with one request to the audited page unless you pass it. |
| `get_fix_details` | `audits:read` | Yes | `audit_id`, `issue_id`, optional `platform`. Returns where the change goes, a code snippet and steps. |
| `compare_audits` | `audits:read` | Yes | `audit_id`, optional `baseline_audit_id` (defaults to the previous audit of the same site). Score changes, issues fixed / new / still open, and the site's score history. Free accounts get score changes only. |
| `verify_fix` | `audits:run` | No | `audit_id`, optional `issue_ids`. Compares with a newer audit of the same site; if none exists it starts a re-check (uses one audit) and returns the result when called again shortly after. |
| `list_my_audits` | `audits:read` | Yes | Optional `limit` (1–50). |
| `get_account` | `account:read` | Yes | Plan and remaining audit allowance. |

`platform` accepts: `wordpress`, `nextjs`, `netlify`, `vercel`, `apache`, `nginx`, `static`.

## Example prompts

- *Use AISEOgic to show my account and how many audits I have left today.*
- *Audit https://example.com with AISEOgic and summarise the three biggest problems.*
- *Give me the AISEOgic fix plan for that audit. My site runs on WordPress and I don't have any paid plugins.*
- *Show the exact code to fix `no_xframe` for that audit.*
- *Which issues would give me the biggest score gain for the least work?*
- *I fixed the headers — check whether it worked.*
- *Compare this audit with my previous one.*
- *List my last five AISEOgic audits.*

## Prompts

`full_audit_and_fix_plan`, `weekly_seo_check`, `did_my_fix_work`, `quick_wins` — ready-made starting points that appear as commands in apps that support MCP prompts.

## Limits

Audit allowance depends on your plan (Free accounts can run a limited number of audits through the connector and see the top issues; paid plans get more and full results). Requests are rate-limited per account. See https://app.aiseogic.com/pricing.
