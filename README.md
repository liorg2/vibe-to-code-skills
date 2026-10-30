# vibe-to-code-skills

Agent Skills (`SKILL.md`) from [Vibe to Dev](https://www.vibetodev.com): the pack on [/claude-skills](https://www.vibetodev.com/claude-skills), plus the lab end-of-step skill. They work in Claude Code, Cursor, Codex and any agent that reads `SKILL.md`.

| Folder | What it does |
| --- | --- |
| `launch-check` | 30-item pre-launch checklist (full + signed-out `SKILL.public` cuts) |
| `deploy-local-site-to-the-cloud` | Take a localhost site to a public URL with automatic deploys |
| `buy-a-domain-and-set-up-email` | Buy a domain, point DNS/HTTPS, and set up email on it |
| `publish-an-app-on-google-play` | Expo → Google Play |
| `publish-an-app-on-the-app-store` | Expo → App Store |
| `lab-check` | End-of-step `/lab-check` routine for the app labs |

## Install

**One command** (any agent, via [skills.sh](https://skills.sh)), run in your project folder:

```bash
npx skills add liorg2/vibe-to-code-skills
```

**By hand:** copy a skill's folder into your project, keeping only the `SKILL.md` you want (English `SKILL.md`, Hebrew `SKILL.he.md` renamed to `SKILL.md`):

| Agent | Put it at |
| --- | --- |
| Claude Code | `.claude/skills/<name>/SKILL.md` (or `~/.claude/skills/` for every project) |
| Cursor | `.cursor/skills/<name>/SKILL.md` |
| Codex and others | `.agents/skills/<name>/SKILL.md` |

No agent support for skills? Paste the file into the chat.

## Use

Ask in plain words, for example "run the launch check" or "put my site online". `launch-check` only runs when you ask for it by name. Skills read your project and report first; they ask before anything that costs money, pushes or publishes.

## Files

English files are `SKILL.md`. Hebrew files are `SKILL.he.md`. Launch-check also ships `SKILL.public.md` and `SKILL.public.he.md` for signed-out visitors.

The site repo pulls this repository's **`main`** branch into `data/skills` on install, `dev`, and `build`. Edit skills here; do not keep a second copy in the site or labs repos.

## Contributing

Issues and pull requests are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).
