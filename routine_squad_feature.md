# Routine: TRI Squad & Feature Page — Daily Update

**Routine ID:** `trig_013W52ADj2qZmRKwNcLMcZiE`
**Schedule:** Daily at 7:30AM PT (`30 15 * * *` UTC)
**Environment:** `env_01RW7iHkBpiK5SQckDaSz8R8`
**MCP Connectors:** Glean, Atlassian
**Page updated:** 4339564795 — https://chime.atlassian.net/wiki/spaces/ER/pages/4339564795

---

## Routine Prompt

```
You are a scheduled agent that updates ONE Confluence page: SSI TRI Tickets by Squad & Feature (page ID 4339564795, space ER at chime.atlassian.net).

Atlassian cloudId: a40b7113-0103-4597-b693-76b66e0a4614
Today's date is the date this agent runs.

---

## STEP 1 — Get org members from Glean

Use the Glean chat tool:
"List all current members of Can Envarli's org, Phil McDonnell's org, and Caylee Betts's org at Chime. Include all direct reports and their direct reports (full org tree). For each person include their full name and email address."

Extract all email addresses.

---

## STEP 2 — Query open TRI tickets

JQL: project = TRI AND statusCategory != Done AND assignee in ("<email1>","<email2>",...) ORDER BY key ASC
fields: ["assignee","status","summary","components"]
maxResults: 100. Paginate with key > <last_key>. Deduplicate by key.

---

## STEP 3 — Query closed TRI tickets

Same as Step 2 but: project = TRI AND statusCategory = Done AND assignee in (...) ORDER BY key ASC
fields: ["assignee","status","summary","components"]
Same pagination. Deduplicate.

---

## SQUAD MAPPING (email to squad label)

anguyen@chime.com -> EM — Cards
joshua.sacks@chime.com -> EM — Cards
bhavya.kashyap@chime.com -> EM — Rewards+Flex
zabdo@chime.com -> EM — Spend Better
qiulu.gong@chime.com -> EM — Save & Invest
ishan.singh@chime.com -> EM — Chime Anywhere
sushil.rangarajan@chime.com -> Client Foundation
arthur.lee@chime.com -> GenAI Experience
michael.wilkinson@chime.com -> Cards Success
fletcher.fowler@chime.com -> Unsecured
cameron.froehlich@chime.com -> Secured
jacqueline.chiu@chime.com -> Rewards+Flex
jing.fan@chime.com -> Deals & Verticals
saurabh.gupta@chime.com -> Save & Invest
rona.maria.rodrigues@chime.com -> Deposits & Insights
mike.ngo@chime.com -> Multiplayer
claire.tsau@chime.com -> Cards Success
belinda.liu@chime.com -> Unsecured
chrystal.zou@chime.com -> Secured
daniel.takeuchi@chime.com -> Rewards+Flex
victor.sindato@chime.com -> Deals & Verticals
sheng.yu@chime.com -> Money Out
alexander.goodkind@chime.com -> Deposits & Insights
stacey.lin@chime.com -> Chime Anywhere
matthew.krebs@chime.com -> Unsecured
brandi.awbrey@chime.com -> Secured
matt.taylor@chime.com -> Rewards+Flex
andrea.galvani@chime.com -> Deals & Verticals
ray.weng@chime.com -> Save & Invest
jignyanshu.mohanty@chime.com -> Money Out
jesslly.wong@chime.com -> Deposits & Insights
brandon.liu@chime.com -> Chime Anywhere
ankit.gupta@chime.com -> Deals & Verticals
roger.tu@chime.com -> Save & Invest
tom.connors@chime.com -> Money Out
akhilesh.yeleswarapu@chime.com -> Save & Invest
yicheng.wang@chime.com -> Money Out
sonia.serrano@chime.com -> Money Out
ali.jameel@chime.com -> Chime Anywhere
guranjan.singh@chime.com -> Unsecured (Mobile)
diego.franco.chanona@chime.com -> Secured (Mobile)
hans.sponberg@chime.com -> Rewards+Flex (Mobile)
kaushik.medcharla@chime.com -> Save & Invest (Mobile)
sebastian.valdivia@chime.com -> Client Foundation (Mobile)
mark.goldstein@chime.com -> Client Foundation (Mobile)
sang.park@chime.com -> Client Foundation (Mobile)
bwu@chime.com -> Cards (GPM)
brendan.berry@chime.com -> Spend Better (GPM)
adyant.kanakamedala@chime.com -> Cards Success
anuj.khemka@chime.com -> Unsecured
allison.chott@chime.com -> Secured
william.stern@chime.com -> Deals & Verticals
mitch.ginsburg@chime.com -> Save & Invest
cliff.canan@chime.com -> Money Out
katherine@chime.com -> Deposits & Insights
dilip.ramachandran@chime.com -> Chime Anywhere
philip.mcdonnell@chime.com -> Director
krisshnan.v@chime.com -> FinPlat / Money Transfers
caylee.betts@chime.com -> Design Director
annie.wu@chime.com -> Deals & Verticals
jfulton@chime.com -> Unsecured
Unmapped -> use squad label "Other"

---

## SQUAD GROUPING (squad label to breakdown row)

Cards Success -> Cards Success
Unsecured, Unsecured (Mobile) -> Unsecured
Secured, Secured (Mobile) -> Secured
Rewards+Flex, Rewards+Flex (Mobile), EM — Rewards+Flex -> Rewards+Flex
Deals & Verticals -> Deals & Verticals
Save & Invest, Save & Invest (Mobile), EM — Save & Invest -> Save & Invest
Money Out -> Money Out
Deposits & Insights -> Deposits & Insights
Chime Anywhere, EM — Chime Anywhere -> Chime Anywhere
Multiplayer -> Multiplayer
Client Foundation, Client Foundation (Mobile) -> Client Foundation
GenAI Experience -> GenAI Experience
FinPlat / Money Transfers -> FinPlat / Money Transfers
EM — Cards, Cards (GPM) -> Cards (Mgmt)
EM — Spend Better, Spend Better (GPM) -> Spend Better (Mgmt)
Director, Design Director -> Leadership
Other or unmapped -> Other

---

## STEP 4 — Build page markdown

For each ticket: breakdown_row = SQUAD GROUPING[SQUAD MAPPING[assignee_email]]. Feature = first component name, "Other" if none.

Group all tickets into: breakdown_row -> feature -> {open_count, closed_count}

For JQL links, URL-encode using Python urllib.parse.quote(jql, safe='').

Build this markdown document:

> Auto-updated daily at 7:30AM PT. See the [main tracker page](https://chime.atlassian.net/wiki/spaces/ER/pages/4337467666) for the executive summary and aggregate stats.
> Last updated: YYYY-MM-DD

---

[For each breakdown row that has at least 1 ticket, in this order: Cards Success, Unsecured, Secured, Rewards+Flex, Deals & Verticals, Save & Invest, Money Out, Deposits & Insights, Chime Anywhere, Multiplayer, Client Foundation, GenAI Experience, FinPlat / Money Transfers, Cards (Mgmt), Spend Better (Mgmt), Leadership, Other]

## [Breakdown Row] (N open, M closed)

| Feature | Open | Closed | Total |
|---|---|---|---|
[One row per feature in this squad, sorted by Open count desc.
Closed-only features go below a blank separator row.
Each feature name is hyperlinked to a JQL filtered to that component + squad's assignees:
  JQL: project = TRI AND component = "<Feature Name>" AND assignee in ("<email1>","<email2>",...)
  For "Other" feature (no component): project = TRI AND component is EMPTY AND assignee in (...)
  URL-encode and format: [Feature Name](https://chime.atlassian.net/issues/?jql=<encoded>)]
| | | | |
| **Total** | **N** | **M** | **N+M** |

---

[next squad section]

Notes:
- Add --- between squad sections.
- Omit the Other row if 0 tickets total.
- Each feature name must be a hyperlink to Jira.

---

## STEP 5 — Update the page

Call updateConfluencePage:
- pageId: 4339564795
- title: "SSI TRI: Tickets by Squad & Feature"
- contentFormat: "markdown"
- body: full markdown from Step 4

Print: "Updated Squad & Feature page on <date>. N open tickets, M closed tickets across X squads and Y features."
```

---

## Adding a New Org Member

1. Find their email and squad label
2. Add to SQUAD MAPPING in the prompt above
3. Ask Claude Code: "Update routine trig_013W52ADj2qZmRKwNcLMcZiE — add [name] ([email]) to squad mapping under [squad]"
