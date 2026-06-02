# 🐬 Dolphin — Scrum Ceremonies Playbook

> **Sprint Length:** 2 weeks (10 working days)
> **Team:** PM (Scrum Master / PO proxy) + Fullstack Dev + Backend Dev + Frontend Dev + QA Engineer + Designer

---

## Overview — Sprint Calendar

```
WEEK 1                                    WEEK 2
Mon     Tue  Wed  Thu  Fri               Mon  Tue  Wed   Thu  Fri
 |       |    |    |    |                 |    |    |     |    |
[Sprint Planning]                                  [Refinement] [Review + Retro]
[Standup][Std][Std][Std][Std]           [Std][Std][Std] [Std][Std]
```

---

## 1. Sprint Planning

**When:** Day 1 of each sprint (Monday morning)
**Duration:** 2 hours
**Attendees:** PM + 3 developers + QA Engineer + Designer
**Location:** Google Meet or in-person

### Purpose
Align the team on what will be built this sprint and how. By the end, every developer knows exactly what they're working on and the team has committed to a sprint goal.

### Before the Meeting (PM prepares)

- [ ] Backlog is refined — top tickets all meet the Definition of Ready
- [ ] Team capacity calculated (see capacity formula in PM_GUIDE.md)
- [ ] Sprint goal drafted (1 clear sentence summarizing the sprint's purpose)
- [ ] Jira sprint created and named: `Dolphin Sprint [N] — [Month DD–DD, YYYY]`

### Agenda

| Time | Activity |
|------|----------|
| 0:00–0:10 | **Opening** — Review capacity and sprint goal draft |
| 0:10–0:50 | **Part 1: What** — PM presents top backlog items; team asks clarifying questions; stories are accepted into sprint |
| 0:50–1:40 | **Part 2: How** — Team discusses approach for each ticket; tasks/sub-tasks created; **story point estimates confirmed**; cross-product order set (API before consumers) |
| 1:40–1:55 | **Commit** — Team confirms sprint commitment fits capacity; sprint goal finalized |
| 1:55–2:00 | **Wrap up** — Sprint officially started in Jira |

> **Sequencing during planning:** For any feature spanning API + web/mobile, agree the order explicitly — the API/Firestore story lands first, consumers follow. See [`ARCHITECTURE_CONTEXT.md`](ARCHITECTURE_CONTEXT.md) §5. Confirm UI stories have **Designer mockups attached** before committing them.

### Sprint Goal Format

> *"By the end of this sprint, [user/stakeholder] will be able to [capability], so that [business value]."*

**Example:**
> *"By the end of this sprint, registered users will be able to reset their passwords via email, so that they can regain access without contacting support."*

### PM Facilitation Tips

- Keep the "What" discussion under 40 minutes — don't let it become a design meeting
- If a ticket sparks a debate, timebox it to 5 minutes then table it for a separate spike
- Protect developers from committing to more than capacity allows
- Write the sprint goal on the Jira sprint board

---

## 2. Daily Standup

**When:** Every weekday, same time (suggested: 9:30 AM)
**Duration:** 15 minutes (hard stop)
**Attendees:** PM + 3 developers + QA Engineer + Designer
**Format:** Google Meet OR async in `#dolphin-standups` (team decides)

### Purpose
Quickly surface progress, plans, and blockers. This is NOT a status report to the PM — it's the team synchronizing with each other.

### Three Questions (each person answers)

1. **What did I complete since the last standup?** *(reference Jira ticket numbers)*
2. **What am I working on today?** *(reference Jira ticket numbers)*
3. **Do I have any blockers or impediments?**

### Async Standup Template (Google Chat `#dolphin-standups`)

```
📅 Standup — [Date]

✅ Done:
- DOL-[id]: [brief description]

🔨 Today:
- DOL-[id]: [brief description]

🚧 Blockers:
- [None OR description of blocker]
```

### PM's Role in Standup

- Keep the standup under 15 minutes — table deeper discussions ("let's take that offline")
- Note all blockers and follow up immediately after the meeting
- Update Jira board if any tickets need to move columns
- Do NOT use standup to assign new work

### Handling Blockers

| Blocker Type | PM Action |
|-------------|----------|
| Needs a decision from business team | Escalate to business contact in `#dolphin-biz-requests` same day |
| Depends on another dev's work | Schedule a quick 1:1 between the two devs to align |
| Technical unknowns | Log a Spike ticket; time-box investigation to 1 day |
| External service/API issue | File with vendor; provide workaround if possible |
| Missing design/mockup | Escalate to design resource immediately |

---

## 3. Backlog Refinement (Grooming)

**When:** Mid-sprint, Day 7 (Wednesday of Week 2)
**Duration:** 1 hour
**Attendees:** PM + 3 developers + QA Engineer + Designer
**Goal:** Keep 2 sprints worth of backlog in a "Ready" state

### Purpose
Prepare upcoming tickets so that Sprint Planning runs smoothly. Tickets that are refined now become the candidates for the next sprint.

### Before the Meeting (PM prepares)

- [ ] Next sprint's candidate tickets identified and sorted by priority
- [ ] Business requests translated into stories with draft acceptance criteria
- [ ] Epics reviewed for progress and scope
- [ ] **Designer briefed on upcoming UI stories** so mockups are ready *before* those stories are pulled into a sprint (design is part of the Definition of Ready)
- [ ] Any unclear items noted for discussion

> **Roles in refinement:** The **Designer** walks through mockups for UI stories. The **QA Engineer** pressure-tests the acceptance criteria — "how would I verify this?" — and flags untestable AC and missing edge cases before the story is called Ready. For API stories, confirm whether the change is **breaking** ([`API_VERSIONING.md`](API_VERSIONING.md) §3).

### Agenda

| Time | Activity |
|------|----------|
| 0:00–0:05 | Review backlog at a glance — confirm what's being groomed today |
| 0:05–0:45 | Walk through each ticket top-down: clarify, refine AC, estimate story points, identify dependencies |
| 0:45–0:55 | Prioritization check — adjust order if needed |
| 0:55–1:00 | Confirm which tickets are now "Ready" for next sprint |

### Estimation Guidance — Story Points (Fibonacci)

Story points measure **relative complexity and effort**, not time. The team estimates together using the Fibonacci sequence: **1, 2, 3, 5, 8, 13, 21**.

| Points | Complexity | What It Feels Like | Example |
|--------|-----------|-------------------|---------|
| **1** | Trivial | I've done this a hundred times, no unknowns | Update a text label, fix a typo, change a config value |
| **2** | Simple | Straightforward, minimal unknowns | Add a field to a form, update a static page, fix a minor CSS issue |
| **3** | Small | Clear scope, some implementation decisions | Simple CRUD endpoint, small reusable UI component |
| **5** | Medium | Moderate complexity, a few unknowns | Multi-step form, REST API integration, feature with 3–4 AC items |
| **8** | Large | Complex, multiple moving parts, significant unknowns | New microservice endpoint, complex UI flow, cross-system integration |
| **13** | Very Large | High uncertainty, likely needs to be broken down | Major feature, architectural change — consider splitting |
| **21** | Epic-sized | This should be an Epic, not a story — split it | Multi-week initiative — break into smaller stories |

> 🎯 **The golden rule:** If a story is estimated at **13 or higher**, it's a signal to break it into smaller stories before it enters a sprint. A single sprint story should ideally be **8 points or less**.

> 📊 **Velocity counts Stories only.** Estimate Bugs, Tasks, and Spikes too (you need them for capacity), but only **Story** points feed the velocity trend. Commit Story points up to the team's velocity, then fit bugs/tasks/spikes into the remaining capacity. See [`PM_GUIDE.md`](PM_GUIDE.md) §4.

### Why Fibonacci and Not 1–10?

The gaps between Fibonacci numbers grow intentionally. The larger the task, the less precise our estimate — and that's okay. The sequence forces the team to make a meaningful choice:
- Is this a 5 or an 8? That gap forces a real conversation about scope and risk.
- You can't give everything a "6" — Fibonacci prevents false precision.

### Planning Poker (How to Estimate as a Team)

During Backlog Refinement, use Planning Poker to get unbiased estimates:

1. PM reads the ticket and acceptance criteria aloud
2. Each developer **privately** picks their estimate (use cards, fingers, or a Planning Poker app)
3. Everyone reveals at the same time
4. If estimates align → that's the estimate ✅
5. If estimates differ → the **highest** and **lowest** estimators explain their reasoning
6. Team discusses and re-votes until consensus is reached
7. PM records the agreed estimate on the Jira ticket

> **Never let one person estimate alone.** The developer doing the work is often too optimistic or too pessimistic without the group's perspective.

### PM Facilitation Tips

- Ask developers **"What would make this ticket easier to estimate?"** — often it reveals missing information
- Use the "XL rule": if the team can't finish it in one sprint, split it
- Don't estimate alone — always involve the developer(s) who will do the work
- Keep discussions focused on the ticket at hand; new topics go to the parking lot

---

## 4. Sprint Review (Demo)

**When:** Day 10 of sprint (Friday afternoon)
**Duration:** 45 minutes
**Attendees:** PM + 3 developers + QA Engineer + Designer + business stakeholders
**Format:** Google Meet with screen sharing

### Purpose
Demonstrate completed work to stakeholders, gather feedback, and officially close the sprint. This is a **collaborative session**, not a presentation.

### Before the Meeting (PM prepares)

- [ ] Confirm all "Done" tickets with developers
- [ ] Prepare a brief agenda listing what will be demoed
- [ ] Ensure staging environment is stable
- [ ] Invite business stakeholders (calendar invite with Google Meet link)
- [ ] Move unfinished tickets out of the sprint (do NOT count as Done)

### Agenda

| Time | Activity |
|------|----------|
| 0:00–0:05 | PM opens: sprint goal recap, what was committed, what was completed |
| 0:05–0:35 | **Live demos** — each developer demos their completed work (rotate, ~10 min each) |
| 0:35–0:45 | Q&A and feedback from stakeholders; PM notes feedback as backlog items |

### Demo Best Practices for Developers

- Demo in the **staging environment** (never production)
- Show it working end-to-end, not just the code
- Focus on the user's perspective — "Here's what the user can now do"
- Keep each demo under 10 minutes

### What Counts as Done for the Review

Only tickets that have passed QA and are in **Done** status are shown. Partially completed work is **not demoed** — move it to the next sprint.

### Collecting Feedback

PM captures all stakeholder feedback during the review:
- New requests → create a new Story/Epic in Jira
- Changes to existing tickets → add a comment to the Jira ticket
- Approvals/sign-offs → note in Jira ticket and email to stakeholder

---

## 5. Sprint Retrospective

**When:** Immediately after Sprint Review, Day 10 (Friday)
**Duration:** 45 minutes
**Attendees:** PM + 3 developers + QA Engineer + Designer (NO business stakeholders)
**Format:** Psychological safety is paramount — this is a safe space

### Purpose
Continuously improve how the team works together. Focus on process, not people.

### Retrospective Format — Start / Stop / Continue

Use a shared Google Doc or Jamboard with three columns:

| Column | Question |
|--------|----------|
| 🟢 **Start** | What should we START doing that we're not doing? |
| 🔴 **Stop** | What should we STOP doing because it's slowing us down? |
| 🔵 **Continue** | What is working well that we should KEEP doing? |

### Agenda

| Time | Activity |
|------|----------|
| 0:00–0:05 | Set the tone — this is a safe space; focus on process not people |
| 0:05–0:15 | Silent brainstorm — everyone writes items in each column independently |
| 0:15–0:30 | Team reads and discusses — group similar items, vote on top issues |
| 0:30–0:40 | Action items — pick **1–3 concrete improvements** to implement next sprint |
| 0:40–0:45 | Close — PM documents action items in Jira as Tasks for the next sprint |

### PM Facilitation Tips

- Start with a quick mood check (1–5 how did this sprint feel?)
- Let silence happen during brainstorm — don't fill it
- Protect the team: if someone raises a blame statement, redirect to "what process could prevent this?"
- Always end with **specific, assigned action items** — vague actions never get done
- Review last sprint's retro actions at the START of the next retro (did we follow through?)

### Retrospective Action Item Format

> **Action:** [Specific change to make]
> **Owner:** [Person responsible]
> **By when:** [Next sprint or specific date]

---

## 6. Quick Reference — Ceremonies at a Glance

| Ceremony | When | Duration | Who | Key Output |
|----------|------|----------|-----|-----------|
| Sprint Planning | Day 1 (Mon) | 2 hrs | PM + devs | Sprint goal, committed tickets |
| Daily Standup | Daily (Mon–Fri) | 15 min | PM + devs | Blocker list, updated board |
| Backlog Refinement | Day 7 (Wed) | 1 hr | PM + devs | Ready backlog for next sprint |
| Sprint Review | Day 10 (Fri) | 45 min | PM + devs + business | Stakeholder feedback, done items |
| Sprint Retrospective | Day 10 (Fri) | 45 min | PM + devs | 1–3 process improvements |

---

## 7. Ceremony Anti-Patterns to Avoid

| Anti-Pattern | Why It's a Problem | Solution |
|-------------|-------------------|---------|
| Standup becomes a status report to the PM | Kills team collaboration | Redirect: "Tell the team, not me" |
| Sprint Planning without refined backlog | Wastes 2 hours debating unclear tickets | Enforce Backlog Refinement |
| No sprint goal | Team has no focus; everything is equally important | Always write one goal per sprint |
| Retrospective without action items | Nothing improves | Require 1 specific action every retro |
| Demo of unfinished work | Confuses stakeholders and erodes trust | Only demo Done tickets |
| Extending standups to solve problems | Wastes everyone's time | Table it: "Let's take that offline" |
| Everything lands in QA on Day 9–10 | QA can't verify a sprint's work in two days; tickets slip or skip QA | **Code-complete target = Day 8.** Work not in QA by Day 9 likely won't reach Done this sprint — plan for it, don't pretend. |
| Web/mobile consumer built before its API | Consumer is blocked or built against a guess | Sequence the API story first ([`ARCHITECTURE_CONTEXT.md`](ARCHITECTURE_CONTEXT.md) §5) |
| UI story pulled in without a mockup | Dev guesses the design, rework follows | Designer mockup is part of Definition of Ready |
