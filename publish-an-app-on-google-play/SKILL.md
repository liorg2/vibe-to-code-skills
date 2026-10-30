---
name: publish-an-app-on-google-play
description: Publish your app on Google Play. From an Expo app on your phone to a listing on Google Play: developer account, build, testing track, store listing and review. Use when the user says "publish to Google Play", "Play Store", "Android release", "upload my app", ".aab", "closed testing", "Data safety form", "Play Console".
---

# Publish your app on Google Play

From Vibe to Dev (https://www.vibetodev.com/). Guide with official links: https://www.vibetodev.com/guides/publish-an-app-on-google-play
Time: A few hours of work, then at least two weeks of testing for a new personal account. Cost: One-time $25 registration fee.

Product names in this skill (hosts, databases, payment, email, error-tracking and AI tools) are examples only. First find out what this project actually uses, from its code, config and package files, and apply each step to that. If a step names a tool the project doesn't use, do the equivalent step for the tool it does use; if the user hasn't picked one yet, offer the options and let them choose. This skill works in any AI coding agent (Claude Code, Cursor, Codex, Copilot and others). If it names one tool's command, folder or file, use your agent's equivalent.

## Rules

- Inspect first, explain in plain language, one step at a time. Don't move to the next step until this one's "Done when" is true.
- Never create paid resources, push, deploy or submit without asking. The user does every account, payment and final-publish click.
- Each step is marked:
  - 👤 YOU: only the user can do this (account, payment, identity or a dashboard). Explain it, then wait.
  - 🤖 AI: you do this in the project, within the rules above.
  - 🤝 BOTH: you prepare or check; the user does the account or publish part.
- In step descriptions, "you" addresses the user; in "Task:" lines, "I" and "me" mean the user: report to them.

## Redact

Never print a secret, key, password or token. Report variable names only, and write `<REDACTED>` in place of any value you have to show.

## How to talk

- Brief, friendly and professional. Short sentences, plain words. No filler, no praise, no repeating what the user just said.
- Each message: what you found or did, then the one thing the user needs to do or decide. Details only if asked.
- Ask only what you can't find in the project yourself. One question at a time, with your best guess as the default answer.
- Answer what was asked, then stop.

## Start

Inspect this Expo project and report its package ID (permanent once uploaded), version code (must rise on every upload), permissions, data collection, icons, build configuration and likely policy gaps. Do not create accounts, pay fees, upload builds or submit forms. Make a checklist backed by file paths, separating facts you found from questions the user must answer.

## 1. Create a Play Console developer account

👤 YOU

Pay the one-time fee and verify your identity with an official ID. Choose personal or organization; an organization account needs a D-U-N-S number. Verification can take a few days.

Done when: the Play Console account exists and identity verification is approved.

## 2. Know the testing rule before you start

👤 YOU

New personal accounts created after November 13, 2023 must run a closed test with at least 12 testers who have stayed opted in for the 14 days right before you apply. If a tester opts out, their 14 days start over, and you still need 12 opted in on the day you apply. Meeting that bar lets you apply; Google still reviews the app. Line up friends with Android phones now. Check the current rule on Google's page; it has changed before.

Done when: the user has at least 12 testers lined up, knows that an opt-out restarts that tester's 14 days, and has read Google's current testing rule.

## 3. Build the Android bundle with EAS

🤝 BOTH

Google Play takes an .aab file. The package name you ship is permanent. Each new upload needs a higher version code than the one already on Play, or Play rejects the file. First ask the AI to inspect and validate the Expo configuration. Then run the account and cloud-build commands yourself; they can upload code and use paid service quotas.

Task: Inspect app.json/app.config and eas.json for a Google Play production build. Check the package ID, version code, permissions, icons and production profile. The package name cannot change after the first upload, and every later upload needs a higher version code. Propose the smallest required edits, but do not run EAS or access an account.

```
npm install -g eas-cli
eas login
eas build:configure
eas build --platform android --profile production
```

Done when: you validated the config, including a package name that can stay forever and a version code higher than any build already on Play, and the user ran the cloud build and has a .aab file.

## 4. Create the app in Play Console

👤 YOU

Create app → name, default language, app or game, free or paid. Free cannot become paid later. A free app can still sell in-app purchases.

Done when: the app exists in Play Console with its name, language and free/paid choice.

## 5. Fill in App content

🤝 BOTH

A privacy policy URL, Data safety form, content rating, target audience, ads declaration and a test login if needed. If people can create an account, Google requires a way to delete it both inside the app and on a web page, and the Data safety form asks for that page's address. Let the AI inventory SDKs and code paths, but verify every answer yourself: policy declarations are your responsibility.

Task: Create a draft Data safety inventory from the dependencies and code. For every data type, cite the file or SDK that suggests it and mark uncertain items as questions. Do not decide the final legal declaration for me.

Done when: every App content section in Play Console shows complete, each answer checked by the user.

## 6. Write the store listing

🤝 BOTH

Ask the AI to draft honest listing copy from the features that actually exist. You still need a 512×512 icon, a 1024×500 feature graphic and at least two real phone screenshots, not mockups of missing features.

Task: Draft an 80-character short description and a clear full Google Play description using only features you can verify in this repository. List every claim with the file that supports it, and omit unsupported promises.

Done when: the listing has a short and full description, the icon, the feature graphic and at least two real screenshots.

## 7. Upload to a closed testing track

👤 YOU

Testing → Closed testing → create a release and upload the .aab by hand in the console. Add your testers' emails, save the list, and send them the opt-in link (note: the opt-in link only works after your first closed-testing release finishes Google's initial review). To upload from your terminal instead, create a Google service-account key and give it access in Play Console (the Expo submit page walks through it). By default eas submit puts the build on the internal testing track, which is not the closed test that counts toward production access. To target the closed test, set "track": "alpha" in the android part of your eas.json submit profile, or upload that build by hand.

```
eas submit --platform android
```

Done when: the build is on the closed testing track (not internal) and testers have opted in.

## 8. Apply for production and release

👤 YOU

After the 14 days, answer Google's questions about your test and apply for production access. Review usually takes a few days. Then roll out, starting with a percentage of users if you want a safety margin.

Done when: the test ran 14 days, production access is approved, and the release is rolling out.

## Finish every session with a status report

| # | Step | Who | Status |
|---|------|-----|--------|

Status is Done, In progress, Waiting on you, or Not started. Under the table: the next step and exactly what the user must do for it, then anything blocked and why.
