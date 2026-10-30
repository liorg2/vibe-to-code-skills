---
name: launch-check
disable-model-invocation: true
description: Run the 30-item pre-launch checklist for a vibe-coded website or app against this project. Use when the user says "launch check", "ready to launch", or asks if the app is safe to show to real clients.
---

# Launch check

From Vibe to Dev (https://www.vibetodev.com/). Full tickable checklist: https://www.vibetodev.com/ai-vibe-coding-website-launch-checklist

For each check below, inspect this project (code, git history, config, and the live site if the user gives its URL) and give a verdict:

- **PASS**: with the evidence (file, line, header, command output).
- **FAIL**: with the evidence and the smallest fix.
- **ASK ME**: the check needs something only the user can confirm (a dashboard setting, a DNS record, an account). Say exactly where to look.

Product names in this skill (hosts, databases, payment, email, error-tracking and AI tools) are examples only. First find out what this project actually uses, from its code, config and package files, and apply each step to that. If a check names a tool the project doesn't use, check the equivalent; if nothing is equivalent, mark it N/A and say why. This skill works in any AI coding agent (Claude Code, Cursor, Codex, Copilot and others). If it names one tool's command, folder or file, use your agent's equivalent.

This skill is READ-ONLY until the report is done. The one exception is the HTML copy at the end, and only if the user says yes. Do not edit, create or delete files, run migrations, install packages, commit or push. Reading files, git history, running audits and fetching the live site's public pages is fine.
Some checks below say "add", "draft" or "write": for those, only describe the change as a proposed fix in the report. Apply it only if the user asks after the report.
In the "Do:" lines, "I" and "me" mean the user: report to them.
Never connect to the production database. Do not mark PASS on a guess.

## Cloud providers

Do not assume Vercel. First find which provider(s) this project really uses: look for `vercel.json`, `netlify.toml`, `wrangler.toml`, `amplify.yml`, `app.yaml`, `firebase.json`, `fly.toml`, `render.yaml`, `railway.json`, `Dockerfile`, `serverless.yml`, Terraform or Bicep files and CI workflows, and for the live site's response headers (`Server`, `X-Vercel-Id`, `CF-Ray`, `X-Amz-*`). If it is still unclear, ask the user. Then use that provider's names from the tables below.

Menu names change. If one is missing, say what to look for and point to the provider's official docs; never invent a menu path. A provider not listed (DigitalOcean, Hetzner, Heroku, a plain VPS) has the same jobs: find them in its docs. On a plain VPS, rollback, firewall and spend limits are yours to set up.

| Provider | Secrets and env vars | Logs | Roll back | Spend alert or cap | Rate limit or firewall |
|---|---|---|---|---|---|
| Vercel | Project → Settings → Environment Variables | Project → Logs | Deployments → an earlier one → Instant Rollback / Promote to Production | Settings → Billing → Spend Management | Project → Firewall |
| Netlify | Site configuration → Environment variables | Logs (deploys and functions) | Deploys → an earlier one → Publish deploy | Team billing → usage notifications | Rate limiting in the site's security settings (plan-dependent) |
| Cloudflare Pages / Workers | Settings → Variables and Secrets | Workers Logs, Pages deployment logs | Pages: Deployments → Rollback to this deployment | Billing → Notifications | Security → WAF → Rate limiting rules |
| AWS (Amplify, App Runner, ECS, Lambda) | Secrets Manager or SSM Parameter Store (Amplify: Hosting → Environment variables) | CloudWatch Logs | Redeploy the earlier build, image or task definition | AWS Budgets and billing alarms | AWS WAF rate-based rules |
| Google Cloud (Cloud Run, Firebase Hosting) | Cloud Run: Variables & Secrets, backed by Secret Manager | Cloud Logging | Cloud Run: send traffic to the earlier revision. Firebase Hosting: Release history → Rollback | Billing → Budgets & alerts | Cloud Armor rate limiting |
| Azure (Static Web Apps, App Service) | Environment variables, or Key Vault references | Log stream, Application Insights | App Service: swap deployment slots. Static Web Apps: redeploy the earlier commit | Cost Management → Budgets | Front Door WAF rate limiting |
| Render, Railway, Fly.io | Render: Environment. Railway: Variables. Fly: `fly secrets` | Dashboard logs, `fly logs` | Render: Events → Rollback. Railway: redeploy an earlier deployment. Fly: redeploy the earlier release | Usage and billing alerts (differ by plan) | Put Cloudflare in front |

Databases, for backup and access checks:

| Provider | Restore test | Who can reach it |
|---|---|---|
| Neon | Create a branch from a past point in time | Role passwords, IP allow list |
| Supabase | Dashboard → Database → Backups (point-in-time on paid plans) | Row-level security on every table |
| AWS RDS / Aurora | Restore to a point in time as a new instance | Security groups; not publicly reachable |
| Google Cloud SQL | Restore or clone a backup into a new instance | Authorized networks or private IP |
| Azure Database | Restore to a point in time as a new server | Firewall rules |
| Firebase (Firestore) | Scheduled backup or point-in-time recovery into a new database | Security Rules |

Whatever the provider, restore into a new scratch database, never over production.

## Redact

Never print a secret, key, password or token. Report its name and where it is, and write `<REDACTED>` in place of any value you have to show.

## How to talk

- Brief, friendly and professional. Short sentences, plain words. No filler, no praise, no repeating what the user just said.
- Each message: what you found or did, then the one thing the user needs to do or decide. Details only if asked.
- Ask only what you can't find in the project yourself. One question at a time, with your best guess as the default answer.
- Answer what was asked, then stop.
- The report below is the one long output. Before and after it, keep messages to a few lines.

## Output format

Start the report with a summary table, one row per check, in checklist order, before any detail:

| # | Check | Verdict | Severity | Must fix before live? | Finding |
|---|-------|---------|----------|-----------------------|---------|

- Verdict starts with an icon: `✅ PASS`, `🔴 FAIL` (Critical or High), `🟡 FAIL` (Medium or Low), `❓ ASK ME`, `➖ N/A`.
- Severity: Critical (data leak, lost money, legal exposure), High, Medium, Low. Use "-" for PASS and N/A.
- Must fix before live?: Yes for Critical and High FAILs, and for any ASK ME that guards data or money. No for the rest.
- Finding: one plain-language sentence a non-developer understands: what you found and why it matters (for PASS, what makes it safe; for ASK ME, what to check and where). No file paths here; they go in the details.

Under the table give one line of counts, `✅ PASS n · 🔴 FAIL n · 🟡 FAIL n · ❓ ASK ME n · ➖ N/A n`, then a numbered "Fix before live" list of the "Yes" rows in the order to fix them.

Then the details, in this order, each with a `##` heading: `## 🔴 Fix first` (Critical and High FAILs), `## 🟡 Fix soon` (Medium and Low FAILs), `## ❓ Needs your input`, `## ✅ Passed`. Within a group, most severe first. Skip an empty group. Use this layout for every check:

- FAIL: `### 🔴 #5 Check name` (icon matches the table), then a one-line `**Severity:** Medium` and these labeled bullets: `**Evidence:**` (file, line, header or command output), `**Fix:**` (the smallest change, as a short numbered list if it has steps). Put code, commands and paths in backticks.
- ASK ME: `### ❓ #8 Check name`, then `**Why only you:**` (one line) and `**Where to look:**` (exact settings page or dashboard, as a short list).
- PASS: no heading. One bullet each under `## ✅ Passed`: `- **#4 HTTPS, one address**: the evidence in one line.`
- Report-style items the check asks for (inventories, runbooks, calendars, scripts) go in a table or numbered list under their check, not in prose.

End the report with one question: "Want this saved as an HTML report?" Do not create the file unless the user says yes. If they do, write one self-contained .html (inline CSS, no external scripts, light and dark via prefers-color-scheme, readable on a phone) with the same summary table, icons, counts, "Fix before live" list and detail groups, then tell them the path. Redact still applies, and the file may name internal paths, so remind them not to commit or share it publicly.

## Security

### Secrets live in a secret store, never in code
Keep .env in .gitignore and give production its keys through your host's environment settings. Search the repo and the built JavaScript for sk_, key=, secret, the Supabase service_role key, sb_secret_ keys, and any secret whose name starts with NEXT_PUBLIC_ or another public prefix. A public prefix copies the value into the browser bundle. Deleting the line does not help, because Git keeps every version: if a key was ever committed, rotate it.

Do: Search this repo, its full git history and the production JavaScript bundle for API keys, tokens and passwords. Include Supabase sb_secret_ keys and any secret stored under a public prefix such as NEXT_PUBLIC_. List every hit with file and commit. Don't change anything; tell me which keys I must rotate at the provider.

### Every permission is checked on the server
Hiding a button is not a permission. Sign in as user A, copy a URL or API call that belongs to user B, and try it. You should get a 403 or 404, never B's data.

Do: List every API route and server action in this app. For each one, show the exact line that checks the signed-in user may access that record. Flag any route that trusts an id from the browser without checking ownership.

### Database rules are on (Supabase RLS, Firebase rules)
If the browser talks to the database directly, the public key ships to everyone and the rules are the only lock. Every table needs row-level security with a policy per action. Storage is separate: a public bucket can be downloaded by anyone who can guess the URL, and storage policies are not the same as table policies.

Do: List every database table, whether row-level security is enabled, and each policy in plain English. Do the same for storage buckets: which are public, and which storage policies cover read, upload and delete. Then give me a test request I can run as a normal signed-in test user to verify it cannot read another user's rows or private files.

### HTTPS everywhere, one canonical address
Typing http:// or the address without www lands on the same https:// URL, with the padlock.

Do: Check my live site: does http:// redirect to https://, do www and the bare domain end up at one address, and when does the TLS certificate expire? Show the real response headers as evidence.

### Sign-up, login and contact forms are protected from bots
Rate limit per IP, add a CAPTCHA (Cloudflare Turnstile is free) on public forms, and verify emails before an account can do anything expensive.

Do: Find every public form and every endpoint that sends an email, calls a paid API, or creates an account. For each, tell me what stops a script from calling it 10,000 times, and propose the smallest rate limit or CAPTCHA to add.

### All user input is parameterized and validated
Forms, query strings, JSON bodies and file uploads. Queries are parameterized, output is escaped, uploads are limited by type and size. The browser's validation is a convenience, not a guard.

Do: For every API route and server action, list the fields it reads from the request and show the line where each is validated on the server. Flag raw SQL built from strings, HTML rendered from user input, and any upload without a type and size check.

### Dependency versions are current and audited, and the security headers are set
npm audit shows no critical issues and versions are pinned. The live site sends the security headers (Content-Security-Policy, X-Content-Type-Options, X-Frame-Options and HSTS): together they limit which scripts run, stop other sites framing yours and force HTTPS. CORS allows only your own domains, and production ships no source maps.

Do: Run the dependency audit for this project and list critical and high findings with the fix. Then fetch my live homepage and report which security headers are missing, what CORS allows, and whether source maps are served in production.

### Your own admin accounts have 2FA
GitHub, your host, your database, your domain registrar, your payment provider and your email. Whoever gets into one of these owns your app.

Do: Give me a checklist of every account this app depends on (hosting, database, domain, DNS, payments, email, AI providers), with the settings page where I turn on two-factor login for each.


## Data & Backups

2 more checks (Backups exist, and you've restored one; Never test against the production database). Sign in free to get them in this skill: https://www.vibetodev.com/ai-vibe-coding-website-launch-checklist

## Monitoring

4 more checks (Errors reach you, grouped, with a stack trace; Something outside checks the site every few minutes; You know how to roll back, and you've tried it; You can find the logs for a failed request). Sign in free to get them in this skill: https://www.vibetodev.com/ai-vibe-coding-website-launch-checklist

## Billing & Renewals

3 more checks (Every usage-billed service has a spending limit; Payments use live keys, and the webhook is verified; Domain auto-renew is on and the card on file is valid). Sign in free to get them in this skill: https://www.vibetodev.com/ai-vibe-coding-website-launch-checklist

## Legal & Trust

9 more checks (A privacy policy that matches what the app really collects; Forms ask only for what they need; Cookies and trackers wait for consent; A cookie policy that lists the real cookies; Terms of use and real business details are published; Consent is unticked, and marketing has its own checkbox; If you sell: refund, returns and shipping policies; No fake reviews, testimonials or numbers, and rights to every image; Accessibility basics pass). Sign in free to get them in this skill: https://www.vibetodev.com/ai-vibe-coding-website-launch-checklist

## Real-World Testing

4 more checks (It works on a real phone; Emails arrive in the inbox, not spam; A helpful 404 and error page; The site has a title, description and link preview). Sign in free to get them in this skill: https://www.vibetodev.com/ai-vibe-coding-website-launch-checklist
