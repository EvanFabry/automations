---
name: outreach-setup
description: Use to set up a teammate for HSC sponsor outreach, or when someone runs /outreach-setup — connects Notion through Composio (the shared HSC workspace as `hsc`, plus any workspace of their own), checks their other connectors, confirms they can see the Sponsors CRM, records their LinkedIn connections as warm routes, points them at map sharing, schedules their outreach run hourly (with their own CRM, its purpose, and task tracker if they keep them) and runs it once. Also use for "set me up for outreach", "install the outreach automation", "connect my Notion workspaces", or Evan's central setup of the watchdog and crawl.
---

# Outreach setup

Get one teammate from "plugin installed" to "my sponsor rows stay current
every hour, with one digest each morning", in one sitting of about fifteen minutes, and leave a record that it
worked. The full install guide is at
https://prospecting.hscompute.com/settings/install; send them there for any
step they would rather follow on their own.

## 0. How to read this file

**Sections are numbered in precedence order; the lower number wins.** Policy
— owners, stage meanings, do-not-contact — lives on the Rules page (§2), not
here.

## 1. Safety — non-negotiable

- **Never send.** Setup sends no email, text or message to anyone.
- **Credentials stay with their owner.** Never ask for, read back or store a
  password, token or app-specific password. Every connector and every
  Composio account is connected by the teammate, in their own settings.
- **Never replace a connection.** Never remove a Composio account (the
  `remove` action) or reconnect one that is active (`reinitiate_all`, where a
  version still offers it, replaces existing connections). A wrong or missing
  workspace is added as a new account, or relabelled with `rename`, which
  changes only the name.
- **The HSC workspace is the `hsc` account.** Read and write the Rules page,
  Sponsors CRM and Run log only through the Composio Notion account `hsc`
  (§2), never by search and never in another workspace.
- **Their data only.** Edit only the teammate's own row on the Rules page's
  Owners table, their own scheduled task, and their own map's contacts. Never
  change another person's CRM rows, task or share. Their own CRM and task
  tracker go in their scheduled task, never on the Rules page, and setup only
  reads them.
- **A LinkedIn export never leaves this conversation whole.** Record only each
  connection's name, company, profile URL and connected date; drop every email
  address and anything else the file carries.

## 2. Locations

| What | Where | ID for Composio's Notion tools |
| --- | --- | --- |
| Rules page (Owners table) | https://app.notion.com/p/3f81ec23f3cf81a6b3d6fe73edd7cdcb | page `3f81ec23f3cf81a6b3d6fe73edd7cdcb` |
| Sponsors CRM | collection://6451ec23-f3cf-83f1-a4f7-87bab3ce5292 | data source `6451ec23-f3cf-83f1-a4f7-87bab3ce5292`, database `e4d1ec23f3cf8227bddd01abe67cc8d9` |
| Run log | collection://9c977f46-c7b2-4126-9357-781d56a057f4 | data source `9c977f46-c7b2-4126-9357-781d56a057f4`, database `64efc12133db449187e39aae74145619` |
| Warm-path map and sharing | https://prospecting.hscompute.com (the `prospecting` connector) | |
| Composio connector | https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Composio&connectorUrl=https%3A%2F%2Fconnect.composio.dev%2Fmcp | |
| Install guide | https://prospecting.hscompute.com/settings/install | |

**Notion goes through the Composio connector, one connected account per
workspace.** The HSC workspace, which holds the Rules page, CRM and Run log,
is the account named `hsc`. Select it on every call: run Notion tools through
`COMPOSIO_MULTI_EXECUTE_TOOL` with `account: "hsc"` on each item in `tools`,
never as a Notion tool called on its own, which may not carry the account.
`NOTION_QUERY_DATA_SOURCE` reads rows by data source ID, `NOTION_WHO_AM_I`
names the workspace an account reaches, `NOTION_LIST_USERS` lists its people,
and `NOTION_FETCH_DATABASE` shows a database's columns. Check arguments with
`COMPOSIO_GET_TOOL_SCHEMAS`; if a slug is gone, find its replacement with
`COMPOSIO_SEARCH_TOOLS`.

## 3. Modes

- **Teammate** (default): §4, for the person running it.
- **Central**, only when Evan asks for it: §5.

## 4. Teammate setup

Do the steps in order and keep a checklist in the conversation. A step that
needs the teammate's hands — connecting something, sharing a page — is theirs;
say exactly where to click, then wait and re-check rather than moving on.

### 4.1 Connectors

| Connector | Needed for |
| --- | --- |
| Composio, with the HSC Notion workspace as `hsc` | the CRM, Rules page and Run log — required |
| Gmail | status from email, and drafts — required |
| prospecting (hscompute.com) | routes and recorded contacts — required |
| Granola | meeting notes — recommended |
| Beeper (desktop app, on their computer) | texts and WhatsApp — recommended |

Notion goes through Composio for everyone, so one run can reach the HSC
workspace and the teammate's own workspaces side by side:

1. **Add Composio.** Send them to the Composio connector link (§2). It opens
   Claude's custom connector dialog with the name and the URL
   (`https://connect.composio.dev/mcp`) filled in; they click Add, then sign
   in to Composio when asked.
2. **Connect the HSC workspace as `hsc`**, first. If `list` already shows a
   Notion account that reaches the CRM, rename it to `hsc` (`rename`, its
   `account_id`). Otherwise `add` a Notion connection with alias `hsc`. On
   Notion's consent screen they choose the Haight Street Compute workspace
   and give it the pages holding the Sponsors CRM, the Run log and the Rules
   page.
3. **Connect their own workspaces**, one account each, under any name they
   will recognise (`personal`, `acme`) — only the ones the daily run should
   keep current (§4.4). Each `add` is a new account beside the others; if
   Composio refuses for the account limit, they choose one they no longer
   use and remove it themselves in Composio's dashboard (setup never does).
4. **Remove the built-in Notion connector** in Claude (Customize → Connectors
   → Notion → Disconnect), so there is one Notion path and nothing reaches
   the CRM but `hsc`.
5. **Check** that `NOTION_WHO_AM_I` through `hsc` names the HSC workspace,
   then go on to §4.2.

Then check each of the other connectors and name any that is missing.

### 4.2 The CRM

Query the Sponsors CRM through `hsc`. If it fails:

- `hsc` reaches another workspace (§4.1 step 5): rename that account to a
  name of their choosing (`rename`), then connect the HSC workspace as `hsc`.
- `hsc` is the HSC workspace but the CRM is not found: Composio was not given
  the page. In Notion they open the CRM, then ••• → Connections → add
  Composio, and the same for the Rules page.
- Neither: they are viewing the CRM through a public link, which connectors
  cannot use. Ask them to check they are a member or invited guest.

Then find their Notion user id (from rows where they are Account Owner, or
with `NOTION_LIST_USERS` through `hsc`) and make sure the Owners table lists
them with every Notion user they have. Add their row if it is missing.

### 4.3 Their network

1. Call `whoami` on the prospecting connector and confirm it names them.
2. Ask whether they will share their warm-path map with the event team. If
   yes, send them to https://prospecting.hscompute.com/settings/sharing to
   grant each teammate by name. Sharing is read-only, and teammates see their
   contacts only where they match a sponsor.
3. Ask them to link their address book at
   https://prospecting.hscompute.com/settings/contacts (Google, or iCloud for
   iPhone contacts with an app-specific password they create themselves). It
   keeps itself in sync from then on. Only contacts that can be a route are
   kept — a company, a LinkedIn profile, a work email at a company the team
   tracks, or a name that is one of their own LinkedIn connections — never
   numbers or addresses.
4. Ask for LinkedIn's data export (Settings → Data privacy → Get a copy of your
   data → Connections). When they attach `Connections.csv`, open a run
   (`record_run`, kind `contact-sync`) and send the rows to `record_contacts`
   in batches of up to 500: source `linkedin`, channel `linkedin_connection`,
   name from First and Last Name, company, the profile URL, and `last_at` from
   Connected On. Drop the email column (§1). Finish with `run_summary` and
   tell them how many were recorded.

### 4.4 Their own CRM and task tracker (optional)

Ask whether they keep their own CRM or task tracker in Notion that the daily
run should keep current. For each one they name:

1. Take the database's URL and the Composio account its workspace is
   connected as (§4.1 step 3). For a CRM, also ask for its purpose in one
   line ("investors I might raise from", "people to hire"): the run adds a
   new lead to it only when the lead matches that line.
2. Open it through that account with `NOTION_FETCH_DATABASE`, and tell them
   which existing columns the run will write: for a CRM, the ones meaning
   Last Contact, Next step, Next step date, Thread link, Suggested status and
   Last synced; for a tracker, its title, a date, a link and, on new tasks
   only, its status. The run adds no columns; one they want is theirs to add.
3. Tell them what stays personal: its rows never reach the Sponsors CRM, the
   prospecting server, the Run log or Slack, and the digest only counts them.

Both go in their scheduled task (§4.5), never on the Rules page.

### 4.5 Their hourly run

Create one scheduled task with this session's scheduling tool, or, when they
already have a `Daily outreach run · <first name>` task, change that one
rather than adding a second:

- Name: `Daily outreach run · <first name>`
- Schedule: hourly, cron `0 * * * *` — the top of every hour. The scheduler
  spreads start times across the hour, so do not pick a minute yourself. Most
  runs find nothing and stop after a few calls; the digest goes out once a
  day, from the first run after 8am.
- Prompt: `Run the daily-outreach-run skill for <name> (Notion users: <their
  user ids>). Personal CRM: <database URL> (Composio account <name>).
  Personal CRM purpose: <one line>. Task
  tracker: <database URL> (Composio account <name>). They are not watching;
  follow the skill exactly.` Leave
  out the Personal CRM sentences or the Task tracker sentence when they have
  none.
- Notifications on, so the digest and the "a sponsor is waiting on you"
  messages reach their phone.

If Beeper runs on their computer, the task should require that computer so
chats are read; say so if the tool cannot set it, and tell them where to turn
on "Require this computer".

### 4.6 First run

Run the daily-outreach-run skill now for them, as a backfill. It writes their
first Run log row; check through `hsc` that the row exists. End with what got
set up, what is still theirs to do, and their first digest.

## 5. Central setup (Evan only)

Create, once, on Evan's account, and skip any that already exists. The
watchdog and lead scout read and write the HSC workspace, so Evan's account
needs Composio with `hsc` connected first (§4.1).

1. **Watchdog** — the outreach-watchdog skill every four hours.
2. **Crawl** — the warm-path-map skill on weekdays, requiring the computer
   whose Chrome holds Evan's LinkedIn session, so the crawl can open pages.
3. **Lead scout** — the lead-scout skill daily, each morning. When a weekly
   task already exists, change its schedule rather than adding another.

Then confirm the Run log's "Watchdog heartbeat" row updates after the first
watchdog run.
