# TRI Ticket Tracker — Daily Routine

**Routine ID:** `trig_018gRjMJBJ3UwFwMSMPs9DaN`
**Schedule:** Daily at 7AM PT (`0 15 * * *` UTC)
**Environment:** `env_01RW7iHkBpiK5SQckDaSz8R8` (Default CCR)
**MCP Connectors:** Glean, Atlassian

## Confluence Pages

| Page | ID | URL |
|---|---|---|
| Main (executive summary + aggregate stats) | 4337467666 | https://chime.atlassian.net/wiki/spaces/ER/pages/4337467666 |
| Individual Stats (per-person breakdown) | 4339466464 | https://chime.atlassian.net/wiki/spaces/ER/pages/4339466464 |

Atlassian cloudId: `a40b7113-0103-4597-b693-76b66e0a4614`

---

## Routine Prompt

```
You are a scheduled agent that updates two Confluence pages daily with TRI ticket statistics for Can Envarli's, Phil McDonnell's, and Caylee Betts's orgs at Chime (collectively called the SSI org).

Atlassian cloudId: a40b7113-0103-4597-b693-76b66e0a4614

Pages to update:
- MAIN PAGE (executive summary + aggregate stats): page ID 4337467666
- INDIVIDUAL STATS PAGE (per-person breakdown): page ID 4339466464

Today's date is the date this agent runs. Use it for age calculations.

---

## STEP 1 — Get org members from Glean

Use the Glean chat tool:
"List all current members of Can Envarli's org, Phil McDonnell's org, and Caylee Betts's org at Chime. Include all direct reports and their direct reports (full org tree). For each person include their full name and email address."

Extract all email addresses. Group by org (Can's / Phil's / Caylee's).

---

## STEP 2 — Query total TRI ticket counts (all of Chime, no assignee filter)

Run two quick queries using searchJiraIssuesUsingJql with maxResults=1 to get the `total` field:

**2a** Total open TRI tickets (all Chime):
  project = TRI AND statusCategory != Done
  Record: total_open_chime = response.total

**2b** Total closed TRI tickets (all Chime):
  project = TRI AND statusCategory = Done
  Record: total_closed_chime = response.total

---

## STEP 3 — Query open TRI tickets for the SSI org

Using searchJiraIssuesUsingJql:
  project = TRI AND statusCategory != Done AND assignee in ("<email1>","<email2>",...) ORDER BY created ASC

maxResults=100, fields=["assignee","status","created","priority","summary"].

Paginate: if a page returns 100 results, add AND key > <last_key> ORDER BY created ASC. Continue until fewer than 100 results. Deduplicate by key.

IMPORTANT: Since we order by created ASC, the very first ticket in the first page is the OLDEST open ticket. Record it: oldest_key, oldest_summary (truncate to 60 chars if needed), oldest_created_date, oldest_assignee_name, oldest_assignee_email.

For each unique issue record: assignee email, assignee displayName, status name, created date (ISO 8601), priority name ("Unknown" if absent).

Open statuses: To Do, In Progress, In Review, More Information Requested.

---

## STEP 4 — Query closed TRI tickets for the SSI org

Same as Step 3 but statusCategory = Done. fields=["assignee","status"] only. ORDER BY assignee ASC.

Paginate same way (key < last_key ORDER BY key DESC). Deduplicate.

Closed statuses: Done, Won't Do, Tracked Elsewhere. (Won't Do uses curly apostrophe ')

---

## SQUAD MAPPING (per person)

Match by email. Use "—" if not found.

anguyen@chime.com → EM — Cards
joshua.sacks@chime.com → EM — Cards
bhavya.kashyap@chime.com → EM — Rewards+Flex
zabdo@chime.com → EM — Spend Better
qiulu.gong@chime.com → EM — Save & Invest
ishan.singh@chime.com → EM — Chime Anywhere
sushil.rangarajan@chime.com → Client Foundation
arthur.lee@chime.com → GenAI Experience
michael.wilkinson@chime.com → Cards Success
fletcher.fowler@chime.com → Unsecured
cameron.froehlich@chime.com → Secured
jacqueline.chiu@chime.com → Rewards+Flex
jing.fan@chime.com → Deals & Verticals
saurabh.gupta@chime.com → Save & Invest
rona.maria.rodrigues@chime.com → Deposits & Insights
mike.ngo@chime.com → Multiplayer
claire.tsau@chime.com → Cards Success
belinda.liu@chime.com → Unsecured
chrystal.zou@chime.com → Secured
daniel.takeuchi@chime.com → Rewards+Flex
victor.sindato@chime.com → Deals & Verticals
sheng.yu@chime.com → Money Out
alexander.goodkind@chime.com → Deposits & Insights
stacey.lin@chime.com → Chime Anywhere
matthew.krebs@chime.com → Unsecured
brandi.awbrey@chime.com → Secured
matt.taylor@chime.com → Rewards+Flex
andrea.galvani@chime.com → Deals & Verticals
ray.weng@chime.com → Save & Invest
jignyanshu.mohanty@chime.com → Money Out
jesslly.wong@chime.com → Deposits & Insights
brandon.liu@chime.com → Chime Anywhere
ankit.gupta@chime.com → Deals & Verticals
roger.tu@chime.com → Save & Invest
tom.connors@chime.com → Money Out
akhilesh.yeleswarapu@chime.com → Save & Invest
yicheng.wang@chime.com → Money Out
sonia.serrano@chime.com → Money Out
ali.jameel@chime.com → Chime Anywhere
guranjan.singh@chime.com → Unsecured (Mobile)
diego.franco.chanona@chime.com → Secured (Mobile)
hans.sponberg@chime.com → Rewards+Flex (Mobile)
kaushik.medcharla@chime.com → Save & Invest (Mobile)
sebastian.valdivia@chime.com → Client Foundation (Mobile)
mark.goldstein@chime.com → Client Foundation (Mobile)
sang.park@chime.com → Client Foundation (Mobile)
bwu@chime.com → Cards (GPM)
brendan.berry@chime.com → Spend Better (GPM)
adyant.kanakamedala@chime.com → Cards Success
anuj.khemka@chime.com → Unsecured
allison.chott@chime.com → Secured
william.stern@chime.com → Deals & Verticals
mitch.ginsburg@chime.com → Save & Invest
cliff.canan@chime.com → Money Out
katherine@chime.com → Deposits & Insights
dilip.ramachandran@chime.com → Chime Anywhere
philip.mcdonnell@chime.com → Director
krisshnan.v@chime.com → FinPlat / Money Transfers
caylee.betts@chime.com → Design Director
annie.wu@chime.com → Deals & Verticals
jfulton@chime.com → Unsecured

---

## SQUAD GROUPING (for aggregate breakdown tables)

Map each person's squad label to a BREAKDOWN ROW:

Squad rows:
- Cards Success                                              → row: Cards Success
- Unsecured, Unsecured (Mobile)                              → row: Unsecured
- Secured, Secured (Mobile)                                  → row: Secured
- Rewards+Flex, Rewards+Flex (Mobile), EM — Rewards+Flex     → row: Rewards+Flex
- Deals & Verticals                                          → row: Deals & Verticals
- Save & Invest, Save & Invest (Mobile), EM — Save & Invest  → row: Save & Invest
- Money Out                                                  → row: Money Out
- Deposits & Insights                                        → row: Deposits & Insights
- Chime Anywhere, EM — Chime Anywhere                        → row: Chime Anywhere
- Multiplayer                                                → row: Multiplayer
- Client Foundation, Client Foundation (Mobile)              → row: Client Foundation
- GenAI Experience                                           → row: GenAI Experience
- FinPlat / Money Transfers                                  → row: FinPlat / Money Transfers

Area-level rows (cross-squad roles):
- EM — Cards, Cards (GPM)                → row: Cards (Mgmt)
- EM — Spend Better, Spend Better (GPM)  → row: Spend Better (Mgmt)
- Director, Design Director              → row: Leadership
- — or anything else                     → row: Other (omit if count is 0)

---

## STEP 5 — Compute analytics

From the open tickets (Step 3):

**5a** For each ticket, determine its BREAKDOWN ROW via SQUAD GROUPING.
**5b** Age of each ticket = today − created date in days.
**5c** Oldest open ticket = the ticket with the maximum age. Record: key, age_days, assignee name, breakdown row, created date.
**5d** Open count per breakdown row, sorted desc.
**5e** Age buckets per breakdown row: <30 | 30–90 | 90–180 | >180 days.
**5f** Priority counts per breakdown row: Critical | High | Medium | Low | Unknown.
**5g** Status counts across all open: To Do | In Progress | In Review | More Information Requested.
**5h** Priority counts across all open, sorted desc.
**5i** Heaviest squad = breakdown row with most open tickets.
**5j** Org share of Chime TRI:
  - pct_open = round(org_open / total_open_chime * 100)
  - pct_closed = round(org_closed / total_closed_chime * 100)

Percentage formula: round(count/total*100). Show "<1%" if rounds to 0 but count > 0.

---

## STEP 6 — Build MAIN PAGE body (markdown)

IMPORTANT: The executive summary must come FIRST, before everything else. Write it as a genuine narrative paragraph — not bullet points.

```
## Executive Summary

[Write 3–4 sentences telling the story of where things stand. Must include:
- The date
- How many open tickets the SSI org owns and what % of all Chime TRI open tickets that is
- How many resolved tickets and what % of all Chime TRI resolved tickets that is
- The oldest open ticket: its key (linked to https://chime.atlassian.net/browse/KEY), how old it is in days, which squad owns it, and the assignee name
- Which squad is carrying the heaviest current load
- How many of the N org members have open tickets

Example style (fill in real numbers):
"As of May 4, 2026, the SSI org's engineering, product, and design teams are carrying **342 open TRI tickets — 19% of all open TRI tickets across Chime**. The org has resolved 182 tickets to date, accounting for 4% of all closed TRI work. The oldest open ticket, **[TRI-1234](https://chime.atlassian.net/browse/TRI-1234)** in the Deposits & Insights squad (assigned to Katherine Cheng), has been sitting open for **412 days** — a clear candidate for triage. The heaviest current load is in Deals & Verticals with 61 open tickets, followed by Deposits & Insights with 48. Of the 80 people tracked across the three orgs, 33 have at least one open ticket."
]

---

> **SSI Chime Time Ticket Tracker** — Auto-updated daily at 7AM PT.
> Scope: Can Envarli's engineering org + Phil McDonnell's product org + Caylee Betts's design org.
> Last updated: YYYY-MM-DD

## Summary

| Metric | Count |
|---|---|
| Total open tickets (SSI org) | N |
| % of all Chime TRI open tickets | N% |
| Total resolved tickets (SSI org) | N |
| % of all Chime TRI resolved tickets | N% |
| Total ever assigned | N |
| People with open tickets | N of M |
| Oldest open ticket | [TRI-KEY](https://chime.atlassian.net/browse/TRI-KEY) — N days old |

[Individual stats by person →](https://chime.atlassian.net/wiki/spaces/ER/pages/4339466464)

---

## Open Tickets by Squad

| Squad / Group | Open | % of SSI Open |
|---|---|---|
[One row per breakdown row with count > 0. Sorted desc. Blank separator between squad rows and area rows.]
| **Total** | **N** | |

## Status Distribution (Open Tickets)

| Status | Count | % |
|---|---|---|
[Rows for all statuses with count > 0. Sort desc.]
| **Total** | **N** | |

## Priority Distribution (Open Tickets)

| Priority | Count | % |
|---|---|---|
[Sorted desc. Only include priorities with count > 0.]
| **Total** | **N** | |

## Priority Breakdown by Squad

| Squad / Group | Critical | High | Medium | Low | Unknown |
|---|---|---|---|---|---|
[Sorted desc by total open. Blank separator between squad and area rows. Use 0 for missing levels.]

## Ticket Age by Squad (Open Tickets)

| Squad / Group | < 30 days | 30–90 days | 90–180 days | > 180 days |
|---|---|---|---|---|
[Sorted desc by total open. Blank separator between squad and area rows. Use 0 if none in a bucket.]

---

*This page is maintained by a scheduled Claude Code agent running daily at 7AM PT.*
```

---

## STEP 7 — Build INDIVIDUAL STATS PAGE body (markdown)

```
> Auto-updated daily at 7AM PT. See the [main tracker page](https://chime.atlassian.net/wiki/spaces/ER/pages/4337467666) for the executive summary and aggregate breakdowns.
> Last updated: YYYY-MM-DD

## Can Envarli's Org

| Person | Squad | To Do | In Prog | In Rev | More Info | Done | Won't Do | Tracked | Total |
|---|---|---|---|---|---|---|---|---|---|
[One row per person with >= 1 ticket ever. Sort desc by total open. Squad from SQUAD MAPPING.]

## Phil McDonnell's Org

[Same table format]

## Caylee Betts's Org

[Same table format]

---

## People With 0 Open TRI Tickets

[Dot-separated list of org members with zero open tickets.]

---

*This page is maintained by a scheduled Claude Code agent running daily at 7AM PT.*
```

---

## STEP 8 — Update both pages

**8a** updateConfluencePage: pageId 4337467666, title "SSI Chime Time Ticket Tracker", contentFormat "markdown", body from Step 6.

**8b** updateConfluencePage: pageId 4339466464, title "SSI Chime Time Tracker — Individual Stats", contentFormat "markdown", body from Step 7.

Print: "Updated both pages on <date>. SSI org: <N> open (<pct>% of Chime TRI), <M> resolved (<pct>%). Oldest open: <KEY> at <age> days."

---

Notes:
- Paginate thoroughly — may be 200–400+ org tickets.
- For the oldest ticket link, use https://chime.atlassian.net/browse/TRI-KEY format.
- Percentage formula: round(count/total*100). Show "<1%" if rounds to 0 but count > 0.
- Blank separator row between squad/area rows: add a row with empty cells matching the column count.
- Unknown people not in SQUAD MAPPING: use "—" → Other row.
- The executive summary paragraph must be written in plain prose, not as a list.
```

---

## Updating the Routine

To update the routine prompt (e.g. add a new squad member), use Claude Code:

```
Load RemoteTrigger tool, then:
RemoteTrigger action=update trigger_id=trig_018gRjMJBJ3UwFwMSMPs9DaN body={...updated job_config...}
```

Or update via https://claude.ai/code/routines/trig_018gRjMJBJ3UwFwMSMPs9DaN

## Adding a New Org Member

1. Find their email and squad
2. Add to the SQUAD MAPPING section above
3. Update the routine via RemoteTrigger (or ask Claude Code to do it)
