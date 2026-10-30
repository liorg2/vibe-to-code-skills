---
name: publish-an-app-on-the-app-store
description: Publish your app on the Apple App Store. From an Expo app to the App Store without owning a Mac: developer program, cloud build, TestFlight, listing and App Review. Use when the user says "publish to the App Store", "iOS release", "TestFlight", "App Store Connect", "App Review rejected", "submit my iPhone app".
---

# Publish your app on the Apple App Store

From Vibe to Dev (https://www.vibetodev.com/). Guide with official links: https://www.vibetodev.com/guides/publish-an-app-on-the-app-store
Time: A day of work, plus enrolment (1–14 days) and review (often 1–2 days). Cost: $99 a year for the Apple Developer Program.

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

Inspect this Expo project and report its bundle ID (permanent once uploaded), build number (must rise on every build), permissions, privacy manifest, privacy-sensitive APIs, account flows including Sign in with Apple if another social login exists, icons, build configuration and likely review gaps. Do not enrol, pay, upload a build or submit anything. Cite file paths and separate verified facts from questions the user must answer.

## 1. Enrol in the Apple Developer Program

👤 YOU

Sign in with an Apple Account that has two-factor authentication, choose individual or organization (an organization needs a D-U-N-S number) and pay the yearly fee. The app is listed under the name you enrol with.

Done when: the Apple Developer Program membership is active.

## 2. Build in the cloud, no Mac needed

🤝 BOTH

EAS signs in to your Apple account and creates certificates and profiles, then builds on Apple hardware. The bundle ID you ship is permanent, and every later build needs a higher build number. (Setting ios.infoPlist.ITSAppUsesNonExemptEncryption to false in app.json if your app only uses standard HTTPS avoids Apple's export-compliance prompt on every build.) Ask the AI to validate configuration first; run the authenticated build yourself.

Task: Inspect app.json/app.config and eas.json for an App Store production build. Check the bundle ID, build number, permissions, export compliance (ITSAppUsesNonExemptEncryption), icons and production profile. The bundle ID cannot change after the first upload, and every later build needs a higher build number. Propose only required edits; do not run EAS or access an account.

```
npm install -g eas-cli
eas login
eas build:configure
eas build --platform ios --profile production
```

Done when: you validated the config, including a bundle ID that can stay forever and a build number higher than any build already uploaded, and the user's cloud build finished successfully.

## 3. Create the app record in App Store Connect

👤 YOU

My Apps → + → New App: platform, name (must be unique on the store), language, bundle ID (choose the bundle ID that Step 2 registered, matching app.json) and a SKU of your choice.

Done when: the app record exists in App Store Connect with the same bundle ID as the project.

## 4. Upload the build

👤 YOU

The build appears in App Store Connect after Apple finishes processing it, usually within the hour.

```
eas submit --platform ios
```

Done when: the build shows as processed in App Store Connect.

## 5. Test with TestFlight

👤 YOU

Add yourself and up to 100 people on your team as internal testers right away. Outside testers (by email or a public link) need a short beta review first. Install the TestFlight app on the phone and run the real build.

Done when: the user installed the build through TestFlight on a real phone and the main journey works.

## 6. Fill in the listing and privacy details

🤝 BOTH

Ask the AI to draft description and keywords from real features and inventory likely data collection from code and SDKs. You provide screenshots, URLs, age rating and final App Privacy answers after checking them.

Task: Draft App Store description and keywords using only verified features. Then create a separate privacy-question checklist from the code and dependencies, citing evidence and marking uncertainty instead of guessing.

Done when: the description, keywords, screenshots, URLs, age rating and App Privacy answers are filled in and checked by the user.

## 7. Prepare for App Review

🤝 BOTH

Give reviewers a demo account if there is sign-in. If the app offers Google or any other social login, it also needs Sign in with Apple, using Apple's own button. Account deletion must delete the account, not only hide it. The build needs a privacy manifest for Apple's required-reason APIs (in Expo, the ios.privacyManifests setting). Ask the AI to audit common rejection risks (crashes, placeholder content, thin web wrappers and missing account deletion), then test each finding yourself. Read the current guidelines before submitting.

Task: Audit this app for common App Review blockers. Check sign-in, including Sign in with Apple beside any other social login and using Apple's button, account deletion that really deletes the account, the privacy manifest for required-reason APIs, placeholder content, broken links, permissions and web-wrapper behavior. Return a prioritized checklist with file paths and manual tests.

Done when: every review blocker on your list is fixed or tested by the user, and a demo account is ready if there is sign-in.

## 8. Submit and release

👤 YOU

Add the build to the version and press Submit for Review. Before that, accept the latest agreement under Agreements, Tax, and Banking. Set Digital Services Act trader status, or the app stays unavailable in the EU. Choose to release automatically after approval or by hand. If rejected, the message in Resolution Center says which guideline; fix it and resubmit.

Done when: the latest agreement is accepted, EU trader status is set or the user has chosen to skip the EU for now, and the version is submitted for review, or approved and released.

## Finish every session with a status report

| # | Step | Who | Status |
|---|------|-----|--------|

Status is Done, In progress, Waiting on you, or Not started. Under the table: the next step and exactly what the user must do for it, then anything blocked and why.
