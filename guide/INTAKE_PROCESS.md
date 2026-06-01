# 🐬 Dolphin — Business Request Intake Process

> This document defines how requests from the business team are received, evaluated, translated into development tickets, and prioritized. Share this guide with the business team so they know how to submit requests.

---

## Why This Process Exists

Without a structured intake process:
- Developers get pulled in multiple directions by competing requests
- Important work gets skipped because loud voices dominate
- The team can't plan effectively without a stable backlog
- Context and requirements get lost between business and dev

This process ensures every request is **heard**, **evaluated**, and **prioritized fairly** while giving the development team the clarity they need to execute.

---

## 1. How to Submit a Request (For the Business Team)

All requests must go through the PM. Do NOT contact developers directly for feature requests or bug reports.

### Channel: `#dolphin-biz-requests` (Google Chat)

Post your request using this template:

```
🆕 NEW REQUEST

Type: [Feature Request / Bug / Change Request / Question]
Title: [One-line summary]
Priority (your view): [Critical / High / Medium / Low]
Deadline (if any): [MM/DD/YYYY or "No hard deadline"]

Description:
[What do you want? Describe the problem or opportunity, not the solution.]

Who benefits:
[Which users, customers, or teams does this affect?]

Business impact:
[What happens if we don't do this? Revenue impact? Compliance? Customer churn?]

Supporting materials:
[Links to mockups, screenshots, specs, examples — attach anything relevant]
```

### Alternatively: Submit directly in Jira (if given access)
Business team members can be given **Reporter** access to Jira and use the intake form configured there (see Jira Setup Guide).

---

## 2. PM Intake Responsibilities

Upon receiving a request, the PM should:

| Step | Action | Timeframe |
|------|--------|-----------|
| 1 | Acknowledge the request in `#dolphin-biz-requests` | Same business day |
| 2 | Ask clarifying questions if needed | Within 1–2 business days |
| 3 | Log the item in Jira backlog (use appropriate template) | Within 2 business days |
| 4 | Assign initial priority and link to Epic | During next refinement session |
| 5 | Communicate estimated timeline to requester | After sprint planning |

### Clarifying Questions Checklist

Before creating a Jira ticket, make sure you can answer:

- [ ] What user problem does this solve?
- [ ] Who is the primary user affected?
- [ ] What does success look like? (How do we know it's done?)
- [ ] Is there a specific deadline or business event driving this?
- [ ] Are there existing systems or designs this must align with?
- [ ] Is this a new capability or a change to something existing?
- [ ] Have we done anything similar before?

---

## 3. Request Types and Ticket Mapping

| Business Says | PM Creates in Jira |
|---------------|-------------------|
| New feature / capability | **Epic** → broken into **User Stories** |
| Improvement to existing feature | **User Story** (linked to existing Epic) |
| Something is broken / not working | **Bug** |
| "Can we change how X works?" | **User Story** or **Task** depending on size |
| "We need to research options for X" | **Spike** |
| Configuration / data fix | **Task** |

---

## 4. Prioritization Framework

Prioritization is the PM's responsibility, in consultation with business stakeholders and the dev team.

### Priority Decision Matrix

Score each request on these factors (1–3 each), then sum:

| Factor | 1 Point | 2 Points | 3 Points |
|--------|---------|---------|---------|
| **Business Impact** | Low (nice-to-have) | Medium (efficiency/revenue) | High (revenue, compliance, churn) |
| **User Impact** | Few users affected | Some users affected | All/most users affected |
| **Urgency** | No deadline | Soft deadline within 1–2 months | Hard deadline within 2 weeks |
| **Dev Effort** | High effort (many sprints) | Medium effort (1 sprint) | Low effort (< 1 day) |

**Total Score:**
- **10–12** → Critical
- **7–9** → High
- **4–6** → Medium
- **3** → Low

> This is a guide, not a rule. Business strategy and stakeholder agreements may override the score.

### Who Decides Priority?

| Priority Level | Decision Maker |
|---------------|----------------|
| Critical | PM + business stakeholder + tech lead (dev team) aligned |
| High | PM with business stakeholder input |
| Medium | PM judgment; confirmed in backlog refinement |
| Low | PM judgment; team agrees |

---

## 5. Saying No (or "Not Now")

Not every request will be accepted immediately. Here's how to handle pushback gracefully:

### "Not now" (defer to backlog)

> *"Thank you for submitting this. It's been logged in our backlog as a [Medium/Low] priority item. Based on the current sprint plan, the earliest it could be picked up is [Sprint N / approximate date]. I'll keep you updated."*

### "No" (won't do)

> *"After reviewing this with the team, we've decided not to move forward with this because [reason: out of scope / technical constraint / conflicts with roadmap]. If the situation changes, feel free to resubmit. I'm happy to discuss further."*

### Scope Change Mid-Sprint

Mid-sprint changes are **disruptive and costly**. They should only happen for Critical issues.

> *"We can't add this to the current sprint without dropping another ticket. If this is truly urgent, let's discuss which existing item we would swap it for. Otherwise, I'll queue it for Sprint [N+1]."*

---

## 6. Request Lifecycle (End-to-End)

```
Business Team
     │
     │ Submits request via #dolphin-biz-requests or Jira
     ▼
PM Receives & Acknowledges (same day)
     │
     │ Clarifies requirements
     ▼
PM Creates Jira Ticket (within 2 business days)
     │
     │ Assigns priority and links to Epic
     ▼
Backlog Refinement (mid-sprint)
     │
     │ Dev team reviews, asks questions, estimates hours
     │ Ticket marked "Ready" if meets DoR
     ▼
Sprint Planning (start of sprint)
     │
     │ Ticket selected for sprint based on priority + capacity
     ▼
Development (In Progress → Code Review → QA)
     │
     ▼
Sprint Review
     │
     │ Business stakeholder sees demo, approves
     ▼
Done & Deployed
     │
     │ PM notifies requester
     ▼
Business Team Confirms ✅
```

---

## 7. Communicating Back to the Business Team

### Weekly Status Update (every Friday)

Send to the business team leadership via `#dolphin-biz-requests` or email:

```
📊 Dolphin Dev Team — Weekly Update [Date]

✅ Completed This Sprint:
- [Feature/fix and one-sentence description]
- [Feature/fix and one-sentence description]

🔨 In Progress This Sprint:
- [Item and expected completion]

📋 Coming Up Next Sprint:
- [Top 3 planned items]

⚠️ Risks / Blockers:
- [Any delays or concerns the business should know about]

❓ Decisions Needed From Business:
- [Any open questions that would unblock the team]
```

### Request Status Responses

| Status | What to Tell the Business Team |
|--------|-------------------------------|
| Just logged | "Received and logged as DOL-[id]. Priority: [X]. Estimated sprint: [N]." |
| In refinement | "Being reviewed by the dev team this week. Estimate and timeline TBD." |
| In sprint | "Work has started! Expected in Sprint [N] by [date]." |
| In QA | "Development complete. Going through QA — nearly there!" |
| Done | "Deployed to production! Here's what to expect: [brief description]." |
| Blocked | "This item is blocked by [reason]. We're working on it and estimate [timeframe]." |

---

## 8. Managing Stakeholder Expectations

### Key Principles

1. **Underpromise and overdeliver** — give conservative timelines, then surprise them
2. **No surprises** — communicate risks and delays proactively, not after the fact
3. **Context, not just status** — explain the "why" behind priority decisions
4. **Involve stakeholders in trade-offs** — when capacity forces a choice, present the options and let them help decide

### Handling "This Is Urgent" Requests

Every request feels urgent to the person submitting it. Use this script:

> *"I hear you on the urgency. Help me understand the impact — what happens to the business if this isn't done by [date]? That will help me make the case for prioritizing it over other items currently in the sprint."*

Then either:
- Re-prioritize legitimately if the business impact is real
- Explain why other work takes precedence if it doesn't meet the bar
