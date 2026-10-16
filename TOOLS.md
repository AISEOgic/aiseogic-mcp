# Tools

All tools act on **your own** AISEOgic account. Access is granted per scope on the consent screen.

| Tool | Scope | Read-only | Notes |
|---|---|---|---|
| `start_audit` | `audits:run` | No | Starts an audit of a public website (`url`). Starting the same URL again while it is running returns the running audit. Private/internal addresses are refused. |
| `get_audit` | `audits:read` | Yes | `audit_id`. Returns status/progress, then scores (0–100: overall, seo, speed, security, accessibility, ai_seo, geo, aeo, llmo) and the top issues. |
| `get_fix_plan` | `audits:read` | Yes | `audit_id`, optional `platform`. Fixes ordered by impact and effort; each has an `issue_id`, evidence, and whether ready-made code exists. The site platform is detected with one request to the audited page unless you pass it. |
| `get_fix_details` | `audits:read` | Yes | `audit_id`, `issue_id`, optional `platform`. Returns where the change goes, a code snippet and steps. |
| `list_my_audits` | `audits:read` | Yes | Optional `limit` (1–50). |
| `get_account` | `account:read` | Yes | Plan and remaining audit allowance. |

`platform` accepts: `wordpress`, `nextjs`, `netlify`, `vercel`, `apache`, `nginx`, `static`.

## Example prompts

- *Use AISEOgic to show my account and how many audits I have left today.*
- *Audit https://example.com with AISEOgic and summarise the three biggest problems.*
- *Give me the AISEOgic fix plan for that audit. My site runs on WordPress and I don't have any paid plugins.*
- *Show the exact code to fix `no_xframe` for that audit.*
- *List my last five AISEOgic audits.*

## Limits

Audit allowance depends on your plan (Free accounts can run a limited number of audits through the connector and see the top issues; paid plans get more and full results). Requests are rate-limited per account. See https://app.aiseogic.com/pricing.
