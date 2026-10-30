# vibe-to-code-skills

Agent Skills (`SKILL.md`) for [Vibe to Code](https://vibetodev.com): the downloadable pack on `/claude-skills`, plus the lab end-of-step skill.

| Folder | What it does |
| --- | --- |
| `launch-check` | 30-item pre-launch checklist (full + signed-out `SKILL.public` cuts) |
| `deploy-local-site-to-the-cloud` | Take a localhost site to a public URL with automatic deploys |
| `buy-a-domain-and-set-up-email` | Buy a domain, point DNS/HTTPS, and set up email on it |
| `publish-an-app-on-google-play` | Expo → Google Play |
| `publish-an-app-on-the-app-store` | Expo → App Store |
| `lab-check` | End-of-step `/lab-check` routine for the app labs |

English files are `SKILL.md`. Hebrew files are `SKILL.he.md`. Launch-check also ships `SKILL.public.md` and `SKILL.public.he.md` for signed-out visitors.

The site repo pulls this repository's **`main`** branch into `data/skills` on install, `dev`, and `build`. Edit skills here; do not keep a second copy in the site or labs repos.
