# 🐬 Dolphin — Jira Project Setup Guide

> This guide walks you through setting up a Jira Scrum project from scratch for the Dolphin development team.
> **Prerequisite:** You have a Jira Software (Cloud) account. Free tier works for teams up to 10 users.

---

## Step 1 — Create the Jira Project

1. Log in to Jira → Click **Create project**
2. Select **Scrum** (under Software Development)
3. Click **Use template** → **Scrum**
4. Fill in:
   - **Project name:** `Dolphin`
   - **Project key:** `DOL`
   - **Project lead:** Your name
5. Click **Create project**

---

## Step 2 — Configure the Workflow (Board Columns)

Navigate to: **Project Settings → Workflows**

Create or edit the default workflow to include these statuses in order:

| Column (Status) | Description | Who Moves Tickets Here |
|----------------|-------------|----------------------|
| **Backlog** | All new and unrefined items | PM (when ticket is created) |
| **Refined** | Groomed, estimated, meets DoR | PM (after refinement session) |
| **In Sprint** | Committed to current sprint, not started | PM (during sprint planning) |
| **In Progress** | Actively being worked on | Developer |
| **Code Review** | PR submitted, waiting for review | Developer |
| **QA / Testing** | PR merged, ready for QA verification | Developer (after merge) |
| **Done** | QA passed, deployed (or ready to deploy) | PM or QA verifier |
| **Blocked** | Cannot proceed — reason documented in comments | Developer or PM |

### How to Add Statuses in Jira

1. Go to **Project Settings → Workflows**
2. Click the **pencil icon** to edit the active workflow
3. Click **Add status** for each column above
4. Connect statuses with transitions (drag arrows between them)
5. Publish the workflow

### Recommended Transitions

```
Backlog → Refined (after grooming)
Refined → In Sprint (sprint planning)
In Sprint → In Progress (developer picks up ticket)
In Progress → Code Review (PR opened)
In Progress → Blocked (blocker identified)
Blocked → In Progress (blocker resolved)
Code Review → QA / Testing (PR approved and merged)
Code Review → In Progress (PR rejected — needs rework)
QA / Testing → Done (QA approved)
QA / Testing → In Progress (QA failed — needs rework)
Done → In Progress (bug found after Done — reopen)
```

---

## Step 3 — Configure Issue Types

Navigate to: **Project Settings → Issue Types**

Set up these issue types:

| Issue Type | Icon | Purpose |
|-----------|------|---------|
| **Epic** | ⚡ | Large initiative spanning multiple sprints |
| **Story** | 📖 | User-facing feature (1 sprint or less) |
| **Bug** | 🐛 | Defect or unexpected behavior |
| **Task** | ✅ | Technical work with no direct user value (DevOps, refactor, config) |
| **Spike** | 🔍 | Time-boxed research or investigation |
| **Sub-task** | 🔩 | Breakdown of a Story or Task |

---

## Step 4 — Configure Custom Fields

Navigate to: **Project Settings → Fields**

Add these custom fields to your project:

| Field Name | Type | Used On | Options |
|-----------|------|---------|---------|
| **Priority** | Single select | All types | Low, Medium, High, Critical |
| **Epic Link** | Epic link | Story, Bug, Task, Spike | Links to parent Epic |
| **Sprint** | Sprint | All types | Auto-populated from sprint |
| **Story Points** | Number | Story, Task, Bug, Spike | Fibonacci: 1, 2, 3, 5, 8, 13, 21 |
| **Estimated Hours** | Number | All types | Optional — used for scheduling only, not estimation |
| **Affected Product** | Multi-select | Bug | Web App, Mobile App, API, Internal Tool |
| **Reporter** | User | All types | Person who submitted the request |
| **Severity** | Single select | Bug only | Low, Medium, High, Critical |
| **Environment** | Single select | Bug only | Production, Staging, Development |
| **GitHub PR Link** | URL | Story, Bug, Task | Link to Pull Request |
| **Business Requester** | Text | Story | Name of person who requested the feature |
| **Release Target** | Sprint / Version | Story, Task | Which release this belongs to |

---

## Step 5 — Set Up the Scrum Board

Navigate to: **Board → Board settings**

### Column Mapping
Map your workflow statuses to board columns exactly as defined in Step 2.

### Swimlanes (Optional but Recommended)
Set swimlanes to **Epics** so you can see work grouped by feature area.

### Quick Filters (Add These)
These make it easy to filter the board by person or type:

| Filter Label | JQL |
|-------------|-----|
| My Tickets | `assignee = currentUser()` |
| Bugs Only | `issuetype = Bug` |
| Blocked | `status = Blocked` |
| Critical | `priority = Critical` |
| In Review | `status = "Code Review"` |
| Frontend | `labels = frontend` |
| Backend | `labels = backend` |
| Fullstack | `labels = fullstack` |

---

## Step 6 — Configure Labels

Labels help you filter by domain and assignee responsibility. Add these labels to your project:

| Label | Usage |
|-------|-------|
| `frontend` | Primarily frontend work |
| `backend` | Primarily backend/API work |
| `fullstack` | Requires both frontend and backend |
| `mobile` | Mobile application work |
| `web` | Web application work |
| `api` | API / microservice work |
| `internal-tool` | Internal tooling or dashboard |
| `infrastructure` | DevOps, CI/CD, cloud config |
| `database` | Schema changes, migrations, data work |
| `security` | Auth, permissions, vulnerability fixes |
| `performance` | Optimization, caching, load |
| `tech-debt` | Refactoring, cleanup, upgrades |
| `design` | UI/UX changes requiring design review |
| `hotfix` | Emergency production fix |

---

## Step 7 — Set Up Versions / Releases

Navigate to: **Project Settings → Releases (Versions)**

Create a version for each sprint or release cycle:

**Naming convention:** `Sprint [N] — [Month YYYY]`
**Example:** `Sprint 1 — June 2026`

This allows you to track what shipped in each sprint and generate release notes.

---

## Step 8 — Create the Sprint Structure

Navigate to: **Backlog** in your project.

1. Click **Create Sprint** → Rename it: `Dolphin Sprint 1 — [Start Date] to [End Date]`
2. Set start date (Monday) and end date (Friday, 2 weeks later)
3. Write the Sprint Goal in the sprint header
4. Drag refined tickets from the backlog into the sprint
5. Click **Start Sprint** when ready

---

## Step 9 — Connect GitHub to Jira

This enables automatic ticket transitions based on GitHub activity.

### Option A: Jira + GitHub Integration (Official)

1. Go to **Jira Settings → Apps → Find new apps**
2. Search for **GitHub for Jira** → Install
3. Follow the OAuth flow to connect your GitHub organization
4. Select the repositories to link (all Dolphin repos)

### Option B: GitHub Actions (Manual Automation)

Add this to your repo's GitHub Actions workflow to auto-transition Jira tickets:

```yaml
# .github/workflows/jira-transition.yml
name: Jira Ticket Transition

on:
  pull_request:
    types: [opened, closed]

jobs:
  transition:
    runs-on: ubuntu-latest
    steps:
      - name: Extract Jira ticket from branch
        id: extract
        run: |
          BRANCH="${{ github.head_ref }}"
          TICKET=$(echo "$BRANCH" | grep -oP 'DOL-\d+')
          echo "ticket=$TICKET" >> $GITHUB_OUTPUT

      - name: Transition on PR Open → Code Review
        if: github.event.action == 'opened'
        # Use Jira REST API to transition the ticket
        run: |
          curl -X POST \
            -H "Authorization: Basic ${{ secrets.JIRA_TOKEN }}" \
            -H "Content-Type: application/json" \
            -d '{"transition": {"id": "CODE_REVIEW_TRANSITION_ID"}}' \
            "https://your-domain.atlassian.net/rest/api/3/issue/${{ steps.extract.outputs.ticket }}/transitions"
```

### Automation Rules in Jira (Recommended)

Navigate to: **Project Settings → Automation**

Create these automation rules:

| Trigger | Condition | Action |
|---------|-----------|--------|
| Branch created | Branch name contains `DOL-[0-9]+` | Transition ticket → **In Progress** |
| PR opened | PR title contains `DOL-[0-9]+` | Transition ticket → **Code Review** |
| PR merged | PR title contains `DOL-[0-9]+` | Transition ticket → **QA / Testing** |
| PR closed (not merged) | PR title contains `DOL-[0-9]+` | Transition ticket → **In Progress** |
| Comment added: `/qa-pass` | Issue is in QA / Testing | Transition ticket → **Done** |
| Comment added: `/qa-fail` | Issue is in QA / Testing | Transition ticket → **In Progress** |

---

## Step 10 — Set Up Jira Dashboards

Navigate to: **Dashboards → Create Dashboard** → Name it `Dolphin Sprint Dashboard`

### Recommended Gadgets to Add

| Gadget | Configuration |
|--------|--------------|
| **Sprint Health** | Current sprint, show story counts by status |
| **Assigned to Me** | Issues assigned to current user, current sprint |
| **Burndown Chart** | Current sprint burndown (hours remaining) |
| **Issues in Progress** | All tickets currently "In Progress" |
| **Blocked Issues** | All tickets in "Blocked" status |
| **Bug Statistics** | Bug count by priority, current sprint vs last sprint |
| **Recently Updated** | Issues updated in last 24 hours |
| **Sprint Velocity Chart** | Velocity over last 5 sprints |

---

## Step 11 — Configure Notifications

Navigate to: **Project Settings → Notifications**

### Recommended Notification Rules

| Event | Notify |
|-------|--------|
| Issue created | PM (Reporter) |
| Issue assigned | Assignee |
| Issue status changed | Reporter + Assignee + Watchers |
| Comment added | Reporter + Assignee + Watchers |
| Priority changed to Critical | PM + all developers |
| Blocked status set | PM |
| Issue resolved (Done) | Reporter |

### Google Chat Integration

1. In Google Chat, create a webhook for `#dolphin-dev` space
2. In Jira: **Project Settings → Slack/Chat Notifications** (or use a Jira app)
3. Forward: Critical priority tickets, Blocked tickets, Sprint start/end events

---

## Step 12 — User Roles in Jira

| Role | Jira Permission Level | Who Gets It |
|------|----------------------|-------------|
| Project Admin | Admin | PM |
| Developer | Developer (can edit, transition, comment) | All 3 developers |
| Reporter | Reporter (can create, comment, view) | Business team members |
| Viewer | Browse only | Other stakeholders |

---

## Quick Reference — JQL Queries

Save these as Jira filters for quick access:

```sql
-- Current sprint all tickets
project = DOL AND sprint in openSprints()

-- My open tickets
project = DOL AND assignee = currentUser() AND statusCategory != Done

-- All bugs (open)
project = DOL AND issuetype = Bug AND statusCategory != Done ORDER BY priority DESC

-- Critical items
project = DOL AND priority = Critical AND statusCategory != Done

-- Blocked tickets
project = DOL AND status = Blocked

-- Backlog ready for sprint
project = DOL AND status = Refined AND sprint is EMPTY ORDER BY priority DESC

-- Sprint velocity (last 5 sprints)
project = DOL AND sprint in closedSprints() AND statusCategory = Done

-- Unestimated tickets in backlog
project = DOL AND "Story Points" is EMPTY AND statusCategory != Done AND sprint is EMPTY
```
