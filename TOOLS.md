# Tools

All tools act on **your own** AISEOgic account. Access is granted per permission on the consent screen: `audits:read` (read results and run page checks), `audits:run` (start audits and save fix progress — the "write" permission) and `account:read` (plan and usage).

## Audit and understand
| Tool | Permission | Read-only | Notes |
|---|---|---|---|
| `start_audit` | `audits:run` | No | `url`, optional `scope`: `single_page` (default) or `full_site` (Pro/Agency). Returns an `audit_id` at once; the same URL started again while running returns the running audit. Private/internal addresses are refused. |
| `get_audit` | `audits:read` | Yes | `audit_id`. `state` is `queued`, `running`, `done` or `failed` (the original `status` is returned too); then scores (0–100: overall, seo, speed, security, accessibility, ai_seo, geo, aeo, llmo) and the top issues. |
| `list_my_audits` | `audits:read` | Yes | Optional `limit` (1–50). |
| `compare_audits` | `audits:read` | Yes | `audit_id_a`, `audit_id_b` (any order; or just one to compare with the previous audit of the same site), optional `detail_level` (`summary` default, or `full`). Score changes and which issues were fixed, are new, or unchanged. Free accounts get score changes only. |
| `list_domains` | `audits:read` | Yes | The websites you have audited, with audit count and latest score. |
| `get_domain_history` | `audits:read` | Yes | `domain`, optional `limit`, `detail_level`. Score trend, best and worst audit. |

## Fix
| Tool | Permission | Read-only | Notes |
|---|---|---|---|
| `get_fix_plan` | `audits:read` | Yes | `audit_id`, optional `platform`. Fixes ordered by impact and effort; each has an `issue_id`, evidence and whether ready-made code exists. |
| `get_quick_wins` | `audits:read` | Yes | `audit_id`, optional `limit` (1–15). Issues ranked by score points per minute of estimated effort. |
| `get_fix_details` | `audits:read` | Yes | `audit_id`, `issue_id`, optional `platform`. Where the change goes, a snippet and steps. |
| `get_fix_patch` | `audits:read` | Yes | `fix_id` (`audit_id:issue_id`, both from `get_fix_plan`) and `platform` (`wordpress`, `nextjs`, `shopify`, `html`). Ready-to-paste output (headers, `robots.txt` rules, `llms.txt`, meta tags, JSON-LD…) that is checked to be valid before it is returned. Where a platform cannot do something (for example Shopify cannot set response headers) it says so and gives the workaround. |
| `recheck_fix` | `audits:read` | Yes | `fix_id`. Re-tests only that issue on the live page: `resolved`, `still_failing` (with the reason) or `not_checkable` (speed measurements need a full audit). Uses no audit. |
| `mark_fix_status` | `audits:run` | No | `fix_id`, `status` (`todo`, `in_progress`, `done`, `skipped`), optional `note`. Returns the progress for that audit. |
| `verify_fix` | `audits:run` | No | `audit_id`, optional `issue_ids`. Compares with a newer audit of the same site; if none exists it starts one (uses one audit) and returns the result when called again shortly after. |

## AI visibility and page checks
| Tool | Permission | Read-only | Notes |
|---|---|---|---|
| `check_ai_visibility` | `audits:read` | Yes | `url`, `prompts` (1–5 questions), optional `brand`, `competitors`, `engines`. Paid plans. Reports per question and engine whether you are mentioned, cited (only when the engine provides sources — otherwise unknown) and at what position, who else is named, and share of voice. Only engines set up on the service are used; others are listed as not available — nothing is simulated. Results are cached for 24 hours. |
| `test_ai_bot_access` | `audits:read` | Yes | `url`. Reads robots.txt and reports, for each known AI crawler, whether it may fetch the site. Does not test firewalls or CDN bot protection. |
| `analyze_answer_blocks` | `audits:read` | Yes | `url`. Question headings, the answer under each (format, length), snippet-readiness and FAQ markup. |
| `check_entity_consistency` | `audits:read` | Yes | `brand`, `url`. Compares your brand name with the names the page uses for itself; lists mismatches and missing entity signals. |
| `check_citation_readiness` | `audits:read` | Yes | `url`. Seven content signals (Q&A, lists, statistics, sources, dates, definitions, structured data) with a score and what to add. |
| `generate_schema` | `audits:read` | Yes | `url`, `type` (`Organization`, `WebSite`, `WebPage`, `Article`, `FAQPage`, `BreadcrumbList`, `LocalBusiness`). JSON-LD from details found on the page; missing required details are listed, not invented. |

## Account
| Tool | Permission | Read-only | Notes |
|---|---|---|---|
| `get_account` | `account:read` | Yes | Plan and remaining audit allowance. |
| `get_usage` | `account:read` | Yes | Credits used and left, limits, reset date, recent activity. |
| `estimate_audit_cost` | `account:read` | Yes | `url`, optional `depth`. Credits, pages and time for an audit and whether you can run it now. Starts nothing. |

`platform` (for `get_fix_plan`, `get_fix_details`) accepts: `wordpress`, `nextjs`, `netlify`, `vercel`, `apache`, `nginx`, `static`. `get_fix_patch` accepts `wordpress`, `nextjs`, `shopify`, `html`.

## Prompts
`full_audit_and_fix_plan`, `weekly_seo_check`, `did_my_fix_work`, `quick_wins`, `ai_visibility_check`, `audit_fix_verify`, `estimate_then_audit` — ready-made starting points that appear as commands in apps that support MCP prompts.

## Example prompts

**Quick audit**
> Use AISEOgic to audit https://yourwebsite.com. Show my scores, the 5 biggest problems, and a step-by-step fix plan ordered by impact.

**Am I visible in AI answers?**
> Use AISEOgic to check whether AI assistants mention my brand "YOUR BRAND" (https://yourwebsite.com). Test these questions: "best [your category] tools", "[your category] for small business", "alternatives to [competitor]". Tell me who gets mentioned instead of me and what to change.

**Audit, fix, verify**
> Use AISEOgic to audit https://yourwebsite.com. Take the top 3 fixes, give me the ready-to-paste code for my platform (WordPress / Next.js / Shopify / plain HTML), and after I say "done", re-check each fix and tell me which ones passed.

**Before / after**
> Use AISEOgic to compare my last two audits for https://yourwebsite.com. Show what improved, what got worse, and what is still unfixed.

**Check cost first**
> Use AISEOgic to estimate the credit cost of a full audit of https://yourwebsite.com, show my remaining credits, then run it only if I have enough.

**Weekly routine**
> Every time I say "weekly SEO check", use AISEOgic to audit https://yourwebsite.com, compare it to the previous audit, and give me the 3 most important things to do this week.

## Limits

Audit allowance depends on your plan (Free accounts can run a limited number of audits through the connector and see the top issues; paid plans get more and full results). Some features — such as AI-visibility checks — are for paid plans, and requests are rate-limited per account. `get_usage` shows your numbers. See https://app.aiseogic.com/pricing.
