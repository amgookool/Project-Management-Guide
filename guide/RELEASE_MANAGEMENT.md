# 🐬 Dolphin — Release Management

> **Audience:** Project Manager (you), with steps the dev team and QA execute.
> Dolphin ships **two fundamentally different kinds of releases**, and treating them the same is the #1 way to get burned:
>
> - **Web & APIs** — release *continuously*. Merging is shipping. Fast, reversible, fully in your control.
> - **Mobile** — release on its *own cadence*, gated by app-store review and slowed by force-update lag. You do **not** control the timeline once it leaves your hands.
>
> Do not try to force mobile into the 2-week sprint boundary. Plan around its reality.

---

## 1. Web & API Releases (Continuous, via CI/CD)

### How it works

GitHub Actions deploys automatically based on branch (see `ARCHITECTURE_CONTEXT.md` §3):

| Action | Result |
|--------|--------|
| Merge to `staging` | Auto-deploys to **staging** Firebase environment |
| Promote / merge to `main` | Auto-deploys to **production** Firebase environment |

So a web/API change reaches production the moment it lands on `main` — **independent of the sprint calendar.** A ticket finished on Day 3 can be live on Day 3.

### Release flow per ticket

```
PR approved → merge to staging → auto-deploy to staging
   → QA verifies on staging (ticket = QA / Testing)
      → QA passes → promote to main → auto-deploy to production
         → ticket = Done
```

### Pre-promotion checklist (before merging to `main`)

- [ ] PR reviewed and approved by ≥1 developer
- [ ] All CI checks (lint, tests, build) pass on the PR
- [ ] Deployed to **staging** and QA has signed off there
- [ ] Acceptance criteria verified on staging
- [ ] Any Firestore security-rule / data-shape changes confirmed safe for **all** consumers (Portal, other web, mobile)
- [ ] If the change touches an API contract → API versioning rules followed (`API_VERSIONING.md`)
- [ ] PM aware (so the weekly business update is accurate)

### Rollback

Because deploys are driven by `main`, rollback is a **git operation**: revert the offending commit(s) on `main` and let GitHub Actions redeploy the previous good state. For Firestore data changes, a revert of code does **not** undo data writes — call this out in the ticket's risk/rollback plan (see `TASK.md`).

### Release notes

The PM compiles release notes from Jira **Fix Version/s** (see `JIRA_SETUP.md` §8). Because web ships continuously, treat the **sprint version** as the unit of communication to the business — "here's everything that went live this sprint" — even though individual items shipped throughout the two weeks.

---

## 2. Mobile Releases (Flutter — Staged, Store-Gated)

Mobile is a different animal. You ship a **binary** to Apple and Google, they **review** it (hours to days, occasionally longer), and even after approval, **users choose when to update** — which is why we built **force update** into the app.

### 2.1 The mobile release pipeline

```
Develop on feature branch (DOL-[id]/...)
   → merge & build
      → TestFlight (iOS) / Play internal testing (Android)
         → selected internal testers + chosen users verify on real devices
            → submit to App Store / Play for review
               → store approves
                  → release to production (optionally phased/staged rollout)
                     → ticket = Done
```

> We currently use **TestFlight** to put builds in front of selected users before production. Mirror this on Android with **Google Play internal/closed testing** if you ship there.

### 2.2 Versioning & build numbers

Every mobile release carries two numbers — keep them straight:

| Number | Format | Changes when | Example |
|--------|--------|-------------|---------|
| **Version (semantic)** | `MAJOR.MINOR.PATCH` | User-facing release | `2.4.0` |
| **Build number** | integer, always increments | Every build uploaded to a store | `512` |

- `MAJOR` — breaking change or a release that **requires** force update
- `MINOR` — new features, backward compatible
- `PATCH` — bug fixes only
- Record the shipped version in the Jira **Fix Version/s** and in `#dolphin-releases`.

### 2.3 Force update — when and how

The app has a **built-in force update**. Use it deliberately — it's a hammer, not a default.

**Force an update when:**
- The new version depends on an **API change that is not backward compatible** with older app versions (see `API_VERSIONING.md` — prefer versioning the API over forcing updates).
- A **security** fix must reach every user immediately.
- A data-corruption or crash-on-launch bug exists in older versions.

**Do NOT force an update for:**
- Ordinary new features or cosmetic changes — let users update naturally.

**Mechanism:** maintain a **minimum supported version** (stored server-side, e.g., in Firestore / Remote Config). When a client below the minimum launches, it blocks with an "update required" screen. Bumping the minimum supported version is itself a **decision the PM signs off on** — record it in the release ticket.

### 2.4 Store review lead time — plan for it

- App-store review is **outside your control**. Budget **1–3 business days** (sometimes more) between "submitted" and "live."
- **Never promise the business a mobile feature on a specific date that assumes instant release.** Communicate "submitted to Apple/Google on X; expected live within a few business days of approval."
- Submit **early** in the sprint cycle if a mobile feature needs to be live by a deadline.

### 2.5 Phased rollout

For risky mobile releases, use the stores' **staged/phased rollout** (e.g., 10% → 50% → 100% over a few days) so a bad build reaches only a fraction of users before you halt it. Pair with a clear "halt rollout" trigger (crash-rate threshold).

### 2.6 Mobile "Done" is not "released"

A mobile ticket can be **code-complete and QA-passed on TestFlight** within a sprint, but **live in production days later** after store review. Decide per ticket what "Done" means and say it out loud:

- **Recommended:** mobile ticket = **Done** when QA has approved the build on TestFlight/internal testing **and** it has been submitted to the store. Track actual production release via the **Fix Version** and a release note in `#dolphin-releases`. This keeps the sprint honest without pretending you control Apple's queue.

---

## 3. Release Cadence — Side by Side

| | Web / APIs | Mobile |
|---|-----------|--------|
| **Trigger** | Merge to `main` | Submit build → store approval |
| **Cadence** | Continuous (any day) | Its own schedule; batch features per build |
| **Who gates it** | QA on staging | QA on TestFlight **+ Apple/Google review** |
| **Time to users** | Minutes | Hours to days |
| **Rollback** | Git revert → redeploy | Ship a new build (can't recall a release); halt phased rollout |
| **"Done" in Jira** | Promoted to `main` (= live) | QA-approved on TestFlight + submitted |
| **Old versions in the wild?** | No (everyone gets latest) | **Yes** — force update / min-version manages this |

---

## 4. Critical Hotfix — Per Platform

| Step | Web / API | Mobile |
|------|-----------|--------|
| 1 | Branch `hotfix/DOL-[id]/...` | Branch `hotfix/DOL-[id]/...` |
| 2 | PR + expedited review (1 approval) | PR + expedited review (1 approval) |
| 3 | Merge to `staging`, QA smoke-test | Build → TestFlight, QA smoke-test |
| 4 | Promote to `main` → live in minutes | Submit with **expedited review request** to Apple; staged rollout on Play |
| 5 | PM announces in `#dolphin-releases` | If users must get it now → bump **minimum supported version** to force update |

> For mobile, even a "hotfix" cannot bypass store review — that's why the **force-update minimum version** is your real emergency lever for the installed base.

---

## 5. Pre-Release Checklist (Both Platforms)

- [ ] All sprint tickets for this release in **Done**
- [ ] QA signed off (web: on staging / mobile: on TestFlight)
- [ ] Release notes written for the business team
- [ ] **Fix Version** set on every included ticket
- [ ] Rollback / halt-rollout plan identified
- [ ] Mobile only: store metadata, screenshots, and privacy declarations up to date
- [ ] Mobile only: decided whether this release forces an update (and updated min version if so)
- [ ] PM announces in `#dolphin-releases`
