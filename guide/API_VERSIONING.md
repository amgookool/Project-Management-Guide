# 🐬 Dolphin — API Versioning, Breaking-Change & Deprecation Policy

> **Audience:** PM + Backend/Fullstack devs.
> **Applies to:** All Dolphin APIs built on Google Cloud Functions + Express — both the **Web APIs** (one per web app) and the **Integration APIs** (third-party-facing).
>
> **Why this policy exists:** An API is a *promise* to whoever calls it. We have two kinds of callers we can't update in lockstep:
> 1. **The mobile app** — because of store review + force-update lag, **old app versions are always live**, calling older expectations of our APIs.
> 2. **External integration partners** — we don't control their release schedule at all.
>
> Break that promise without a process and you take down a partner integration or every user on an old app build. This policy makes breaking changes **safe, announced, and scheduled.**

---

## 1. Two Tiers of API — Different Rules

| Tier | Consumers | Versioning Rigor |
|------|-----------|------------------|
| **External / Integration APIs** & **any API the mobile app calls** | Third parties + installed mobile app versions we can't force-update instantly | **Strict.** Full versioning + deprecation window required. |
| **Internal Web APIs** consumed *only* by a web app we deploy in lockstep | Portal / landing / other web (deployed together via CI/CD) | **Lighter.** Still version, but a coordinated same-deploy change is acceptable because consumer + API ship together. |

> 🔑 **Decision rule:** *Can every consumer of this endpoint be updated at the same instant the API changes?*
> - **Yes** (web SPA + its own API, same deploy) → a coordinated change is OK.
> - **No** (mobile, external partner) → **strict versioning applies. Never break it; version it.**

---

## 2. Versioning Scheme

**URL-path versioning.** Every API is namespaced by a major version in the path:

```
https://<region>-<project>.cloudfunctions.net/<api>/v1/resource
https://api.dolphin.example/v1/users/123          (if behind a custom domain / gateway)
```

Rules:
- The version is the **major** version only: `/v1`, `/v2`. Minor/patch changes do **not** change the URL.
- A new **major version** is created **only** for a breaking change (see §3).
- Old major versions keep running until formally sunset (see §5).
- Express apps expose each version as its own router (`/v1`, `/v2`) so versions can coexist in one Cloud Function or be split per function.

> 🔎 **Confirm with the team:** URL-path versioning is the documented default because it's the most explicit and the easiest for external partners and mobile to reason about. If the team prefers header-based (`Accept-Version`) or query-param versioning, raise it with the PM before this becomes the standard — but pick **one** and apply it everywhere.

---

## 3. What Counts as a Breaking Change

A change is **breaking** if an existing, untouched client could start failing or behaving differently. When in doubt, treat it as breaking.

| ❌ Breaking (needs a new major version + deprecation) | ✅ Non-breaking (safe within the same version) |
|------------------------------------------------------|------------------------------------------------|
| Removing or renaming an endpoint | Adding a **new** endpoint |
| Removing or renaming a field in a response | Adding a **new optional** field to a response |
| Adding a **required** request parameter | Adding a new **optional** request parameter |
| Changing a field's type or format (e.g. `string` → `number`, date format) | Adding a new enum value *(only if clients tolerate unknowns — otherwise breaking)* |
| Changing authentication / authorization requirements | Performance / internal refactor with identical contract |
| Changing the meaning/semantics of an existing field | Bug fix that makes behavior match documented contract |
| Tightening validation so previously-accepted requests now fail | Loosening validation to accept more |
| Changing error codes/shapes clients depend on | Adding a new error code for a genuinely new condition |

**Rule:** Non-breaking changes ship into the **current** version. Breaking changes require a **new major version** running **alongside** the old one.

---

## 4. The Breaking-Change Workflow (PM-owned)

When refinement reveals a story needs a breaking API change:

1. **Flag it in refinement.** A breaking change is never a quiet story — it gets its own visible plan.
2. **Create a new versioned route** (`/v2/...`) implementing the new contract. The old route (`/v1/...`) keeps working unchanged.
3. **Identify every consumer** of the old version: Portal? other web? mobile? which external partners? List them on the ticket. (Use the `Affected Product` field + `ARCHITECTURE_CONTEXT.md` §5.)
4. **Migrate internal consumers** (web, mobile) to the new version as their own stories — sequenced so the API lands first.
5. **For mobile:** the new contract ships in a new app version. Until enough users have updated, the old version must keep serving them — or you bump the **minimum supported version** to force the update (`RELEASE_MANAGEMENT.md` §2.3). **Coordinate these two decisions explicitly.**
6. **For external partners:** notify them (see §5), give them the deprecation window, and only then sunset.
7. **Deprecate, then sunset** the old version per §5.

---

## 5. Deprecation & Sunset Policy

You don't delete an old API version the day the new one ships. You **deprecate** it (still works, but officially on notice), then **sunset** it (turned off) after a defined window.

### Minimum support windows

| Consumer of the old version | Minimum window before sunset |
|-----------------------------|------------------------------|
| **External / integration partner** | **90 days** from written deprecation notice *(propose; adjust per contract/SLA)* |
| **Mobile app (installed base)** | Until usage of the old version drops below an agreed threshold (e.g. <2% of active users) **or** the old version is below the forced **minimum supported version** |
| **Internal web only** | Until the consuming web app is migrated and deployed (can be days — they deploy in lockstep) |

### How to deprecate

1. **Announce in writing.** Email/notify external partners; post in `#dolphin-releases` and `#dolphin-dev` for internal. State: what's deprecated, the replacement, and the **sunset date**.
2. **Mark it in responses.** Add a `Deprecation` and `Sunset` HTTP header (RFC 8594) to responses from the old version:
   ```
   Deprecation: true
   Sunset: Sat, 01 Aug 2026 00:00:00 GMT
   Link: <https://docs.dolphin.example/v2/migration>; rel="deprecation"
   ```
3. **Track usage.** Monitor calls to the deprecated version (logs / metrics). Do not sunset while meaningful traffic remains — especially from mobile.
4. **Sunset.** After the window *and* once traffic is acceptably low, remove the old version. Announce the removal.

### Jira tracking

- Create a **Task** for the deprecation with the sunset date in the title (e.g. `DOL-xxx: Sunset /v1 users API (sunset 2026-08-01)`).
- Link it to the Epic that introduced the new version.
- Label `api`, plus `integration` if a partner is affected.

---

## 6. API Documentation & Contract

- Every API should publish its contract (OpenAPI/Swagger or, at minimum, a maintained markdown reference) so consumers — internal and external — have a single source of truth.
- A breaking change is **not Done** until the published contract for the new version is updated and the deprecation of the old one is documented.
- Add this to the API ticket's Definition of Done (see the DoD additions in `PM_GUIDE.md`).

---

## 7. PM Quick Checklist for Any API Story

- [ ] Is this change **breaking** (use the §3 table)?
- [ ] If breaking → is a **new major version** planned, with the old one left intact?
- [ ] Have **all consumers** been identified (web / mobile / external)?
- [ ] Is the **mobile force-update** interaction considered (`RELEASE_MANAGEMENT.md` §2.3)?
- [ ] Are external partners getting **written notice + the deprecation window**?
- [ ] Is the **sunset** tracked as its own dated Task?
- [ ] Is the **API contract/docs** update part of the ticket's DoD?
