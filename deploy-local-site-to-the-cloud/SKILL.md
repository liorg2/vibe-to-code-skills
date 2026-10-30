---
name: deploy-local-site-to-the-cloud
description: Publish your site. Take a site that runs on localhost and give it a real public address, with automatic deploys on every push. Use when the user says "deploy", "put my site online", "host this", "it only works on localhost", "go live", "get a public URL".
---

# Publish your site

From Vibe to Dev (https://www.vibetodev.com/). Guide with official links: https://www.vibetodev.com/guides/deploy-local-site-to-the-cloud
Time: 30–60 minutes. Cost: Free for a personal project.

Product names in this skill (hosts, databases, payment, email, error-tracking and AI tools) are examples only. First find out what this project actually uses, from its code, config and package files, and apply each step to that. If a step names a tool the project doesn't use, do the equivalent step for the tool it does use; if the user hasn't picked one yet, offer the options and let them choose. This skill works in any AI coding agent (Claude Code, Cursor, Codex, Copilot and others). If it names one tool's command, folder or file, use your agent's equivalent.

## Rules

- Inspect first, explain in plain language, one step at a time. Don't move to the next step until this one's "Done when" is true.
- Never create paid resources, push, deploy or submit without asking. The user does every account, payment and final-publish click.
- Each step is marked:
  - 👤 YOU: only the user can do this (account, payment, identity or a dashboard). Explain it, then wait.
  - 🤖 AI: you do this in the project, within the rules above.
  - 🤝 BOTH: you prepare or check; the user does the account or publish part.
- In step descriptions, "you" addresses the user; in "Task:" lines, "I" and "me" mean the user: report to them.

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

Never print a secret, key, password or token. Report variable names only, and write `<REDACTED>` in place of any value you have to show.

## How to talk

- Brief, friendly and professional. Short sentences, plain words. No filler, no praise, no repeating what the user just said.
- Each message: what you found or did, then the one thing the user needs to do or decide. Details only if asked.
- Ask only what you can't find in the project yourself. One question at a time, with your best guess as the default answer.
- Answer what was asked, then stop.

## Start

Inspect the repository and tell the user what framework, build command, environment variables, database and hosting requirements you find. For a Next.js app, Vercel is the short path, and Cloudflare Pages is only for a static export. Do not expose secret values, create paid resources, push to Git or deploy yet. Give a short plan and ask before any action that affects an external service.

## 1. Prove it builds on your machine

🤖 AI

Hosting runs a production build, not your dev server. If it fails here it fails there. Have the AI find this project's real production build command (for a Node app, the build script in package.json; npm run build is the usual one), run it, explain errors in plain language and fix them one at a time. If this build passes and the host build later fails, compare the Node version.

Task: Find and run this project's production build command. For a Node app, use the build script in package.json rather than assuming npm. If it fails, identify the first root cause, make the smallest fix and rerun it. Show me the final command and result. If it passes here and later fails on the host, compare the Node version.

```
npm run build
```

Done when: the project's real production build command finishes with no errors, and you showed the user the final command and output.

## 2. Keep secrets out of the code

🤝 BOTH

Ask the AI to check that .env, .env.local and key files are ignored and were never committed. (If your folder is not a Git repository yet, run git init first or check .gitignore directly.) It must report variable names only, never their values. Secrets go into the host's settings in step 6, not into GitHub. If a key was ever committed and pushed, treat it as leaked: create a new one at the provider and revoke the old one. Deleting the file does not take it back.

Task: Audit this repository for accidentally tracked secrets. Do not print secret values. Check ignore rules and Git history (if Git is initialized), then list only the file paths and variable names I need to address.

```
git status --ignored
git log --all -- .env .env.local
```

Done when: no secret file is tracked or in git history, or the user has rotated every leaked key at its provider.

## 3. Put the code on GitHub

👤 YOU

Create an empty private repository on GitHub, then connect your folder and push. Skip the first three lines if your folder is already a Git repository with commits on a branch called main.

```
git init
git add .
git commit -m "First version"
git branch -M main
git remote add origin https://github.com/<you>/<repo>.git
git push -u origin main
```

Done when: the code is on a private remote repository and the user sees the latest commit there.

## 4. Pick a host

👤 YOU

For a Next.js app, Vercel is the shortest path: sign up with your GitHub account. Its free Hobby plan is for non-commercial use; a paid plan is needed for a business. Netlify can host Next.js through its own adapter. On Cloudflare, a full Next.js app (server rendering and server actions) deploys to Workers with vinext, and Cloudflare Pages is only for a static export. Hostinger and DigitalOcean App Platform are also options, mostly on paid plans. A plain HTML site fits any of them.

Done when: the user has picked a host and signed in to it.

## 5. Move the database off your laptop

👤 YOU

Skip this if your site has no database. localhost means "this computer", so the live site cannot reach a database on your laptop. Before the first deploy, create a hosted one (for example Neon for Postgres), run your migrations against it and keep its address for the DATABASE_URL setting in the next step. Type that address into the host's settings yourself, not into the chat.

Done when: skip if there is no database; otherwise a hosted database exists, migrations ran on it, and its connection string is saved for the next step (not in chat).

## 6. Import the repository and add your settings

🤝 BOTH

In Vercel: Add New → Project → choose the repository. The framework is detected for you. Ask the AI which variable names are required, then copy their values yourself into Environment Variables (including DATABASE_URL from the step before) and press Deploy. Set each name for Production and for Preview. Names the framework copies into the browser, such as those starting with NEXT_PUBLIC_, are fixed at build time, so changing one means deploying again. Never paste secret values into the chat.

Task: Inspect the code and produce a deployment checklist of required environment-variable names, where each is used and whether it is needed at build time or runtime. Mark names the framework inlines into the browser, such as NEXT_PUBLIC_. Those are fixed at build time, so a change needs a new deploy. Tell me to set every name on both Production and Preview. Do not read or print their values.

Done when: you listed every required environment variable name, marked which are baked in at build time, and the user confirms each is set on the host for Production and for Preview.

## 7. Open it and read the logs

🤝 BOTH

You get an address like your-app.vercel.app. Click through the main journey. If something fails, copy the relevant redacted log lines to the AI and ask it to diagnose before changing code.

Task: If the build or runtime fails and I paste redacted deployment logs: explain the likely root cause, what evidence supports it and the smallest safe fix. Ask for any missing context instead of guessing.

Done when: the user opened the public URL and the main journey works; any errors are diagnosed from redacted logs.

## 8. Ship by pushing

👤 YOU

From now on every push to main deploys to the live site, and every other branch gets its own preview address. If a release breaks the site, use the host's rollback (on Vercel: Deployments → an earlier deployment → Instant Rollback / Promote to Production) to bring the last good version back while you fix it.

Done when: the user knows that a push to the main branch deploys, and where the host's rollback button is.

## Finish every session with a status report

| # | Step | Who | Status |
|---|------|-----|--------|

Status is Done, In progress, Waiting on you, or Not started. Under the table: the next step and exactly what the user must do for it, then anything blocked and why.
