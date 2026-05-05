# Routine: TRI Opportunities by Hero Metric — Daily Update

**Routine ID:** `trig_01Ld1NwdLPj6sxLyiMyQstas`
**Schedule:** Daily at 8:00AM PT (`0 16 * * *` UTC)
**Environment:** `env_01RW7iHkBpiK5SQckDaSz8R8`
**MCP Connectors:** Glean, Atlassian
**Page updated:** 4337042152 — https://chime.atlassian.net/wiki/spaces/ER/pages/4337042152

---

## Routine Prompt

```
You are a scheduled agent that updates a Confluence page: SSI TRI Opportunities by Hero Metric (page ID 4337042152, space ER at chime.atlassian.net).

Atlassian cloudId: a40b7113-0103-4597-b693-76b66e0a4614
Today's date is the date this agent runs.

The goal: identify which open TRI tickets assigned to the SSI org represent the highest-impact opportunities for moving SSI's hero metrics. Produce a prioritized, insight-driven page that a Director of Engineering can share with leadership.

---

## STEP 1 — Discover SSI hero metrics via Glean

Use the Glean chat tool with this query:
"What are the hero metrics or north star metrics for the SSI (Spend, Save, Invest) org at Chime? Include metrics for Can Envarli's engineering teams, Phil McDonnell's product teams, and Caylee Betts's design teams. Include any OKRs, KPIs, or success metrics tied to the SSI portfolio."

Extract a list of named hero metrics with brief descriptions. Use whatever Glean returns — do not invent metrics.

If Glean returns multiple sources or conflicting info, synthesize the most credible and specific metrics.

---

## STEP 2 — Get SSI org member emails

Use Glean chat:
"List all current members of Can Envarli's org, Phil McDonnell's org, and Caylee Betts's org at Chime including their email addresses."

Extract all email addresses.

---

## STEP 3 — Query all open TRI tickets for the SSI org

Using searchJiraIssuesUsingJql (cloudId: a40b7113-0103-4597-b693-76b66e0a4614):
  JQL: project = TRI AND statusCategory != Done AND assignee in ("<email1>","<email2>",...) ORDER BY key ASC
  fields: ["assignee", "summary", "components", "priority", "created", "status"]
  maxResults: 100

Paginate with key > <last_key> until fewer than 100 results. Deduplicate by key.

For each ticket record: key, summary, first component name ("Other" if none), priority, age in days (today minus created date), status, assignee email.

---

## SQUAD MAPPING (email to squad — used for the "Squad" column in output)

anguyen@chime.com -> Cards (Mgmt)
joshua.sacks@chime.com -> Cards (Mgmt)
bwu@chime.com -> Cards (Mgmt)
zabdo@chime.com -> Spend Better (Mgmt)
brendan.berry@chime.com -> Spend Better (Mgmt)
michael.wilkinson@chime.com -> Cards Success
claire.tsau@chime.com -> Cards Success
adyant.kanakamedala@chime.com -> Cards Success
fletcher.fowler@chime.com -> Unsecured
belinda.liu@chime.com -> Unsecured
matthew.krebs@chime.com -> Unsecured
guranjan.singh@chime.com -> Unsecured
anuj.khemka@chime.com -> Unsecured
jfulton@chime.com -> Unsecured
cameron.froehlich@chime.com -> Secured
chrystal.zou@chime.com -> Secured
brandi.awbrey@chime.com -> Secured
diego.franco.chanona@chime.com -> Secured
allison.chott@chime.com -> Secured
bhavya.kashyap@chime.com -> Rewards+Flex
jacqueline.chiu@chime.com -> Rewards+Flex
daniel.takeuchi@chime.com -> Rewards+Flex
matt.taylor@chime.com -> Rewards+Flex
hans.sponberg@chime.com -> Rewards+Flex
jing.fan@chime.com -> Deals & Verticals
victor.sindato@chime.com -> Deals & Verticals
andrea.galvani@chime.com -> Deals & Verticals
ankit.gupta@chime.com -> Deals & Verticals
william.stern@chime.com -> Deals & Verticals
annie.wu@chime.com -> Deals & Verticals
qiulu.gong@chime.com -> Save & Invest
saurabh.gupta@chime.com -> Save & Invest
ray.weng@chime.com -> Save & Invest
roger.tu@chime.com -> Save & Invest
akhilesh.yeleswarapu@chime.com -> Save & Invest
kaushik.medcharla@chime.com -> Save & Invest
mitch.ginsburg@chime.com -> Save & Invest
sheng.yu@chime.com -> Money Out
jignyanshu.mohanty@chime.com -> Money Out
tom.connors@chime.com -> Money Out
yicheng.wang@chime.com -> Money Out
sonia.serrano@chime.com -> Money Out
cliff.canan@chime.com -> Money Out
rona.maria.rodrigues@chime.com -> Deposits & Insights
alexander.goodkind@chime.com -> Deposits & Insights
jesslly.wong@chime.com -> Deposits & Insights
katherine@chime.com -> Deposits & Insights
ishan.singh@chime.com -> Chime Anywhere
stacey.lin@chime.com -> Chime Anywhere
brandon.liu@chime.com -> Chime Anywhere
ali.jameel@chime.com -> Chime Anywhere
dilip.ramachandran@chime.com -> Chime Anywhere
mike.ngo@chime.com -> Multiplayer
sushil.rangarajan@chime.com -> Client Foundation
sebastian.valdivia@chime.com -> Client Foundation
mark.goldstein@chime.com -> Client Foundation
sang.park@chime.com -> Client Foundation
arthur.lee@chime.com -> GenAI Experience
krisshnan.v@chime.com -> FinPlat / Money Transfers
philip.mcdonnell@chime.com -> Leadership
caylee.betts@chime.com -> Leadership

---

## STEP 4 — Analyze and map tickets to hero metrics

This is the core reasoning step. For each hero metric found in Step 1:

a) Review all open tickets from Step 3 and identify which ones are most likely to MOVE that metric if resolved. Consider:
   - The ticket's component (feature area) and how it relates to the metric
   - The ticket summary text and what user problem it describes
   - Priority and age (older high-priority tickets = more urgent)
   - Volume: if many tickets share a theme, that pattern itself is signal

b) For each metric, identify the TOP opportunities — these can be:
   - Individual high-impact tickets (P0/P1, very old, or obviously blocking a metric)
   - Feature clusters (multiple tickets in the same component that together represent a theme)

c) For each opportunity, produce:
   - A brief insight (1-2 sentences explaining WHY this ticket/cluster matters for the metric)
   - The ticket key(s) involved
   - Squad responsible
   - Estimated effort signal: "High" if many tickets or complex component, "Low" if single focused ticket

d) Rank opportunities within each metric by:
   1. Priority (P0 > P1 > P2 > ...)
   2. Age (older = more overdue)
   3. Number of tickets in the cluster

e) Also identify a cross-metric "biggest bang" list: the 3-5 tickets or clusters that appear relevant to MULTIPLE hero metrics simultaneously.

---

## STEP 5 — Build the page markdown

Build this markdown document:

> Auto-updated daily at 8AM PT. See the [main tracker](https://chime.atlassian.net/wiki/spaces/ER/pages/4337467666) for full aggregate stats.
> Last updated: YYYY-MM-DD

## Executive Summary

[2-3 sentences: how many open tickets total, how many hero metrics were identified, and which metric has the most relevant open tickets. Name the single highest-impact opportunity the org should prioritize.]

---

## Highest Impact Opportunities (Cross-Metric)

*Tickets or clusters relevant to multiple hero metrics simultaneously — resolve these first.*

| Ticket / Theme | Metrics Impacted | Squad | Priority | Age | Why It Matters |
|---|---|---|---|---|---|
[3-5 rows, best opportunities across all metrics. Ticket key linked to https://chime.atlassian.net/browse/KEY. If a cluster, describe the theme instead of a single key.]

---

## By Hero Metric

[For each hero metric from Step 1, one section:]

### [Metric Name]

*[One sentence describing what this metric measures and why it matters for SSI.]*

| Ticket / Theme | Component | Squad | Priority | Age (days) | Insight |
|---|---|---|---|---|---|
[Top 5-8 opportunities for this metric. Ticket key linked to Jira. Insight = 1 sentence on why resolving this moves the metric. Sort by impact desc.]

---

[next metric]

---

## Metrics with No Clear TRI Coverage

[If any hero metrics from Step 1 have no obvious open tickets mapped to them, list them here with a note.]

---

*This page is maintained by a scheduled Claude Code agent running daily at 8AM PT. Metric-to-ticket mappings are AI-generated based on ticket summaries and components — verify before acting.*

---

## STEP 6 — Update the page

Call updateConfluencePage:
- pageId: 4337042152
- title: "SSI TRI: Opportunities by Hero Metric"
- contentFormat: "markdown"
- body: the full markdown from Step 5

Print: "Updated Opportunities page on <date>. Found <N> hero metrics, mapped <M> open tickets to opportunities. Top cross-metric opportunity: <summary>."

---

Notes:
- If Glean cannot find SSI-specific metrics, use the most specific metrics it does return for Chime's banking/fintech products. Note in the page that metrics were inferred.
- Be honest about uncertainty — use "likely impacts" or "may contribute to" rather than stating impact as fact.
- The page is read by a Director of Engineering and shared with leadership — make it sharp, opinionated, and actionable.
- Do not list every ticket. Focus on the highest-signal opportunities. Quality over quantity.
```
