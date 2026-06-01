# SPIKE (RESEARCH) TEMPLATE
# Use this template for time-boxed investigation and research tasks.
# A Spike produces knowledge, not working software. It must always
# produce a documented output (decision, recommendation, or prototype).
# Remove all comments (lines starting with #) before saving.
# -------------------------------------------------------------------

## 🔍 Spike Summary

**What question are we trying to answer?**
<!-- State the research question clearly. A Spike without a clear question is just wandering. -->


**What triggered this Spike?**
<!-- User story that needed investigation, technical uncertainty, new tech evaluation, etc. -->
- Triggered by: DOL-___

---

## 🎯 Goal & Expected Output

**The goal of this Spike is to:**
<!-- Complete this sentence: "By the end of this Spike, we will know [X] so that we can [Y]." -->


**Deliverable (what will be produced at the end):**
<!-- A Spike must always produce one of the following -->
- [ ] Written recommendation / decision document (add to Jira or Confluence)
- [ ] Proof-of-concept / prototype code (link to branch or repo)
- [ ] Architecture Decision Record (ADR)
- [ ] Estimation / complexity assessment for a blocked story
- [ ] Comparison table of options
- [ ] Other:

---

## ⏱ Time Box

**Maximum time allocated:**
<!-- Spikes MUST have a time box. When time is up, document what you found — even if incomplete. -->

| Field | Value |
|-------|-------|
| **Time Box** | ___ hours (max) |
| **Assignee** | |
| **Due By (Sprint Day)** | |
| **Sprint** | |

> ⚠️ **Time box rule:** When the time box expires, STOP and document findings. Do not extend without PM approval. If more time is needed, create a follow-up spike.

---

## 🔬 Research Questions

<!-- List the specific questions that need to be answered -->
1.
2.
3.

---

## 🏷 Metadata

| Field | Value |
|-------|-------|
| **Priority** | [ ] Low  [ ] Medium  [ ] High  [ ] Critical |
| **Epic Link** | DOL-XXX (if applicable) |
| **Label(s)** | `spike` + `backend` / `frontend` / `infrastructure` / `api` |
| **Affected Product(s)** | Web / Mobile / API / Internal Tool |
| **Reporter** | |
| **Story Points** | [ ] 1  [ ] 2  [ ] 3  [ ] 5  [ ] 8  [ ] 13 (= Time Box) |

---

## 🔗 Dependencies

**Blocked By:**
- DOL-

**Unblocks (stories waiting for this Spike's output):**
- DOL-
- DOL-

---

## 📋 Investigation Approach

<!-- How will you conduct this research? What will you look at? -->

**Approach:**
1.
2.
3.

**Options / Alternatives to Evaluate:**
| Option | Pros | Cons |
|--------|------|------|
| Option A: | | |
| Option B: | | |
| Option C: | | |

---

## 📝 Findings

<!-- To be filled in by the developer during / after the Spike -->

**Summary of Findings:**


**Detailed Notes:**


**Option Evaluation:**
| Option | Verdict | Notes |
|--------|---------|-------|
| Option A: | | |
| Option B: | | |
| Option C: | | |

---

## 💡 Recommendation

<!-- Clear recommendation from the developer who conducted the Spike -->

**Recommended Approach:**


**Reasoning:**


**Risks / Caveats:**


**Estimated effort for recommended approach:**
- Estimated hours if we go forward:
- Follow-up stories to create: DOL-___, DOL-___

---

## ✅ Definition of Done

- [ ] Time box expired or research question answered (whichever comes first)
- [ ] Findings documented in this ticket
- [ ] Recommendation made (even if "we need more research")
- [ ] Output shared with the PM and dev team (review in next standup or refinement)
- [ ] Follow-up tickets created if applicable
- [ ] Spike ticket moved to **Done**
