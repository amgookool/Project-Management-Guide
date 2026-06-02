# TECHNICAL TASK TEMPLATE
# Copy this content into a Jira Task description for technical/non-user-facing work.
# Use this for: DevOps setup, refactoring, dependency upgrades, config changes,
#               data migrations, CI/CD, documentation, and infrastructure work.
# Remove all comments (lines starting with #) before saving.
# -------------------------------------------------------------------

## ✅ Task Summary

**What needs to be done:**
<!-- Describe the task clearly. What will be different when this is done? -->


**Why this is needed:**
<!-- Technical rationale, debt reduction, prerequisite for another story, etc. -->


---

## 📋 Detailed Requirements

<!-- List exactly what needs to be built, configured, or changed -->
- [ ]
- [ ]
- [ ]

---

## 🏗 Technical Approach

<!-- Describe the implementation approach agreed upon during refinement -->
**Approach:**


**Alternative Approaches Considered:**


**Decision / Trade-offs:**


---

## 🏷 Metadata

| Field | Value |
|-------|-------|
| **Task Type** | [ ] Refactor  [ ] DevOps / Infrastructure  [ ] Dependency Upgrade  [ ] Data Migration  [ ] Configuration  [ ] Documentation  [ ] CI/CD  [ ] Other |
| **Epic Link** | DOL-XXX (if applicable) |
| **Priority** | [ ] Low  [ ] Medium  [ ] High  [ ] Critical |
| **Story Points** | [ ] 1  [ ] 2  [ ] 3  [ ] 5  [ ] 8  [ ] 13  [ ] 21 |
| **Assignee** | [ ] Fullstack  [ ] Backend  [ ] Frontend |
| **Label(s)** | `tech-debt` / `infrastructure` / `backend` / `frontend` / `database` / `firebase` / `api` / `integration` |
| **Affected Product(s)** | Portal / Landing-other web / Mobile (Flutter) / Web API / Integration API / Firebase / All |
| **Reporter** | |

---

## 🔗 Dependencies

**Blocked By:**
- DOL-

**Blocks:**
- DOL-

**Affected Services / Repos:**
- Repo/service 1:
- Repo/service 2:

---

## ⚠️ Risk Assessment

**Potential Risks:**
- [ ] Risk of breaking existing functionality → Mitigation:
- [ ] Downtime required → Plan:
- [ ] Requires DB migration → Rollback plan:
- [ ] External service changes → Coordination needed with:
- [ ] Other:

**Rollback Plan:**
<!-- If something goes wrong, how do we revert? -->


---

## 🧪 Testing / Verification Plan

<!-- How will we confirm this task was completed successfully? -->

**Verification Steps:**
1.
2.
3.

**Expected Outcome:**


**Automated Tests:**
- [ ] Existing tests still pass
- [ ] New tests added (describe):

---

## 📎 Reference Materials

<!-- Architecture docs, runbooks, ADRs, external docs, PRDs -->
-
-

---

## ✅ Definition of Done

- [ ] All requirements in the checklist above are completed
- [ ] Code reviewed and approved (min. 1 reviewer)
- [ ] All tests pass (no regressions) / CI green
- [ ] Merged to `staging` → deployed to staging and verified
- [ ] Relevant documentation updated
- [ ] Promoted to `main` → deployed to production (= live)
- [ ] Jira ticket updated to Done
