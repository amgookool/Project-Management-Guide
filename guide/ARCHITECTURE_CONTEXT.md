# 🐬 Dolphin — System & Delivery Context

> **Audience:** Project Manager (you) — and anyone who needs to understand *what* Dolphin is shipping and *how* it reaches production.
> This is not an engineering deep-dive. It's the map a PM needs to write good tickets, sequence work correctly, and talk credibly with the team and the business.

---

## 1. What We Build

Dolphin is **not a single app** — it's a small portfolio of products that share one backend. Knowing which product a ticket touches drives the `Affected Product` field, the labels, who's assigned, and how it gets released.

| Product | What It Is | Tech | Lives In |
|---------|-----------|------|----------|
| **Portal** | The main web application — a single-page app | SvelteKit (SPA) | TypeScript monorepo |
| **Landing Page** | Marketing / public-facing site | Web (in monorepo) | TypeScript monorepo |
| **Other web projects** | Additional web frontends | Web (in monorepo) | TypeScript monorepo |
| **Web APIs** | One API per web app — backs Portal and the other frontends | Google Cloud Functions + Express | TypeScript monorepo (each API is its own project) |
| **Integration APIs** | APIs that connect Dolphin to **external third parties** | Google Cloud Functions + Express | TypeScript monorepo (each integration is its own project) |
| **Mobile App** | The customer mobile app | Flutter (iOS + Android) | **Separate repo** (migration in progress — see §4) |

### Backend: Firebase

All products sit on top of **Firebase** as the backend service:

| Firebase Service | Used For |
|------------------|---------|
| **Firestore** | Primary database (NoSQL document store) |
| **Firebase Auth** | User authentication across web and mobile |
| **Firebase Storage** | File / media storage |
| **Cloud Functions** | Hosts the Web APIs and Integration APIs (Express apps) |

> 💡 **Why this matters to the PM:** Firestore is schemaless, so "database changes" rarely mean migrations — but they *do* mean security-rule changes and data-shape coordination across web + mobile. Tag those tickets `database` and flag them as cross-product (see §5).

---

## 2. Repositories

| Repo | Contains | Language |
|------|----------|----------|
| **TypeScript monorepo** | Portal, landing page, other web projects, all Web APIs, all Integration APIs | TypeScript |
| **Flutter mobile repo** | The mobile app (being split out — see §4) | Dart / Flutter |

> The same Jira ticket key (`DOL-[id]`) is used across **both** repos. A single Epic can have stories whose branches live in different repos — that's expected for any feature that spans web/API and mobile.

---

## 3. Environments & CI/CD

Deployment is **automated via GitHub Actions** and driven by which branch code lands on. There is no manual "deploy at sprint end" step — **merging is shipping.**

| Branch | GitHub Actions deploys to | Firebase Environment | Audience |
|--------|--------------------------|---------------------|----------|
| `staging` | Staging | Firebase (staging project) | QA + internal testers |
| `main` | **Production** | Firebase (production project) | Real users |

```
feature branch (DOL-[id]/...)
        │  PR + review
        ▼
   merge to  staging  ──▶ auto-deploy ──▶ STAGING ──▶ QA verifies here
        │
        │  QA passes → promote
        ▼
   merge to  main     ──▶ auto-deploy ──▶ PRODUCTION ──▶ live
```

> ⚠️ **This is the single most important correction to the old workflow.** Earlier docs implied QA happens *after* merging to `main`. It does not — `main` is production. **QA happens on `staging`, before promotion to `main`.** The Jira workflow and automation (see `JIRA_SETUP.md`) reflect this:
> - Merge to `staging` → ticket moves to **QA / Testing**
> - Promote to `main` (production) → ticket moves to **Done**

> 🔎 **Confirm with the team:** This document assumes a **feature → `staging` → `main` promotion** model (feature branches merge into `staging` first; once QA passes, the change is promoted to `main`). If your team instead targets `main` directly and cherry-picks to `staging`, tell the PM so this doc and the Jira automation can be adjusted.

---

## 4. Mobile Repo Migration (In Progress)

The Flutter mobile app currently lives in the monorepo but is **being migrated to its own repository** because Dart/Flutter does not align with the TypeScript monorepo tooling.

**PM implications during and after the migration:**
- Track the migration itself as a **Task** (label `infrastructure`, `mobile`) under an Epic, with a clear rollback point.
- Until it's done, mobile branches may exist in the monorepo; after, they live in the new repo. Branch naming (`DOL-[id]/...`) is unchanged.
- CI/CD for the mobile repo is **separate** from the web/API pipeline — mobile does not deploy to Firebase Hosting; it ships to the app stores (see `RELEASE_MANAGEMENT.md`).

---

## 5. Cross-Product Sequencing (Read This Before Every Sprint)

The portfolio shape means most non-trivial features touch **more than one product in a required order.** The PM's job in refinement and planning is to get the order right so nobody is blocked.

**The golden sequence — build the contract first, consumers second:**

```
1. API / Firestore change  (Backend Dev)      ← must land first
        │  endpoint + data shape available on staging
        ▼
2. Web consumers (Portal/etc.)  +  Mobile consumer (Flutter)   ← can proceed in parallel once the API is stable
```

Practical rules:
- **Never schedule a web/mobile story that consumes a new API endpoint in the same half of the sprint as the API story unless the API lands first.** Split into sub-tasks and sequence them.
- **Mobile lags web on breaking changes.** Because of force-update lag, an old version of the mobile app is always in users' hands. A breaking API change must follow the deprecation policy in `API_VERSIONING.md` — you cannot just change the contract and expect mobile to keep up.
- **Firestore security-rule and data-shape changes are cross-product by default.** A field rename in Firestore can break Portal *and* mobile at once. Tag `database`, flag in refinement, and identify every consumer before estimating.

> Use Epic **swimlanes** on the board (configured in `JIRA_SETUP.md`) to see, at a glance, how many tickets per feature are in flight across products — and spot when a consumer is moving ahead of its API.

---

## 6. Quick Reference — Product → Label → Affected Product

| Work touches… | Labels | `Affected Product` value |
|---------------|--------|--------------------------|
| Portal UI | `frontend`, `web`, `portal` | Portal (Web) |
| Landing page | `frontend`, `web` | Landing Page |
| A Web API | `backend`, `api` | Web API |
| An Integration API | `backend`, `api`, `integration` | Integration API |
| Mobile feature | `frontend`, `mobile`, `flutter` | Mobile App |
| Firestore / auth / storage | `backend`, `database`, `firebase` | (the consuming product[s]) |
| CI/CD, GitHub Actions, Firebase config | `infrastructure` | (as applicable) |

See `JIRA_SETUP.md` for the full label list and how to configure the `Affected Product` field.
