# 🐬 Dolphin — Project Management System

Welcome to the Dolphin PM system. This repository contains your complete guide for managing the engineering team with Scrum, handling business requests, and tracking all work in Jira across Dolphin's web apps, APIs, and Flutter mobile app.

## Who This Is For
You — the Project Manager — and the team of 5: Fullstack, Backend, and Frontend developers, plus a QA Engineer and a Designer.

## What Dolphin Ships
- **Portal** (SvelteKit SPA), a landing page, and other web projects — TypeScript monorepo
- **Web APIs** and **Integration APIs** (Google Cloud Functions + Express) — TypeScript monorepo
- **Mobile app** (Flutter) — migrating to its own repo
- All on a **Firebase** backend (Firestore, Auth, Storage, Cloud Functions)

See [`guide/ARCHITECTURE_CONTEXT.md`](guide/ARCHITECTURE_CONTEXT.md) for the full system & delivery map.

---

## 📁 Contents

### `/guide` — PM Playbooks
| File | Description |
|------|-------------|
| [`PM_GUIDE.md`](guide/PM_GUIDE.md) | Your core PM reference: roles, ceremonies schedule, metrics, escalation paths, and Definition of Done |
| [`ARCHITECTURE_CONTEXT.md`](guide/ARCHITECTURE_CONTEXT.md) | System & delivery map: products, repos, Firebase, environments/CI-CD, and cross-product sequencing |
| [`RELEASE_MANAGEMENT.md`](guide/RELEASE_MANAGEMENT.md) | How web/API (continuous CI/CD) and mobile (TestFlight, store review, force update) releases work |
| [`API_VERSIONING.md`](guide/API_VERSIONING.md) | API versioning, breaking-change, and deprecation policy for Web + Integration APIs |
| [`SCRUM_CEREMONIES.md`](guide/SCRUM_CEREMONIES.md) | Step-by-step facilitation guide for every Scrum ceremony |
| [`INTAKE_PROCESS.md`](guide/INTAKE_PROCESS.md) | How to receive, translate, and prioritize requests from the business team |

### `/jira` — Jira Configuration & Templates
| File | Description |
|------|-------------|
| [`JIRA_SETUP.md`](jira/JIRA_SETUP.md) | Complete Jira project setup guide: board config, workflow, issue types, custom fields, GitHub integration |
| [`templates/EPIC.md`](jira/templates/EPIC.md) | Template for large multi-sprint initiatives |
| [`templates/USER_STORY.md`](jira/templates/USER_STORY.md) | Template for business-facing features and enhancements |
| [`templates/BUG_REPORT.md`](jira/templates/BUG_REPORT.md) | Template for defects and unexpected behavior |
| [`templates/TASK.md`](jira/templates/TASK.md) | Template for technical work (DevOps, refactors, config) |
| [`templates/SPIKE.md`](jira/templates/SPIKE.md) | Template for time-boxed research and investigation |

---

## 🚀 Quick Start Checklist

- [ ] Read [`PM_GUIDE.md`](guide/PM_GUIDE.md) and [`ARCHITECTURE_CONTEXT.md`](guide/ARCHITECTURE_CONTEXT.md) end to end
- [ ] **Confirm with the team:** the `feature → staging → main` branching model documented in `ARCHITECTURE_CONTEXT.md` §3
- [ ] **Confirm with the team:** the URL-path API versioning scheme and deprecation windows in [`API_VERSIONING.md`](guide/API_VERSIONING.md)
- [ ] Follow [`JIRA_SETUP.md`](jira/JIRA_SETUP.md) to configure your Jira project (note the corrected staging/main automation)
- [ ] Set up Google Chat spaces/channels per the communication structure in the PM Guide
- [ ] Connect Jira to GitHub (instructions in the Jira Setup Guide)
- [ ] Share [`INTAKE_PROCESS.md`](guide/INTAKE_PROCESS.md) with the business team
- [ ] Schedule your recurring Scrum ceremonies on the team calendar
- [ ] Run your first Backlog Refinement session before Sprint 1

---

## 🗓 Sprint Rhythm at a Glance

> Sprint length: **2 weeks** | Estimation: **Story Points (Fibonacci: 1, 2, 3, 5, 8, 13, 21)** | Priority: **Low / Medium / High / Critical**

| Sprint Day | Day of Week | Event | Duration |
|-----------|-------------|-------|----------|
| Day 1 | Monday | Sprint Planning | 2 hours |
| Days 1–10 | Mon–Fri | Daily Standup | 15 min |
| Day 7 | Wednesday (mid-sprint) | Backlog Refinement | 1 hour |
| Day 10 | Friday | Sprint Review | 45 min |
| Day 10 | Friday | Sprint Retrospective | 45 min |

---

## 🏷 Ticket Naming Convention

| Type | Jira ID Format | Branch Format |
|------|---------------|---------------|
| Epic | `DOL-[id]` | N/A |
| Story | `DOL-[id]` | `DOL-[id]/short-description` |
| Bug | `DOL-[id]` | `DOL-[id]/fix-short-description` |
| Task | `DOL-[id]` | `DOL-[id]/task-short-description` |
| Spike | `DOL-[id]` | `DOL-[id]/spike-short-description` |

All PRs must reference the ticket: `DOL-123: Brief description of change`
