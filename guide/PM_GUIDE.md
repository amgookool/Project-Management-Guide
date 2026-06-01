# 🐬 Dolphin — Project Manager Guide

> **Audience:** Project Manager (you)
> **Sprint Length:** 2 weeks | **Estimation:** Story Points (Fibonacci) | **Methodology:** Scrum

---

## 1. Your Role at Dolphin

As PM, you hold two critical responsibilities simultaneously:

| Hat | What It Means |
|-----|---------------|
| **Product Owner Proxy** | You own the backlog. You translate business needs into actionable, dev-ready tickets. You prioritize what gets built and in what order. |
| **Scrum Master** | You facilitate all Scrum ceremonies, protect the team from scope creep, remove blockers, and continuously improve the process. |

You are the **single point of contact** between the business team and the development team. Developers should never receive direct feature requests or bug reports from the business — everything flows through you.

---

## 2. Team Structure

| Person | Role | Primary Focus |
|--------|------|---------------|
| **You** | Project Manager | Backlog, ceremonies, stakeholder comms, blocker removal |
| **Developer 1** | Fullstack Developer | Cross-cutting features across frontend + backend, monorepo coordination |
| **Developer 2** | Backend Developer | APIs, microservices, database design, integrations, infrastructure |
| **Developer 3** | Frontend Developer | Web UI (React/Angular/etc.), mobile UI, design system implementation |

### Assigning Work — Rules of Thumb

- **Features with UI + API work** → Assign to Fullstack Dev OR split sub-tasks between Frontend + Backend Devs
- **Pure API / service / DB work** → Backend Dev
- **Pure UI / mobile work** → Frontend Dev
- **Cross-team dependencies** → Flag early in sprint planning; create a sub-task for each developer

---

## 3. Communication Structure (Google Chat)

Set up the following Google Chat Spaces:

| Space | Members | Purpose |
|-------|---------|---------|
| `#dolphin-general` | Everyone | Company-wide announcements |
| `#dolphin-dev` | PM + all 3 devs | Dev team discussions, technical decisions |
| `#dolphin-standups` | PM + all 3 devs | Async standup posts (use template in Ceremonies Guide) |
| `#dolphin-bugs` | PM + devs + business | Bug reports submitted by anyone |
| `#dolphin-releases` | PM + devs + business | Deployment and release announcements |
| `#dolphin-biz-requests` | PM + business team | Business team submits new feature/change requests |

### Golden Rules for Communication

1. **Business team → PM** for all requests (never directly to developers)
2. **PM → Dev team** translates business language into technical tickets
3. **Developers → PM** for blockers; **PM escalates** to business or other stakeholders
4. All decisions made in chat should be **documented in Jira** tickets

---

## 4. Scrum at Dolphin

### Sprint Calendar

| Sprint Day | Day of Week | Event | Who | Duration |
|-----------|-------------|-------|-----|----------|
| Day 1 | Monday | **Sprint Planning** | PM + all devs | 2 hours |
| Days 1–10 | Mon–Fri | **Daily Standup** | PM + all devs | 15 min |
| Day 7 | Wednesday | **Backlog Refinement** | PM + all devs | 1 hour |
| Day 10 | Friday | **Sprint Review** | PM + devs + business | 45 min |
| Day 10 | Friday | **Sprint Retrospective** | PM + all devs | 45 min |

> See [`SCRUM_CEREMONIES.md`](SCRUM_CEREMONIES.md) for full facilitation guides for each event.

### Capacity Planning (Story Points)

Story points measure **relative effort and complexity**, not time. Capacity is tracked in total points the team can complete per sprint — this is called **velocity**.

```
How to establish velocity:
  - Sprint 1–3: Don't commit hard; observe how many points the team completes
  - After Sprint 3: Average the completed points across past 3 sprints = team velocity
  - Example: Team completed 18, 22, 20 points → velocity = ~20 points/sprint

Adjust for:
  - PTO / holidays: reduce sprint commitment proportionally
    (1 dev out for 2 days = lose ~20% of 1/3 of capacity → reduce total by ~7%)
  - New team or new process: start conservatively (70% of estimated velocity)
  - Buffer for bugs/interruptions: built into point values, not calculated separately
```

Track velocity in Jira's built-in Velocity Chart (Backlog → Reports → Velocity Chart).

---

## 5. Backlog Management

### Priority Levels

| Priority | Definition | Response Time |
|----------|------------|---------------|
| **Critical** | System down, data loss, security breach, blocks core business | Immediate — interrupt sprint if needed |
| **High** | Major feature broken, significant business impact, upcoming deadline | Next sprint (or current if capacity allows) |
| **Medium** | Enhancement, non-blocking issue, routine feature | Within 2–3 sprints |
| **Low** | Nice-to-have, minor UI polish, non-urgent refactor | Queued — addressed when capacity available |

### Backlog Layers

```
EPICS (multi-sprint initiatives, owned by business/product vision)
  └── STORIES (user-facing value, fits in one sprint)
        └── TASKS (technical work, no direct user impact)
        └── SUB-TASKS (breakdown of stories when needed)

BUGS (separate from stories — always linked to an Epic if applicable)
SPIKES (time-boxed research — always produces a deliverable output)
```

---

## 6. Mediating Business ↔ Development

### The Translation Framework

When a business request comes in, ask these questions before writing the ticket:

1. **Who benefits?** (user type: customer, admin, internal team)
2. **What is the desired outcome?** (not the solution — the goal)
3. **What does success look like?** (measurable acceptance criteria)
4. **What is the priority and why?** (business impact, deadline)
5. **Are there dependencies?** (other systems, APIs, teams)
6. **What is the scope?** (MVP vs full feature)

### Translation Example

> **Business says:** "We need users to be able to reset their passwords."
>
> **PM writes:**
> - **Epic:** Authentication & Account Management
> - **Story:** As a registered user, I want to reset my password via email so that I can regain access if I forget my credentials.
> - **Acceptance Criteria:**
>   - [ ] User can request a password reset from the login page
>   - [ ] Reset email is sent within 30 seconds
>   - [ ] Reset link expires after 1 hour
>   - [ ] User is redirected to login after successful reset
>   - [ ] Old password no longer works after reset

### Weekly Business Update (Every Friday)

Send a brief update to the business team each Friday covering:
- What shipped this sprint (if sprint ends)
- What the team is working on next week
- Any blockers or risks that affect delivery timelines
- Questions or decisions needed from the business side

---

## 7. Definition of Ready (DoR)

A ticket must meet ALL criteria before it can be pulled into a sprint:

- [ ] Clear, concise title
- [ ] User story format (if applicable): *"As a [user], I want to [action] so that [benefit]"*
- [ ] Acceptance criteria written (testable, specific)
- [ ] Estimated in hours by the development team
- [ ] Priority set (Low / Medium / High / Critical)
- [ ] Linked to parent Epic (if applicable)
- [ ] Dependencies identified and noted
- [ ] Design mockups attached (if UI work is involved)
- [ ] No blocking issues at time of sprint planning

---

## 8. Definition of Done (DoD)

A ticket is **Done** when ALL of the following are true:

- [ ] All acceptance criteria met
- [ ] Code written and pushed to feature branch (`DOL-[id]/description`)
- [ ] Pull Request opened and linked to Jira ticket
- [ ] PR reviewed and approved by at least 1 other developer
- [ ] All automated tests pass (unit + integration)
- [ ] Code merged to `main` (or appropriate base branch)
- [ ] QA tested and approved in staging environment
- [ ] Jira ticket moved to **Done** status
- [ ] Business stakeholder notified (for user-facing features)
- [ ] Documentation updated (if behavior changed)

---

## 9. Bug Management

### Bug Triage Process

```
Bug Reported (via #dolphin-bugs or Jira)
    ↓
PM Reviews within 1 business day
    ↓
PM Assigns Priority:
  Critical → Notify dev team immediately → May interrupt sprint
  High     → Add to top of backlog → Plan for next sprint
  Medium   → Add to backlog → Groom in next refinement
  Low      → Add to backlog → Schedule when capacity allows
    ↓
PM creates Jira Bug ticket (use BUG_REPORT template)
    ↓
Dev investigates, estimates, implements fix
    ↓
QA verifies fix in staging
    ↓
PM notifies reporter when deployed
```

### Critical Bug Protocol

1. PM immediately notifies dev team in `#dolphin-dev`
2. Fullstack Dev (first responder) assesses scope
3. PM and tech lead decide: hotfix branch or pause current work
4. Bug fix follows expedited DoD (PR + 1 approval + QA sign-off)
5. Hotfix deployed and PM communicates resolution to business team

---

## 10. Release Management

### Release Cadence

- **Standard releases** align with sprint end (every 2 weeks)
- **Hotfixes** can be deployed any time for Critical bugs
- **Feature flags** are encouraged for large or risky features

### Pre-Release Checklist

- [ ] All sprint tickets in **Done** status
- [ ] QA signed off on all changes
- [ ] Release notes written (brief summary for business team)
- [ ] Deployment steps confirmed with Backend Dev
- [ ] Rollback plan identified
- [ ] PM announces release in `#dolphin-releases`

---

## 11. Key Metrics to Track

| Metric | How to Measure | Target | Review Frequency |
|--------|---------------|--------|-----------------|
| **Sprint Goal Achievement** | % of committed tickets reaching Done | ≥80% | Per sprint |
| **Sprint Velocity** | Total story points completed per sprint | Track trend (not a target) | Per sprint |
| **Bug Rate** | New bugs logged per sprint | Trending downward | Per sprint |
| **Cycle Time** | Avg time from "In Progress" → "Done" | < 3 days per ticket | Per sprint |
| **Blocked Time** | Days tickets spent in a blocked state | < 10% of cycle time | Per sprint |
| **Business Request Lead Time** | Days from request intake to sprint planning | < 2 sprints | Monthly |

Track these in a simple Jira dashboard or spreadsheet and review trends in retrospectives.

---

## 12. GitHub + Jira Integration

### Branch Naming

```
Feature/Story:  DOL-[id]/short-description
Bug Fix:        DOL-[id]/fix-short-description
Technical Task: DOL-[id]/task-short-description
Spike:          DOL-[id]/spike-short-description
Hotfix:         hotfix/DOL-[id]/short-description
```

### Pull Request Title Format

```
DOL-123: Add password reset email flow
DOL-456: Fix null pointer in user service
DOL-789: Refactor auth middleware
```

### PR Requirements

- Link to Jira ticket in the PR description
- At least 1 reviewer approved
- All CI checks pass
- No merge conflicts with `main`

### Jira ↔ GitHub Automation (configure in Jira)

| GitHub Action | Jira Automation |
|--------------|----------------|
| Branch created with `DOL-[id]` | Move ticket → **In Progress** |
| PR opened | Move ticket → **Code Review** |
| PR merged to main | Move ticket → **QA / Testing** |
| PR closed without merge | Move ticket → **In Progress** |

---

## 13. Onboarding Checklist for New Sprint

Use this at the start of every sprint:

- [ ] Capacity calculated for each developer
- [ ] Backlog refined (all top tickets meet DoR)
- [ ] Sprint goal written (one clear sentence)
- [ ] Sprint tickets selected and assigned
- [ ] Sprint board visible and up to date in Jira
- [ ] Standup cadence confirmed with team
- [ ] Any planned PTO flagged in capacity
