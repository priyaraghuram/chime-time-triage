# SSI Chime Time TRI Tracker

Auto-updating Confluence pages that track TRI (Triage) Jira tickets for the SSI org (Can Envarli's engineering + Phil McDonnell's product + Caylee Betts's design orgs).

---

## Confluence Pages

| Page | ID | URL |
|---|---|---|
| Main (exec summary + aggregate stats) | 4337467666 | https://chime.atlassian.net/wiki/spaces/ER/pages/4337467666 |
| Individual Stats (per-person breakdown) | 4339466464 | https://chime.atlassian.net/wiki/spaces/ER/pages/4339466464 |
| Tickets by Squad & Feature | 4339564795 | https://chime.atlassian.net/wiki/spaces/ER/pages/4339564795 |
| Opportunities by Hero Metric | 4337042152 | https://chime.atlassian.net/wiki/spaces/ER/pages/4337042152 |

Atlassian cloudId: `a40b7113-0103-4597-b693-76b66e0a4614`
Confluence space: ER (Engine Room), at chime.atlassian.net

---

## Daily Routines

Three Claude Code Remote (CCR) routines run automatically in Anthropic's cloud — no local machine needed.

| Time (PT) | Routine ID | Name | Pages updated |
|---|---|---|---|
| 7:00 AM | `trig_018gRjMJBJ3UwFwMSMPs9DaN` | TRI Ticket Tracker — Daily Update | Main + Individual Stats |
| 7:30 AM | `trig_013W52ADj2qZmRKwNcLMcZiE` | TRI Squad & Feature Page — Daily Update | Tickets by Squad & Feature |
| 8:00 AM | `trig_01Ld1NwdLPj6sxLyiMyQstas` | TRI Opportunities by Hero Metric — Daily Update | Opportunities by Hero Metric |

View and manage routines: https://claude.ai/code/routines

---

## Recreating from Scratch

If routines are lost or you need to set this up in a new Claude account, follow these steps.

### Prerequisites

1. A **Claude Code** account at https://claude.ai/code
2. The following **MCP connectors** connected at https://claude.ai/customize/connectors:
   - **Glean** (`chime-prod-be.glean.com`) — for org member lookup and hero metric discovery
   - **Atlassian** (`mcp.atlassian.com`) — for Jira queries and Confluence updates

### Step 1 — Create the Confluence pages

If the pages don't exist, create them via the Atlassian MCP or manually in Confluence:

- Parent page: `4337467666` (main page — create this first in the ER space)
- Child pages with parent `4337467666`:
  - "SSI Chime Time Tracker — Individual Stats"
  - "SSI TRI: Tickets by Squad & Feature"
  - "SSI TRI: Opportunities by Hero Metric"

Note the page IDs once created — you'll need them in the routine prompts.

### Step 2 — Create the routines

Open Claude Code (claude.ai/code or the CLI) and use `/schedule` to create each routine. Paste the prompt from the corresponding file in this repo.

| Routine file | Schedule | MCP connectors needed |
|---|---|---|
| `routine_main.md` | `0 15 * * *` (7AM PT) | Glean, Atlassian |
| `routine_squad_feature.md` | `30 15 * * *` (7:30AM PT) | Glean, Atlassian |
| `routine_opportunities.md` | `0 16 * * *` (8AM PT) | Glean, Atlassian |

Use the **Default** environment (`env_01RW7iHkBpiK5SQckDaSz8R8`) for all routines.

### Step 3 — Trigger an immediate run

After creating each routine, trigger a manual run from https://claude.ai/code/routines to verify it works before waiting for the scheduled time.

---

## Updating the Routines

### Add a new org member

1. Find their email and squad
2. Edit the relevant routine file in this repo (add to the SQUAD MAPPING section)
3. Open Claude Code and ask it to update the routine:
   > "Update routine trig_018gRjMJBJ3UwFwMSMPs9DaN — add [name] ([email]) to the squad mapping under [squad]"

### Change the schedule

Ask Claude Code:
> "Update routine [routine ID] to run at [new time] PT instead"

### Trigger a manual run

Ask Claude Code:
> "Run routine [routine ID] now"

Or click "Run now" at https://claude.ai/code/routines

---

## Architecture

```
claude.ai CCR (cloud)
    ├── 7:00 AM: Main routine
    │       ├── Glean → get org members
    │       ├── Jira → count total Chime TRI open/closed
    │       ├── Jira → fetch all SSI open tickets (paginated)
    │       ├── Jira → fetch all SSI closed tickets (paginated)
    │       ├── Compute analytics + JQL links
    │       ├── Confluence → update Main page (4337467666)
    │       └── Confluence → update Individual Stats page (4339466464)
    │
    ├── 7:30 AM: Squad & Feature routine
    │       ├── Glean → get org members
    │       ├── Jira → fetch all SSI open tickets + components
    │       ├── Jira → fetch all SSI closed tickets + components
    │       └── Confluence → update Squad & Feature page (4339564795)
    │
    └── 8:00 AM: Opportunities routine
            ├── Glean → discover SSI hero metrics
            ├── Glean → get org members
            ├── Jira → fetch all SSI open tickets + summaries
            ├── Claude reasoning → map tickets to hero metrics
            └── Confluence → update Opportunities page (4337042152)
```

---

## Files in This Repo

| File | Description |
|---|---|
| `routine_main.md` | Prompt for the main daily routine (Main + Individual Stats pages) |
| `routine_squad_feature.md` | Prompt for the Squad & Feature page routine |
| `routine_opportunities.md` | Prompt for the Hero Metric Opportunities page routine |
