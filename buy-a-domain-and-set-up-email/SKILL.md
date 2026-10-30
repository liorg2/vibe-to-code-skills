---
name: buy-a-domain-and-set-up-email
description: Buy a domain and get email on it. Buy yourname.com, point it at your site with HTTPS, and send and receive email as hello@yourname.com without landing in spam. Use when the user says "buy a domain", "custom domain", "connect my domain", "email on my domain", "hello@mydomain", "emails go to spam", "MX records", "SPF DKIM DMARC".
---

# Buy a domain and get email on it

From Vibe to Dev (https://www.vibetodev.com/). Guide with official links: https://www.vibetodev.com/guides/buy-a-domain-and-set-up-email
Time: 1 hour, plus up to a day for DNS. Cost: About $10–20 a year for a .com; email from free (forwarding) to a few dollars a month per mailbox.

Product names in this skill (hosts, databases, payment, email, error-tracking and AI tools) are examples only. First find out what this project actually uses, from its code, config and package files, and apply each step to that. If a step names a tool the project doesn't use, do the equivalent step for the tool it does use; if the user hasn't picked one yet, offer the options and let them choose. This skill works in any AI coding agent (Claude Code, Cursor, Codex, Copilot and others). If it names one tool's command, folder or file, use your agent's equivalent.

## Rules

- Inspect first, explain in plain language, one step at a time. Don't move to the next step until this one's "Done when" is true.
- Never create paid resources, push, deploy or submit without asking. The user does every account, payment and final-publish click.
- Each step is marked:
  - 👤 YOU: only the user can do this (account, payment, identity or a dashboard). Explain it, then wait.
  - 🤖 AI: you do this in the project, within the rules above.
  - 🤝 BOTH: you prepare or check; the user does the account or publish part.
- In step descriptions, "you" addresses the user; in "Task:" lines, "I" and "me" mean the user: report to them.

## Custom domain settings by host

Do not assume Vercel. First find which host this project actually uses from its config files or response headers, or ask the user:

| Host | Where to add a custom domain |
|---|---|
| Vercel | Project → Settings → Domains |
| Netlify | Site configuration → Domain management |
| Cloudflare Pages / Workers | Custom domains tab on the Pages project or Worker |
| Render, Railway, Fly.io | Render: Settings → Custom Domains. Railway: Service → Settings → Networking. Fly: `fly certs add` |

## Redact

Never print a secret, key, password or token. Report variable names only, and write `<REDACTED>` in place of any value you have to show.

## How to talk

- Brief, friendly and professional. Short sentences, plain words. No filler, no praise, no repeating what the user just said.
- Each message: what you found or did, then the one thing the user needs to do or decide. Details only if asked.
- Ask only what you can't find in the project yourself. One question at a time, with your best guess as the default answer.
- Answer what was asked, then stop.

## Start

Inspect the repository and identify the current hosting provider, production URL, email-related code and any existing domain assumptions. Forwarding only receives; sending as the domain needs a real mailbox. Do not buy anything, sign in to accounts or change DNS. Give the user a provider-neutral checklist and say exactly what they must verify by hand.

## 1. Choose the name

🤝 BOTH

Short, easy to say out loud, no hyphens. Prefer .com, or a country ending if your customers are local. Let the AI generate candidates and catch confusing spellings, then verify availability and trademarks yourself.

Task: Suggest 20 short domain names for this product based on its README and audience. Avoid hyphens and hard-to-spell words. Group them by naming idea and flag possible confusion; do not claim any name is available.

Done when: the user has picked a name and checked it is available and not someone's trademark.

## 2. Buy it at a registrar

👤 YOU

Cloudflare Registrar sells at cost with no markup; Namecheap and Porkbun are popular too. Compare the renewal price, not the first-year price. Turn on auto-renew, WHOIS privacy (usually free) and two-factor sign-in on the registrar account: whoever holds this login owns your domain. One catch: a domain bought at Cloudflare must keep Cloudflare as its DNS, so in the next step you add records there instead of switching nameservers.

Done when: the domain is bought, with auto-renew, WHOIS privacy and two-factor sign-in turned on.

## 3. Point it at your site

🤝 BOTH

In your host (Vercel: Project → Settings → Domains) add yourname.com and www.yourname.com. It shows the exact DNS records to create at your registrar. (If your DNS is on Cloudflare, set those records to "DNS only" / grey cloud while your host verifies the domain and issues its certificate.) Give the AI the record names and types (never your account login) and ask it to sanity-check them before you save. HTTPS is issued once DNS is live.

Task: Review this proposed DNS record list for conflicts, duplicate records and common mistakes. Explain what each record does. Do not invent missing values; mark anything I must copy from my host.

Done when: both the bare domain and www load the site over HTTPS.

## 4. Decide what kind of email you need

👤 YOU

Only receiving? Free forwarding (Cloudflare Email Routing, which needs your DNS on Cloudflare, or your registrar's forwarding) sends hello@yourname.com to your existing Gmail. Forwarding only receives: a reply from Gmail still goes out as your Gmail address, and Cloudflare Email Routing cannot send. Need to send as hello@yourname.com? Google Workspace or Zoho Mail. Pick one provider: a domain should have one set of MX records.

Done when: the user has chosen forwarding or real mailboxes, and one provider. If they need to send as the domain, they chose a real mailbox, not forwarding.

## 5. Verify the domain and add the MX records

👤 YOU

The email provider gives you a TXT record to prove you own the domain and MX records that route incoming mail to it. Add them exactly as shown at whoever runs your DNS, then press verify in the provider.

Done when: the provider shows the domain as verified and MX records are in DNS.

## 6. Add SPF, DKIM and DMARC so you do not land in spam

🤝 BOTH

SPF lists who may send for your domain (exactly one SPF record, merge if you have two). Each include in that record counts toward a limit of 10 DNS lookups, and past that limit receivers reject the mail. DKIM is a signature key your provider generates. DMARC tells receivers what to do with failures; start with monitoring only. Ask the AI to review records, but copy the exact provider values yourself.

Task: Check these SPF, DKIM and DMARC records for syntax errors, duplicates, and an SPF include chain over 10 DNS lookups. Explain any change. Do not replace provider-specific values with guesses.

```
TXT  @        v=spf1 include:_spf.google.com ~all      (Google example)
TXT  google._domainkey   v=DKIM1; k=rsa; p=…  (copy from the admin console)
TXT  _dmarc   v=DMARC1; p=none; rua=mailto:you@yourname.com
```

Done when: exactly one SPF record, a DKIM record and a DMARC record exist, with the provider's exact values, and the SPF include chain is within 10 DNS lookups.

## 7. Test it

👤 YOU

If you set up a full mailbox or an app sending service, send a message to a Gmail address, open it, choose Show original, and look for SPF: PASS, DKIM: PASS and DMARC: PASS. mail-tester.com gives a score and tells what is missing. If you only set up free forwarding, send an email from another address to hello@yourname.com and confirm it lands in your inbox.

Done when: a test email to Gmail shows SPF, DKIM and DMARC all PASS (or, for forwarding-only, an incoming test email arrives in your inbox).

## 8. Email from your app is separate

🤝 BOTH

Password resets and receipts sent by your app should go through a sending service (Resend, Postmark) on a subdomain like mail.yourname.com, with its own SPF and DKIM records. That keeps a bulk-sending problem from hurting your personal mailbox.

Done when: skip if the app sends no email; otherwise app email goes through a sending service on a subdomain with its own SPF and DKIM.

## Finish every session with a status report

| # | Step | Who | Status |
|---|------|-----|--------|

Status is Done, In progress, Waiting on you, or Not started. Under the table: the next step and exactly what the user must do for it, then anything blocked and why.
