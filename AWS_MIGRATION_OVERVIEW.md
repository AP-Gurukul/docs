# AWS Migration — plain-English overview

> Written for anyone (technical or not) who needs to understand what changed,
> why, and where things stand. This is the "start here" document for the
> Firebase → AWS migration; each repo has its own deeper how-to guides linked
> below.

## The one-paragraph version
The whole platform used to run on Firebase. It's moving to a single shared
AWS backend instead: one Express server (**`backend`** repo) that talks to
one database (**AWS DynamoDB**), that every client — the **admin portal**,
the **mobile app**, and in future the web/Telegram apps — connects to using
the same login system (**AWS Cognito**). No app ever touches the database
directly; everything goes through that one server.

```
   admin-portal        mobile app (apgurukul)        web / telegram (future)
        │                       │                          │
        │        HTTPS request + Cognito login token       │
        └───────────────────────┼──────────────────────────┘
                                 ▼
                     ┌───────────────────────┐
                     │   backend (Express)    │ ← the ONE server
                     └───────────┬────────────┘
                                 ▼
                     ┌───────────────────────┐
                     │   AWS DynamoDB          │ ← the ONE database
                     └───────────────────────┘
```

## Why
- **One place for the rules.** Data-access rules used to be scattered (or, in
  the mobile app's case, non-existent — it talked to Firebase directly).
  Now every rule about who can read/write what lives in one file
  (`backend/src/auth/rules.ts`), enforced for every client the same way.
- **No leaked credentials.** No app ships an AWS key or a database
  connection string. Every client authenticates with a normal login token;
  the server holds the only real credentials.
- **One database, one truth.** The admin portal and the mobile app now see
  the exact same data, always — because they're reading and writing the same
  place, through the same gate.

## Status by phase

| Phase | What | Status |
|---|---|---|
| 1 | Backend can read/write the shared DynamoDB database, speaking the same "language" the admin portal already used | ✅ Done |
| 2 | Normal app users (not just admins) can safely read/write **their own** data only | ✅ Done |
| 3 | Security hardening (rate limits, an unbreakable audit log, automated secret/vulnerability scanning on every code change) | ✅ Done |
| 4 | Mobile app fully switched to the new backend: login, payments, profile-photo upload | ✅ Code done — a few real-world setup steps remain (see below) |
| 5 | Actually put the backend on the internet (AWS App Runner) | 📝 Documented, not yet done — this is a deliberate step with a small ongoing cost |
| 6 | This document + final review | ✅ You're reading it |

## What's genuinely still outstanding (needs a human, not more code)
1. **Rotate an old AWS key** that was sitting in a local config file
   (flagged early in this work; not shipped in the app, but should still be
   replaced on general security principle).
2. **Migrate the ~5 real existing user accounts** onto the new login system.
   A ready-to-run script exists (`backend/scripts/migrateFirebaseUsers.ts`)
   with a full step-by-step guide (`backend/docs/USER_MIGRATION_GUIDE.md`).
   It has **not been run for real yet** — only a safe "dry run" (which
   changes nothing) has been done, and it surfaced one real duplicate-account
   issue to resolve first.
3. **Deploy the backend** to AWS App Runner
   (`backend/docs/DEPLOYMENT_GUIDE.md`) — a small ongoing cost, a deliberate
   go-live moment.
4. **Point the app and (eventually) the admin portal** at the deployed
   backend URL — kept as a clearly separate last step in the deployment
   guide, so it can't happen by accident.

## Where to go for detail
| Topic | Repo | Doc |
|---|---|---|
| How the pieces fit together | `backend` | `docs/ARCHITECTURE.md` |
| Granting AWS database access | `backend` | `docs/BACKEND_AWS_ACCESS.md` |
| Granting AWS image-storage access | `backend` | `docs/BACKEND_S3_ACCESS.md` |
| Testing with your own real login | `backend` | `docs/PHASE3_TESTING_GUIDE.md` |
| Migrating the existing 5 users | `backend` | `docs/USER_MIGRATION_GUIDE.md` |
| Deploying the backend for real | `backend` | `docs/DEPLOYMENT_GUIDE.md` |
| Mobile app migration checklist | `Ap gurukul app/apgurukul` (repo: `mobile`) | `AWS_APP_MIGRATION_PROGRESS.md` |

## A note on the other "mobile-app" repo
This org also has a separate `mobile-app` repository (still on Firebase, a
different codebase) referenced elsewhere in these docs — that is **not** the
app this migration covers. The app described here is the `mobile` repo
(local folder `Ap gurukul app/apgurukul`). If these two apps are meant to be
reconciled or retired, that's a separate decision this document doesn't make.
