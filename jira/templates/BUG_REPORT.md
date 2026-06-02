# BUG REPORT TEMPLATE
# Copy this content into a Jira Bug description when logging a defect.
# Remove all comments (lines starting with #) before saving.
# -------------------------------------------------------------------

## 🐛 Bug Summary

**One-line description of the bug:**
<!-- Be specific: "Login button unresponsive on mobile Safari when email contains special characters" -->


---

## 🔍 Reproduction Steps

<!-- Provide exact steps so any developer can reproduce this bug reliably -->

1. Navigate to / open:
2. Enter / select / click:
3. Observe:

**Reproducible:** [ ] Always  [ ] Intermittently (___% of the time)  [ ] Only once

---

## ✅ Expected Behavior

<!-- What SHOULD happen? What was the intended behavior? -->


---

## ❌ Actual Behavior

<!-- What IS happening instead? Be specific. -->


---

## 📸 Evidence

<!-- Attach screenshots, screen recordings, or error logs directly in Jira -->
- Screenshot:
- Screen recording:
- Console error / stack trace:
```
[Paste error or stack trace here]
```
- Network request/response (if API-related):
```
[Paste relevant request/response here]
```

---

## 🌐 Environment

| Field | Value |
|-------|-------|
| **Environment** | [ ] Production  [ ] Staging  [ ] Development |
| **Affected Product** | [ ] Portal  [ ] Landing/other web  [ ] Mobile (Flutter)  [ ] Web API  [ ] Integration API  [ ] Firebase |
| **Browser / Client** | e.g., Chrome 124, iOS Safari 17, Android Chrome |
| **OS / Device** | e.g., macOS 14, iPhone 15, Windows 11 |
| **App Version / Build** | |
| **User Account / Role** | e.g., Admin user, Free tier customer (anonymize PII) |

---

## 🏷 Classification

| Field | Value |
|-------|-------|
| **Priority** | [ ] Low  [ ] Medium  [ ] High  [ ] Critical |
| **Severity** | [ ] Low  [ ] Medium  [ ] High  [ ] Critical |
| **Story Points** | [ ] 1  [ ] 2  [ ] 3  [ ] 5  [ ] 8  [ ] 13  [ ] 21 |
| **Epic Link** | DOL-XXX (if this bug relates to a known feature area) |
| **Reporter** | |
| **Reported By (business)** | |
| **Date Reported** | |
| **Label(s)** | `bug` + `frontend` / `backend` / `api` / `mobile` |

> **Priority vs Severity:**
> - *Priority* = How urgently should we fix this? (business impact)
> - *Severity* = How bad is the damage to the system? (technical impact)
> Example: A typo on the homepage is low severity but could be high priority if it's on a landing page.

---

## 🔗 Related Items

**Related to Story / Feature:**
- DOL-

**Related Bugs (duplicates or related issues):**
- DOL-

**Customer / Support Tickets (if reported by a customer):**
-

---

## 🔎 Root Cause Analysis

<!-- To be filled in by the developer when the fix is implemented -->
**Root Cause:**


**Why It Wasn't Caught Earlier:**
- [ ] Missing test coverage
- [ ] Edge case not covered in acceptance criteria
- [ ] Environment-specific issue (not caught in staging)
- [ ] Regression from another change (DOL-___)
- [ ] Other:

---

## 🛠 Fix Description

<!-- To be filled in by the developer -->
**What was changed:**


**Files / Services modified:**
-

**PR Link:** [DOL-XXX/fix-description](https://github.com/your-org/repo/pull/...)

---

## 🧪 QA Verification

<!-- To be filled in by QA verifier -->
**Tested by:**
**Tested on:** [ ] Staging  [ ] Pre-production

**Verification Steps:**
1.
2.
3.

**QA Result:** [ ] ✅ Passed  [ ] ❌ Failed (see comment)

---

## ✅ Definition of Done

- [ ] Root cause identified
- [ ] Fix implemented and PR approved
- [ ] All automated tests pass (including new regression test if applicable)
- [ ] Merged to `staging` → QA verified fix on staging (web/API) or TestFlight (mobile)
- [ ] Web/API: promoted to `main` → live in production. Mobile: submitted to store (force-update decision recorded if needed)
- [ ] Original reporter notified
- [ ] Jira ticket closed
