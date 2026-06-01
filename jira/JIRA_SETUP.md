# 🐬 Dolphin — Jira Project Setup Guide

> **Who this is for:** A first-time Jira user setting up a Scrum board for a small dev team.
> **Platform:** Jira Software Cloud (app.atlassian.com)
> **Team size:** 4 people (PM + 3 developers)
> **Free tier:** Supports up to 10 users at no cost — no credit card needed.
>
> This guide walks through every step and screen click needed to configure Jira for the Dolphin team. Every section includes a **"Why we do this"** explanation so you understand the purpose behind each decision — not just what to click.

---

## Understanding Jira's Structure Before You Start

Before touching any settings, it helps to understand the four key layers of Jira:

```
JIRA SITE (your atlassian.net account)
  └── PROJECT (e.g., "Dolphin" — all work lives here)
        └── BOARD (the visual Scrum board — columns of tickets)
              └── ISSUES (the individual tickets: Epics, Stories, Bugs, Tasks)
```

Think of a **Project** as a filing cabinet, the **Board** as the whiteboard on the wall showing what's happening right now, and **Issues** as the sticky notes on that board.

---

## Step 1 — Create Your Jira Account and Site

### 1.1 — Sign Up

1. Open a browser and go to: `https://www.atlassian.com/software/jira`
2. Click the blue **Get it free** button
3. On the sign-up page, enter your work email address and click **Continue**
4. Check your email for a verification link and click it
5. Create a password when prompted

### 1.2 — Create Your Site

After verifying your email, Atlassian will ask you to create a **site**. A "site" is your company's private workspace URL.

1. Enter a site name. Use something professional: `dolphin-dev` or `dolphin-tech`
2. Your URL will be: `https://dolphin-dev.atlassian.net` — bookmark this
3. When asked *"What does your team do?"* → select **Software development**
4. When asked *"How large is your team?"* → select **2–15 people**
5. Click **Agree** on the terms and continue

> 💡 **Why:** The site name becomes part of your permanent URL and is visible in links you share, so choose something recognizable. You cannot change it later without contacting Atlassian support.

### 1.3 — Invite Your Team

Before creating the project, invite your three developers so you can assign them immediately:

1. Click your **profile icon** (top right) → **Atlassian account settings**
2. Go back to `https://dolphin-dev.atlassian.net/admin`
3. Click **Users** in the left sidebar → **Invite users**
4. Enter each developer's work email address
5. Set their role to **Member** for now (you'll assign project roles later)
6. Click **Send invites**

Developers will receive an email to join. They must accept before you can assign tickets to them.

> 💡 **Why:** Inviting developers early means you can assign them work the moment the project is set up. On the Free tier, all users are treated as members — there's no cost per seat up to 10 users.

---

## Step 2 — Create the Project

A Jira "Project" is the container for all of Dolphin's work — every ticket, sprint, board, and report lives inside it.

### 2.1 — Create a New Project

1. From the Jira home screen, look at the **left sidebar** — you'll see a **Projects** section
2. Click **Create project** (or look for a **+** icon next to "Projects")
3. You'll see a template selection screen

### 2.2 — Choose the Right Template

On the template screen, you'll see several options. Find the **Software development** section.

1. Click on **Scrum** (it shows a sprint board icon)
2. Read the description: *"Plan work in sprints, prioritize your backlog, and ship iteratively"* — this is exactly what we want
3. Click **Select** → then click **Use template**

> 💡 **Why Scrum and not Kanban?** Kanban is a continuous flow model — good for operations or support teams. Scrum uses fixed time-boxes (sprints) which gives your small team a regular rhythm: plan → build → review → improve. With a business team expecting updates, the 2-week sprint cadence gives you natural checkpoints to show progress.

### 2.3 — Fill In Project Details

You'll see a form with these fields:

| Field | What to Enter | Why |
|-------|-------------|-----|
| **Project name** | `Dolphin` | The display name across all of Jira |
| **Project key** | `DOL` | A short prefix auto-added to every ticket ID (e.g., `DOL-1`, `DOL-42`). Keep it 3 letters. Jira auto-suggests this from your project name — change it to `DOL` if it suggests something else. |
| **Project lead** | Select your name | The person responsible for the project — that's you as PM |
| **Access** | Private | Only invited team members can see this project |

Click **Create project** when done.

> 💡 **Why "DOL" as the key?** The project key appears in every ticket ID, every Git branch name (`DOL-42/add-login`), and every PR title. Short and recognizable keys reduce friction. You cannot change the project key after creating tickets, so get it right now.

### 2.4 — What You Should See

After clicking Create, Jira will take you to a **Board view** that looks like a Kanban board with three default columns: **To Do**, **In Progress**, and **Done**. Don't worry — we're going to replace these with the right columns in Step 4.

---

## Step 3 — Configure Issue Types

Issue types are the different *kinds* of tickets you can create. Jira gives you defaults, but we need to add some and understand what each one is for.

### 3.1 — Navigate to Issue Types

1. In the left sidebar, click **Project settings** (gear icon, near the bottom)
2. In the Project Settings menu on the left, click **Issue types**
3. You'll see the current issue types — likely: Epic, Story, Task, Sub-task, Bug

### 3.2 — Understanding Each Issue Type

Here's what each type means and when to use it. This is important because using the wrong type makes your backlog confusing:

| Issue Type | When to Use It | Example |
|-----------|---------------|---------|
| **Epic** | A large body of work that spans multiple sprints. Think of it as a theme or initiative. Epics are never "worked on" directly — they contain Stories. | "User Authentication System", "Mobile App v1", "Payment Integration" |
| **Story** | A user-facing feature that can be completed in one sprint. This is your most common ticket type. Always written from the user's perspective. | "As a user, I want to log in with Google so I can access my account" |
| **Bug** | Something that is broken or not working as intended. Different from a Story because it fixes existing behavior rather than adding new behavior. | "Password reset email is not being sent on production" |
| **Task** | Technical work that has no direct user-facing value but is necessary. Used for DevOps, refactoring, database migrations, documentation. | "Set up CI/CD pipeline", "Upgrade Node.js to v20", "Add database indexes" |
| **Spike** | A time-boxed research ticket. Used when the team doesn't know enough to estimate a story yet. Always produces a written output. | "Research WebSocket vs SSE for real-time notifications" |
| **Sub-task** | A breakdown of a Story or Task. Use when a single ticket needs to be split across multiple developers or has very distinct phases. | Under a Story: "Sub-task: Design database schema", "Sub-task: Build API endpoint", "Sub-task: Build UI component" |

> 💡 **Why have separate types?** Because each type has a different lifecycle, different fields, and a different meaning on the board. Mixing bugs in with stories makes it impossible to track your bug rate over time. Mixing technical tasks with user stories makes velocity tracking meaningless. Clean types = clean data = useful reports.

### 3.3 — Add the Spike Issue Type

Jira doesn't include "Spike" by default. Here's how to add it:

1. On the **Issue Types** page, look for a button labeled **Add issue type** or scroll down to find it
2. Click **Add issue type**
3. Fill in:
   - **Name:** `Spike`
   - **Description:** `Time-boxed research or investigation. Always produces a documented output.`
   - **Type:** Standard (not sub-task)
4. Click **Save**

You should now see: Epic, Story, Bug, Task, Spike, Sub-task listed.

> 💡 **Why add Spike?** Without a Spike type, teams either skip research (bad — leads to poorly estimated work) or log research as a Task (bad — pollutes your velocity with non-feature work). A dedicated Spike type makes research visible and shows the business team that investigation *is* work.

---

## Step 4 — Configure the Workflow

The **workflow** defines the journey a ticket takes from creation to completion. This maps directly to the columns you see on the Scrum board.

### 4.1 — Understanding Workflows

Think of a workflow as a flowchart. Each "box" in the flowchart is a **status** (a column on the board). The arrows between boxes are **transitions** (the allowed moves between statuses).

By default, Jira gives you: **To Do → In Progress → Done**

That's too simple for software development. We need a workflow that reflects how code actually gets shipped.

### 4.2 — Navigate to Workflows

1. Go to **Project Settings** → click **Workflows** in the left sidebar
2. You'll see the active workflow (likely called "Software Simplified Workflow")
3. Click the **pencil icon ✏️** or **Edit** next to it
4. Jira opens a visual workflow editor — you'll see boxes (statuses) connected by arrows (transitions)

> ⚠️ **Important:** Jira Cloud uses a simplified workflow editor by default. If you see a "Diagram" view, you're in the right place. If you don't see an edit option, you may need to switch to a "Company-managed project" — but for the Free tier, the simplified view is usually the default and works fine.

### 4.3 — The Statuses We Need and Why

We need **8 statuses**. Here's each one, what it means, and why it exists:

---

**1. Backlog**
- **What it is:** Where all new tickets land when first created. This is the raw, unprocessed pile.
- **Who moves tickets here:** Automatically — all new tickets start here.
- **Why it exists:** The backlog is your team's "inbox." Not everything in the backlog will be worked on this sprint — it's a prioritized list of everything that *could* be done. Having a clear Backlog status separates "things we might do someday" from "things we committed to this sprint."

---

**2. Refined**
- **What it is:** The ticket has been reviewed in Backlog Refinement — it has acceptance criteria, a story point estimate, and all the information a developer needs to start work.
- **Who moves tickets here:** PM, after the Backlog Refinement session.
- **Why it exists:** Without a "Refined" status, developers get assigned unclear tickets and waste time figuring out what they're supposed to build. The Refined status is a quality gate — it means "this ticket is ready to be worked on."

---

**3. In Sprint**
- **What it is:** The ticket has been selected for the current sprint but the developer hasn't started it yet.
- **Who moves tickets here:** PM, during Sprint Planning.
- **Why it exists:** This separates "we've committed to doing this sprint" from "someone is actively working on it right now." It's the sprint's to-do list.

---

**4. In Progress**
- **What it is:** A developer has actively started working on this ticket. They've created a branch in GitHub.
- **Who moves tickets here:** The developer.
- **Why it exists:** This is the most important status for the PM to watch. Too many tickets in "In Progress" at once means developers are context-switching. Ideally each developer has only 1–2 tickets here at a time.

---

**5. Code Review**
- **What it is:** The developer has finished coding and opened a Pull Request (PR) on GitHub. The ticket is waiting for another developer to review the code.
- **Who moves tickets here:** The developer who opened the PR.
- **Why it exists:** Code review is a real phase of work that can block progress for hours or days. Making it a visible status lets the PM see when tickets are "stuck" waiting for reviews and prompt the team to prioritize reviews during standup.

---

**6. QA / Testing**
- **What it is:** The PR has been approved and merged. The feature is now deployed to a staging environment and ready for testing.
- **Who moves tickets here:** The developer after the PR is merged.
- **Why it exists:** "Done coding" is not the same as "done." QA catches bugs before they reach real users. This status makes testing a visible, deliberate step rather than something that gets skipped when the team is in a hurry.

---

**7. Done**
- **What it is:** QA has passed, the ticket meets all acceptance criteria, and the feature is ready to ship (or has already shipped) to production.
- **Who moves tickets here:** PM or QA verifier after confirming the acceptance criteria are met.
- **Why it exists:** This is the "Definition of Done" enforced in the workflow. Only tickets that have truly passed QA reach here — this keeps your sprint metrics honest.

---

**8. Blocked**
- **What it is:** Work on this ticket has stopped because of something outside the developer's control (waiting for a business decision, dependent on another ticket, waiting for a third-party API).
- **Who moves tickets here:** Developer or PM.
- **Why it exists:** Without a Blocked status, blocked tickets look like "In Progress" tickets — the board lies about the team's true state. A visible Blocked column means the PM can immediately see what needs to be escalated.

---

### 4.4 — Adding Statuses in the Workflow Editor

In the visual workflow editor:

1. Look for an **Add status** button (or right-click on the canvas in some versions)
2. For each status you need to add, click **Add status**, enter the name, and choose a category:
   - **To Do** category: Backlog, Refined, In Sprint
   - **In Progress** category: In Progress, Code Review, QA / Testing, Blocked
   - **Done** category: Done
3. After adding each status, you'll see a new box appear on the canvas

> 💡 **Why do categories matter?** Jira uses these three categories (To Do, In Progress, Done) for its built-in charts (burndown, velocity). Assigning the right category ensures your reports are accurate. The "Done" category is what Jira counts as completed work.

### 4.5 — Adding Transitions (the Arrows)

Transitions define which moves are allowed between statuses. In the workflow editor, drag from one status box to another to create a transition. Add these transitions:

```
Backlog        ──→  Refined         (PM refines the ticket)
Refined        ──→  In Sprint       (ticket selected in sprint planning)
In Sprint      ──→  In Progress     (developer starts work)
In Progress    ──→  Code Review     (developer opens PR)
In Progress    ──→  Blocked         (blocker encountered)
Blocked        ──→  In Progress     (blocker resolved)
Code Review    ──→  QA / Testing    (PR approved and merged)
Code Review    ──→  In Progress     (PR rejected — needs rework)
QA / Testing   ──→  Done            (QA approved)
QA / Testing   ──→  In Progress     (QA failed — needs rework)
Done           ──→  In Progress     (reopened — bug found post-release)
```

> 💡 **Why define transitions?** Without defined transitions, anyone can move any ticket anywhere — creating a chaotic board where tickets skip QA or jump backwards unexpectedly. Transitions enforce your team's agreed process. When a developer tries to mark a ticket "Done" without going through QA, Jira won't allow it.

### 4.6 — Publish the Workflow

1. Click **Publish** or **Save** at the top of the workflow editor
2. If prompted to associate the workflow with your project, confirm it
3. Go back to your Board — you should now see your new columns

---

## Step 5 — Configure the Scrum Board

The board is what you'll look at every single day. Getting it right is important.

### 5.1 — Navigate to Board Settings

1. From your project, click **Board** in the left sidebar to view the board
2. Click the **...** (more options) menu in the top right of the board, or look for a **Board settings** link
3. You should see settings for: Columns, Swimlanes, Quick Filters, Card Layout

### 5.2 — Map Columns to Your Workflow Statuses

In **Board settings → Columns**, you map your workflow statuses to the visual columns on the board. Some statuses (like Backlog and Refined) live in the Backlog view, not the sprint board — that's intentional.

| Board Column | Maps to These Statuses |
|-------------|----------------------|
| **To Do** (rename to "Ready") | In Sprint |
| **In Progress** | In Progress |
| **Code Review** | Code Review |
| **QA / Testing** | QA / Testing |
| **Done** | Done |

> ⚠️ Note: **Backlog**, **Refined**, and **Blocked** statuses are visible in the Backlog view and as filtered views, but typically not shown as main board columns. Blocked tickets appear in their last column with a 🚧 flag when you add the right card color (covered below).

> 💡 **Why keep Backlog out of the board?** The sprint board should only show work that's actively in the current sprint. Showing the entire backlog on the board would make it unreadable. The Backlog view (accessible from the left sidebar) is where you manage everything not yet in a sprint.

### 5.3 — Set Up Swimlanes

Swimlanes are horizontal rows that group tickets on the board.

1. In **Board settings → Swimlanes**
2. Set **Base Swimlanes on:** `Epics`
3. Click **Save**

> 💡 **Why swimlanes by Epic?** With multiple products (web, mobile, API, internal tools), your sprint will have tickets from several different feature areas. Swimlanes by Epic let you instantly see "we're working on 3 things for User Auth, 2 things for the Payment feature, and 1 infrastructure task" — instead of a mixed list of 6 tickets with no context.

### 5.4 — Configure Card Colors

Card colors help visually distinguish ticket types at a glance.

1. In **Board settings → Card colors**
2. Set colors based on **Issue types** or **Priorities**:

| Color | Rule | Why |
|-------|------|-----|
| 🔴 Red | Priority = Critical | Instantly visible — needs attention now |
| 🟠 Orange | Priority = High | High urgency items stand out |
| 🟡 Yellow | Issue type = Bug | Bugs are always visually distinct from features |
| 🔵 Blue | Issue type = Story | Default for feature work |
| ⚫ Gray | Issue type = Task | Technical work is less visually prominent |
| 🟣 Purple | Issue type = Spike | Research tickets are clearly different |

### 5.5 — Add Quick Filters

Quick Filters are one-click buttons above the board that let you instantly filter what you're seeing.

1. In **Board settings → Quick Filters**
2. Click **Add quick filter** for each:

| Button Label | JQL (Jira Query Language) | Why Useful |
|-------------|--------------------------|-----------|
| **My Tickets** | `assignee = currentUser()` | Each developer clicks this to see only their work |
| **Bugs** | `issuetype = Bug` | PM can quickly see all bugs in the sprint |
| **Blocked** | `status = Blocked` | Instantly surface everything that's stuck |
| **Critical** | `priority = Critical` | See only the most urgent items |
| **In Review** | `status = "Code Review"` | See what PRs are waiting to be reviewed |
| **Frontend** | `labels = frontend` | See only frontend work |
| **Backend** | `labels = backend` | See only backend work |

> 💡 **What is JQL?** JQL (Jira Query Language) is Jira's search syntax. It works like a simple SQL query for filtering tickets. You don't need to master it — just use the queries provided in this guide. The Quick Filters section shows a text box where you paste these exact phrases.

---

## Step 6 — Configure Custom Fields

Custom fields let you capture information specific to your team that Jira doesn't include by default.

### 6.1 — Navigate to Fields

1. Click **Project Settings** → **Issue layout** (or in some Jira versions: **Fields**)
2. You'll see the fields currently shown on tickets

### 6.2 — Fields to Add and Why

> **Note:** Some of these fields may already exist in Jira (like Priority and Story Points) — you just need to ensure they're visible on your issue screens. Others need to be created.

**Story Points** *(likely already exists — verify it's enabled)*
- Navigate to **Project Settings → Fields**
- Find "Story Points" and ensure it's checked/active
- In Jira Software, Story Points are a built-in number field
- Go to **Project Settings → Estimation** and confirm the estimation statistic is set to **Story Points**

> 💡 **Why Story Points as the estimation field?** Jira's Burndown Chart, Velocity Chart, and Sprint reports are built around this field. If you use a custom "Hours" field instead, all of Jira's built-in reporting stops working. Story Points keep your metrics connected to Jira's native tools.

**Priority** *(built-in — configure the options)*
- Go to **Jira settings** (the gear icon in the top right navigation bar, NOT project settings)
- Click **Issues** → **Priorities**
- The default priorities are Highest/High/Medium/Low/Lowest — we want to replace these
- Click each one and rename/reorder to: **Critical**, **High**, **Medium**, **Low**
- You can also change the icons and colors here (red for Critical, orange for High, etc.)

> 💡 **Why rename priorities?** "Highest" and "Lowest" are ambiguous. "Critical" immediately communicates "something is on fire." Clear language prevents tickets from being incorrectly prioritized.

**Creating a Custom Field: Affected Product**
1. Click the gear icon (top right) → **Issues** → **Custom fields**
2. Click **Create custom field**
3. Select **Checkboxes** (allows multi-select)
4. Name it: `Affected Product`
5. Add options: `Web Application`, `Mobile Application`, `API / Microservice`, `Internal Tool`
6. Click **Create** and then associate it with your Bug screen

**Creating a Custom Field: Environment**
1. Repeat the process above
2. Select **Select List (single choice)**
3. Name it: `Environment`
4. Add options: `Production`, `Staging`, `Development`
5. Associate it with your Bug screen

**Creating a Custom Field: Business Requester**
1. Select **Text Field (single line)**
2. Name it: `Business Requester`
3. Associate it with your Story screen

> 💡 **Why "Business Requester"?** When a business stakeholder asks "what happened to my feature request?", you can search Jira by their name and pull up every ticket they've submitted. It creates accountability and traceability without needing a separate system.

### 6.3 — Summary of All Fields

| Field | Type | Which Screens | Why You Need It |
|-------|------|--------------|----------------|
| **Story Points** | Number | Story, Task, Bug, Spike | Drives all Jira velocity and burndown charts |
| **Priority** | Select | All | Critical/High/Medium/Low — drives triage decisions |
| **Epic Link** | Epic link | Story, Bug, Task, Spike | Groups work under feature themes |
| **Sprint** | Sprint | All | Auto-assigned when ticket enters sprint |
| **Affected Product** | Multi-select (custom) | Bug | Tells you which product is impacted |
| **Environment** | Select (custom) | Bug | Was this found in Production or Staging? |
| **Business Requester** | Text (custom) | Story | Who asked for this feature? |
| **Severity** | Select (custom) | Bug | How bad is the technical damage? (vs Priority = urgency) |

---

## Step 7 — Add Labels

Labels are free-form tags you add to any ticket. Unlike issue types (which define *what* the ticket is), labels define *who owns it* and *what domain it touches*.

### 7.1 — How Labels Work in Jira

Labels are typed directly into the Label field on any ticket — Jira suggests matching labels as you type. There's no pre-configuration needed; just ensure your team uses consistent names.

### 7.2 — Establish These Standard Labels

Share this list with your developers so everyone uses the same label names (capitalization and spelling matter):

| Label | When to Apply | Who It Helps |
|-------|--------------|-------------|
| `frontend` | Work is primarily in the UI layer | PM can filter board to see Frontend Dev's scope |
| `backend` | Work is primarily in services/APIs/DB | PM can filter board to see Backend Dev's scope |
| `fullstack` | Work spans both UI and API | Indicates cross-team dependency |
| `mobile` | Work affects the mobile application | Separate from `web` — different team capacity |
| `web` | Work affects the web application | |
| `api` | Work is building or changing an API endpoint | |
| `internal-tool` | Work affects internal dashboards/tools | Lower business visibility but still tracked |
| `infrastructure` | DevOps, CI/CD, cloud, server config | Invisible to users but critical |
| `database` | Schema changes, migrations, queries | High-risk — needs careful review |
| `security` | Authentication, permissions, vulnerabilities | Always High or Critical priority |
| `performance` | Speed improvements, caching, load testing | |
| `tech-debt` | Refactoring, cleanup, paying down past shortcuts | |
| `hotfix` | Emergency fix going to production immediately | Triggers expedited process |

> 💡 **Why labels instead of just relying on assignee?** A ticket assigned to the Backend Dev might have a small frontend change needed too. Labels capture the *domain* of work independently of who's doing it. The PM can filter `label = database` across all developers to see every DB change happening this sprint — useful for spotting risks.

---

## Step 8 — Set Up Versions (Releases)

Versions in Jira represent a release or a sprint's worth of deliverables. Linking tickets to a version lets you generate release notes automatically.

### 8.1 — Navigate to Versions

1. Click **Project Settings** in the left sidebar
2. Click **Releases** (or **Versions** in older Jira versions)

### 8.2 — Create Your First Version

1. Click **Create version**
2. Fill in:
   - **Name:** `Sprint 1 — June 2026`
   - **Start date:** The Monday your first sprint begins
   - **Release date:** The Friday your first sprint ends (2 weeks later)
   - **Description:** Optional — a brief goal for the sprint
3. Click **Save**

Repeat this before each new sprint begins.

### 8.3 — How to Use Versions Day-to-Day

When creating or editing a ticket, look for the **Fix Version/s** field and select the current sprint version. This tells Jira "this ticket is expected to be done in Sprint 1."

At sprint end, go to **Releases** and click your version to see a summary of what was completed vs. what was not — perfect for generating release notes for the business team.

> 💡 **Why bother with versions on a small team?** Two reasons: (1) It takes 5 seconds to set and saves you 30 minutes of manually compiling "what shipped this sprint." (2) It gives the business team a trackable history — "here's everything we shipped in Sprint 1, Sprint 2, Sprint 3."

---

## Step 9 — Create Your First Sprint

Now that the project is configured, it's time to create the actual sprint structure.

### 9.1 — Understanding the Backlog View

In the left sidebar, click **Backlog**. This is different from the board:

- **Board** = tickets in the *current sprint* arranged by status column
- **Backlog** = *all tickets* not yet in a sprint, sorted by priority

The Backlog view is where you'll spend most of your time as PM — creating tickets, refining them, and moving them into sprints.

### 9.2 — Create Your First Sprint

1. In the Backlog view, look for a **Create Sprint** button near the top
2. Jira creates a new section labeled "Sprint 1" with a date range
3. Click the **...** (more options) on the sprint header → **Edit sprint**
4. Fill in:
   - **Sprint name:** `Dolphin Sprint 1 — [Start Date] to [End Date]`
     (e.g., `Dolphin Sprint 1 — Jun 9 to Jun 20, 2026`)
   - **Start date:** The Monday you plan to begin
   - **End date:** The Friday 2 weeks later
   - **Sprint goal:** Write one sentence about what the sprint will accomplish
     (e.g., *"Enable users to register, log in, and reset their password."*)
5. Click **Update**

> 💡 **Why write a sprint goal?** The goal is the team's north star for the sprint. When a new urgent request comes in mid-sprint, the PM asks: *"Does this help us achieve the sprint goal?"* If no, it goes in the backlog. The goal protects the team from scope creep.

### 9.3 — Add Tickets to the Sprint

1. In the Backlog view, you'll see your backlog items listed below the sprint section
2. **Right-click** on any refined ticket → **Move to Sprint** → select Sprint 1
3. Alternatively, **drag and drop** tickets from the backlog section up into the sprint section
4. Only move tickets that meet the Definition of Ready (estimated, AC written, refined)

### 9.4 — Start the Sprint

Once Sprint Planning is done and tickets are in the sprint:

1. Click the **Start Sprint** button on the sprint header in the Backlog view
2. Confirm the start and end dates
3. Click **Start**

The board will now show only the tickets in the active sprint.

---

## Step 10 — Invite Users and Set Permissions

### 10.1 — Add Developers to the Project

If you already invited them at the site level (Step 1), they may already have access. To verify and assign project roles:

1. Go to **Project Settings → Members** (or **People**)
2. You should see yourself listed
3. Click **Add members** and add your three developers by email or name
4. Set their role to **Member** (on Free tier there's typically just one role)

### 10.2 — Project Roles on Paid Tier

If you upgrade to a paid tier later, you'll want these roles:

| Person | Role | What They Can Do |
|--------|------|-----------------|
| **You (PM)** | **Project Admin** | Create/edit/delete anything, configure settings |
| **Developers** | **Developer** | Create issues, edit issues, transition statuses, log work |
| **Business Team** | **Reporter** | Create issues and view the board — cannot edit or transition |
| **Stakeholders** | **Viewer** | Read-only access |

> 💡 **Why restrict business team to Reporter?** Business stakeholders should be able to *see* progress and *submit* requests, but not move tickets around or change priorities. That's the PM's job. Giving everyone full access leads to tickets being closed prematurely, priorities changed without coordination, or board columns being a mess.

---

## Step 11 — Set Up Jira Automation

Automation rules connect Jira to GitHub so that tickets move automatically when code is committed. This eliminates the need for developers to manually update Jira after every PR.

### 11.1 — Navigate to Automation

1. Go to **Project Settings → Automation**
2. You'll see a list of any existing rules and a **Create rule** button

### 11.2 — Connect GitHub to Jira (Do This First)

Before the automation rules work, Jira needs to know about your GitHub account:

1. Click the **gear icon** (top right — Jira settings, not project settings)
2. Click **Products**
3. Find **Jira Software** and look for **GitHub integration** or **DVCS accounts**
4. Click **Link GitHub account** or **Add account**
5. Select **GitHub** → log in to GitHub when prompted
6. Click **Authorize Atlassian** to give Jira permission to read your GitHub activity
7. Select the Dolphin GitHub organization/account
8. Choose **All repositories** or select specific repos

> 💡 **Why integrate GitHub?** Without this, Jira has no idea what's happening in the codebase. With it, Jira can show you the commit history, PR status, and branch name directly on each ticket. Developers don't need to manually copy links — Jira finds them automatically when the branch name contains the ticket ID (e.g., `DOL-42/add-login`).

### 11.3 — Create Automation Rules

Now create the automation rules. Each rule has a **Trigger**, an optional **Condition**, and an **Action**.

---

**Rule 1: When a Branch is Created → Move to In Progress**

This fires when a developer creates a Git branch containing the ticket ID.

1. Click **Create rule**
2. **Trigger:** Select **"Branch created"** from the Development category
3. **Condition:** *(no condition needed — any branch creation triggers it)*
4. **Action:** Select **"Transition issue"** → select status **"In Progress"**
5. Name the rule: `Auto: Branch created → In Progress`
6. Click **Save and enable**

---

**Rule 2: When a PR is Opened → Move to Code Review**

1. **Trigger:** `Pull request created`
2. **Condition:** *(none needed)*
3. **Action:** `Transition issue` → **Code Review**
4. Name: `Auto: PR opened → Code Review`

---

**Rule 3: When a PR is Merged → Move to QA / Testing**

1. **Trigger:** `Pull request merged`
2. **Condition:** *(none)*
3. **Action:** `Transition issue` → **QA / Testing**
4. Name: `Auto: PR merged → QA/Testing`

---

**Rule 4: When a PR is Declined/Closed → Back to In Progress**

1. **Trigger:** `Pull request declined`
2. **Condition:** *(none)*
3. **Action:** `Transition issue` → **In Progress**
4. Name: `Auto: PR declined → In Progress`

---

**Rule 5: Critical Bug Alert → Notify PM**

1. **Trigger:** `Issue created`
2. **Condition:** `Issue matches` → `priority = Critical AND issuetype = Bug`
3. **Action:** `Send email` → enter your email address
4. Name: `Alert: Critical bug created`

> 💡 **Why automate transitions?** Developers are focused on coding — not on updating project management tools. Every manual step you add to their workflow is a step that will be forgotten. Automation keeps the board accurate in real time without requiring discipline from the team. An accurate board means you always know the true state of the sprint.

---

## Step 12 — Set Up Your Dashboard

The Dashboard is your personal command center in Jira — a customizable home page showing the data most important to you as PM.

### 12.1 — Create the Dashboard

1. In the top navigation bar, click **Dashboards** → **Create dashboard**
2. Name it: `Dolphin Sprint Dashboard`
3. Set visibility to **Private** (only you see it) or **Team** (all project members)
4. Click **Create**

### 12.2 — Add Gadgets

Gadgets are the widgets you add to the dashboard. Click **Add gadget** to see the full list. Add these and configure as described:

---

**Burndown Chart**
- What it shows: Hours/points remaining in the sprint vs. an ideal trend line
- Configuration: Select your project (DOL), set to current sprint
- Why it matters: The burndown is the single most important chart for a PM. If the line is above the ideal trend, the team is behind. If it drops suddenly, work was added or estimated incorrectly. Check this every morning.

---

**Sprint Health Gadget** (or "Sprint Report")
- What it shows: Number of tickets in each status column
- Configuration: Project = DOL, Sprint = current
- Why it matters: Gives you a quick summary without looking at the board

---

**Assigned to Me**
- What it shows: Open tickets assigned to you (the PM)
- Configuration: Default
- Why it matters: You'll have tickets assigned to you too — refinement tasks, documentation, research spikes

---

**Issues in Progress**
- What it shows: All tickets currently "In Progress"
- Configuration: Filter → `project = DOL AND status = "In Progress"`
- Why it matters: Lets you verify each developer has a healthy number of tickets in progress (not too many = context switching)

---

**Blocked Issues**
- What it shows: All tickets in "Blocked" status
- Configuration: Filter → `project = DOL AND status = Blocked`
- Why it matters: Blocked tickets are the PM's #1 priority to resolve. They should never sit unaddressed for more than 24 hours.

---

**Velocity Chart**
- What it shows: Story points completed per sprint over time
- Configuration: Project = DOL, last 8 sprints
- Why it matters: Your velocity trend is how you predict future sprint capacity. After 3 sprints, you'll have a reliable average.

---

**Bug Statistics**
- What it shows: Bug count by status and priority
- Configuration: Project = DOL
- Why it matters: A rising bug count sprint over sprint signals quality problems that need to be addressed before they become a crisis.

---

## Step 13 — Configure Notifications

### 13.1 — Navigate to Notifications

1. Go to **Project Settings → Notifications**
2. You'll see a notification scheme with a list of events and who gets notified

### 13.2 — Recommended Notification Setup

Jira sends email notifications by default. Configure these events:

| Event | Who to Notify | Why |
|-------|-------------|-----|
| **Issue created** | Reporter (person who made the ticket) | Confirms their request was received |
| **Issue assigned** | Assignee | Developer knows work is coming their way |
| **Issue resolved (Done)** | Reporter | Business stakeholder is told when their feature is done |
| **Priority changed to Critical** | Project Admin (PM) + All developers | Everyone drops what they're doing to see this |
| **Status changed to Blocked** | Project Admin (PM) | PM needs to act on this immediately |
| **Comment added** | Reporter + Assignee | Keeps conversation participants in the loop |

> 💡 **Why not notify everyone on everything?** Notification overload is real. If developers receive 20 Jira emails a day about things that don't involve them, they'll start ignoring all Jira emails — including the important ones. Targeted notifications keep the signal high.

### 13.3 — Google Chat Webhook Integration

To get Jira notifications in your `#dolphin-dev` Google Chat space:

**In Google Chat:**
1. Open the `#dolphin-dev` space
2. Click the space name at the top → **Apps & Integrations** → **Webhooks**
3. Click **Add webhooks** → name it `Jira Alerts` → click **Save**
4. Copy the **Webhook URL** that is generated

**In Jira:**
1. Go to **Project Settings → Automation**
2. Create a new rule:
   - **Trigger:** `Issue transitioned` — set to trigger on any transition to **Blocked** or **Critical** priority
   - **Action:** `Send web request`
   - **URL:** Paste the Google Chat webhook URL
   - **Headers:** `Content-Type: application/json`
   - **Body:**
     ```json
     {
       "text": "🚨 *{{issue.key}}* — {{issue.summary}}\nStatus: {{issue.status.name}}\nPriority: {{issue.priority.name}}\nAssignee: {{issue.assignee.displayName}}\nhttps://dolphin-dev.atlassian.net/browse/{{issue.key}}"
     }
     ```
3. Name the rule: `Google Chat: Blocked or Critical alert`
4. Save and enable

---

## Step 14 — Creating Your First Tickets

Now everything is configured. Here's how to actually create a ticket.

### 14.1 — Creating a Story

1. Click **Create** in the top navigation bar (keyboard shortcut: `C`)
2. A dialog box appears. Fill in:
   - **Project:** Dolphin
   - **Issue type:** Story
   - **Summary (title):** Write in user story format: *"As a [user], I want to [action] so that [benefit]"*
     Example: *"As a registered user, I want to reset my password via email so I can regain access if I forget it"*
   - **Description:** Click the Description field and paste content from the `USER_STORY.md` template (in the `/jira/templates` folder of this repo)
   - **Priority:** Select from Low/Medium/High/Critical
   - **Epic Link:** Click this field, type the name of the relevant Epic, and select it
   - **Reporter:** Should default to your name
   - **Assignee:** Leave unassigned until Sprint Planning
   - **Story Points:** Leave blank until Backlog Refinement (when the team estimates)
3. Click **Create**

### 14.2 — Creating an Epic

Epics must be created before the Stories that belong to them.

1. Click **Create**
2. Set issue type to **Epic**
3. Fill in the **Epic Name** field (this is the short label shown on child stories)
   Example: `User Authentication`
4. Fill in the **Summary:** A brief description of the initiative
5. Paste the content from `EPIC.md` template into the Description
6. Click **Create**

### 14.3 — Creating a Bug

1. Click **Create**
2. Set issue type to **Bug**
3. Fill in the Summary as: `[Short description of what is broken]`
   Example: `Password reset email not sent when email contains a + symbol`
4. Paste the content from `BUG_REPORT.md` template into the Description
5. Set **Priority** (use the severity/impact to decide)
6. Set **Affected Product** and **Environment** custom fields
7. Click **Create**

---

## Step 15 — Daily Workflow in Jira (How to Use It Every Day)

### As PM — Your Daily Checklist in Jira

Every morning, open Jira and do this in order:

1. **Check the Burndown Chart** on your dashboard — is the team on track?
2. **Check Blocked tickets** — anything blocked needs to be escalated before standup
3. **Check Code Review tickets** — anything sitting in Code Review for more than 1 day needs a nudge
4. **Review the Board** — do the ticket statuses match what developers said in standup?
5. **Check for new incoming bugs** — triage and prioritize any bugs created since yesterday

### As a Developer — How They Should Use Jira

Share this with your team:

1. When you start work on a ticket → move it to **In Progress** (or it moves automatically when you create the branch)
2. Name your branch: `DOL-[id]/short-description` → Jira links it automatically
3. When you open a PR → PR title must start with `DOL-[id]:` → Jira links the PR to the ticket
4. Add comments on the ticket for anything important: technical decisions, blockers, questions
5. **Never close your own tickets** — QA or PM does that after verification

---

## Quick Reference — JQL Queries

JQL (Jira Query Language) is how you search and filter tickets. You can run these in the **Search bar** at the top (press `/` to focus it) or save them as Filters.

To save a filter: Run a search → click **Save as** → give it a name → it appears under Filters in the sidebar.

```
Current sprint — all tickets:
project = DOL AND sprint in openSprints()

My open tickets (developers use this):
project = DOL AND assignee = currentUser() AND statusCategory != Done

All open bugs by priority:
project = DOL AND issuetype = Bug AND statusCategory != Done ORDER BY priority DESC

Everything Critical right now:
project = DOL AND priority = Critical AND statusCategory != Done

All blocked tickets:
project = DOL AND status = Blocked

Backlog ready for next sprint (refined, not yet in a sprint):
project = DOL AND status = Refined AND sprint is EMPTY ORDER BY priority DESC

Tickets in Code Review (need reviewing):
project = DOL AND status = "Code Review" ORDER BY updated ASC

Tickets without story point estimates:
project = DOL AND "Story Points" is EMPTY AND statusCategory != Done AND sprint is EMPTY

Everything shipped in the last sprint:
project = DOL AND sprint in closedSprints() AND statusCategory = Done ORDER BY sprint DESC
```
