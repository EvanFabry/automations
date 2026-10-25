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
  (§2), never by search and never in another workspace. The one exception is
  the Owners row, which §4.2 writes through Composio's workbench proxy once
  its check shows the proxy reaches the HSC workspace.
- **Their data only.** Edit only the teammate's own row on the Rules page's
  Owners table (a "Not yet named" row waiting for them becomes theirs),
  Account Owner on CRM rows nobody owns that they claim (§4.2), their own
  scheduled task, and their own map's contacts and sharing. Never change
  another person's CRM rows, task or share. Their own CRM and task
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
never as a Notion tool called on its own, which may not carry the account
(the Owners row in §4.2 is the one write that cannot, and checks instead).
`NOTION_QUERY_DATA_SOURCE` reads rows by data source ID, `NOTION_WHO_AM_I`
names the workspace an account reaches, `NOTION_LIST_USERS` lists its people,
`NOTION_FETCH_DATABASE` shows a database's columns, and
`NOTION_UPDATE_ROW_DATABASE` changes a row by its page ID. Every property
value is a string, a number too: `"0"`, never `0`; people are comma-separated
user ids. Check arguments with `COMPOSIO_GET_TOOL_SCHEMAS`; if a slug is gone,
find its replacement with `COMPOSIO_SEARCH_TOOLS`.

## 3. Modes

- **Teammate** (default): §4, for the person running it.
- **Central**, only when Evan asks for it: §5.

## 4. Teammate setup

Do the steps in order and keep a checklist in the conversation. A step that
needs the teammate's hands — connecting something, signing in, sharing a page,
typing a password — is theirs; say exactly where to click, then wait and
re-check rather than moving on.

**Ask once.** As soon as §4.2 can read the CRM, ask every question below in
one round (one AskUserQuestion when this session has it), leaving out any
the conversation has already answered: which sponsors are theirs (§4.2),
whether to share their map (§4.3), their own CRM or task tracker and its
purpose (§4.4), and whether they text sponsors, so Beeper is read (§4.5).
After that, stop only for their hands.

### 4.1 Connectors

| Connector | Needed for |
| --- | --- |
| Composio, with the HSC Notion workspace as `hsc` | the CRM, Rules page and Run log — required |
| Gmail, with Google's "Manage drafts and send emails" permission | status from email, and drafts — required |
| prospecting (hscompute.com) | routes and recorded contacts — required |
| Granola | meeting notes — recommended |
| Beeper (desktop app, on their computer) | texts and WhatsApp — only if they text sponsors (§4) |

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
   keep current (§4.4). Until they have answered that, come back to this step
   after the questions (§4). Each `add` is a new account beside the others; if
   Composio refuses for the account limit, they choose one they no longer
   use and remove it themselves in Composio's dashboard (setup never does).
4. **Remove the built-in Notion connector** in Claude (Customize → Connectors
   → Notion → Disconnect), so there is one Notion path and nothing reaches
   the CRM but `hsc`. Leave it connected when another of their scheduled
   tasks uses Notion (read their task list): these skills reach Notion only
   through Composio either way. A task whose prompt pastes an old copy of a
   skill instead of naming it reads the CRM through the built-in connector:
   that task is a duplicate to turn off first (§4.5), not a reason to keep
   the connector.
5. **Check** that `NOTION_WHO_AM_I` through `hsc` names the HSC workspace,
   then go on to §4.2.

Then check each of the other connectors and name any that is missing.

**Gmail drafts.** The run writes drafts, so Gmail needs Google's "Manage
drafts and send emails" permission, which Google's consent screen offers as a
checkbox that can be left unticked. Reading the mailbox works without it, so
a read proves nothing, and setup never creates a draft to test it. When they
connect Gmail, or when a run has logged a refused draft (daily-outreach-run
§4.6), tell them: in Claude, Customize → Connectors → Gmail → Disconnect; at
https://myaccount.google.com/connections remove Claude, so Google asks for
every permission again; then connect Gmail and tick "Manage drafts and send
emails" before continuing. The run never sends: the permission's name covers
both.

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
them with every Notion user they have. Its columns are Person, Notion
accounts and Notes; a "Not yet named" row holds sponsors ("Owner of …") for
someone outside the workspace.

**Their sponsors.** Offer as choices, in the one question about sponsors,
the CRM rows that have no Account Owner and that no Owners row holds, plus
each "Not yet named" row as a choice of its own ("I'm the owner of Fidelity,
Rho Bank, …"), listing them in the question when there are more than fit.
Set them as Account Owner (`NOTION_UPDATE_ROW_DATABASE` through `hsc`, type
`people`) on the rows they pick and, when they pick a "Not yet named" row,
on the unowned rows it holds. Never change a row that has an owner: a co-owner,
or a row they say is theirs that someone else owns, is for them and that
owner to settle.

**Their Owners row.** Add one if it is missing; when a "Not yet named" row
is theirs, fill that row instead: their name under Person, their Notion users
under Notion accounts, and its "Owner of …" line moved to Notes in place of
"Outside the workspace; add their name here". Composio's
Notion tools cannot write a table row, so find the table and its rows with
`NOTION_FETCH_BLOCK_CONTENTS` through `hsc`, then use the Notion API from
`COMPOSIO_REMOTE_WORKBENCH` (`proxy_execute`). The proxy cannot pick an
account and uses the default one, so first
`GET /v1/blocks/<Owners table block id>` through it; if that fails, it is
reaching another workspace: ask them to make the edit in Notion by hand. A
new row is `PATCH /v1/blocks/<Owners table block id>/children` with one
`table_row` — a cell for each column, filled like the rows above it, the
second mentioning their Notion user — and `after` set to the last named
person's row, so it stays above the "Not yet named" rows. A filled row is
`PATCH /v1/blocks/<its block id>` with its new cells.

### 4.3 Their network

1. Ask whether they will share their warm-path map with the event team. If
   yes, each teammate they name is granted by name at
   https://prospecting.hscompute.com/settings/sharing: do it in the browser
   when this session has one and they asked you to, otherwise send them
   there. Sharing is read-only, and teammates see their contacts only where
   they match a sponsor.
2. Ask them to link their address book at
   https://prospecting.hscompute.com/settings/contacts (Google, or iCloud for
   iPhone contacts with an app-specific password they create themselves). It
   keeps itself in sync from then on. Every contact is kept, so a company
   added later finds who they already know there; phone numbers are kept
   encrypted for them alone, never email or street addresses.
3. Unless an export is already requested or LinkedIn's email has come, ask
   for LinkedIn's data export (Settings → Data privacy → Get a copy of your
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
  tracker: <database URL> (Composio account <name>). Beeper: on this
  computer. They are not watching; follow the skill exactly.` Leave
  out the Personal CRM sentences or the Task tracker sentence when they have
  none. The Beeper sentence follows their answer (§4): "Beeper: on this
  computer." when they text sponsors and Beeper's tools are in this session
  (§4.1), "Beeper: off." when they do not, and none when they do but Beeper
  Desktop is not on this computer yet.
- **Require this computer: on.** Always, for every teammate. The run reads
  Beeper and other desktop connectors only on the computer it was scheduled
  from, and a run that wanders off it reads less without saying so.
- Notifications on, so the digest and the "a sponsor is waiting on you"
  messages reach their phone.

**One task.** Read their task list before creating or changing anything.
When more than one task is named `Daily outreach run · <first name>`, or one
still runs once a day with a pasted copy of the skill in its prompt instead
of the line above, keep the hourly one, change it as above, and tell them to
turn the others off in the desktop app's scheduled tasks. Turning a task off
or deleting it is theirs to do. A duplicate left on writes a second Run log
row every day and drafts from an older copy of the procedure.

Set "Require this computer" with the scheduling tool. If the tool cannot set
it, or an existing task has it off, stop here: tell them to open the Claude
desktop app's scheduled tasks, open `Daily outreach run · <first name>`, and
turn on **Require this computer**, then wait until they say it is on. Do not
start §4.6 until it is: setup is not done without it.

### 4.6 First run

Run the daily-outreach-run skill now for them, as a backfill. It writes their
first Run log row; check through `hsc` that the row exists. End with what got
set up, what is still theirs to do, and their first digest. When
`post_to_slack` answers `posted: false` because Slack is not configured, that
is not a failure: give the digest here, and name Slack as an admin's fix.

## 5. Central setup (Evan only)

Create, once, on Evan's account, and skip any that already exists under any
name. The watchdog and lead scout read and write the HSC workspace, so
Evan's account needs Composio with `hsc` connected first (§4.1). Read his
task list first: a second copy of a central task, or an old one whose prompt
pastes a copy of a skill, is his to turn off (§4.5).

Every one of them requires this computer (§4.5): set it, or stop until
Evan has turned it on, for a task created now and for one that already
exists. Each has notifications on. Where a task below already exists with a
different prompt, change the prompt to the one here; for a task that
requires a computer, that change takes effect only when approved from a
conversation linked to that computer.

1. **Watchdog** — the outreach-watchdog skill every four hours. Name
   `Outreach watchdog`, cron `0 */4 * * *` (the scheduler picks the minute).
   Prompt:

   ```text
   Run the outreach-watchdog skill for the HSC outreach automation (central job, set up by Evan). Read and write the Run log only through the Composio Notion account "hsc", and update the "Watchdog heartbeat" row as the skill describes. Evan is not watching; follow the skill exactly.
   ```

2. **Crawl** — the warm-path-map skill on weekdays, on the computer whose
   Chrome holds Evan's LinkedIn session, so the crawl can open pages. Name
   `Warm-path crawl · weekdays`, cron
   `CRON_TZ=America/Los_Angeles 0 9 * * 1-5`. Prompt:

   ```text
   You are a scheduled weekday task for Evan (evan@foreach.do is this claude account, evan@hscompute.com is the account at hscompute.com). Your job: work ALL outstanding warm-path-map queues using the warm-path-map skill. Evan has given standing authorization for every job the skill defines (seeding, fund research, company scans, fund-page scans, mutual enumeration, corrections, panel views) — but the skill's own safety and pacing rules (its §1 and §5.1) override everything in this prompt. Never use pacing overrides to go faster, and never click anything on LinkedIn other than what the skill permits.

   TIMING (all times America/Los_Angeles):
   1. This task fires at 9:00am PT. Before doing any work, draw a random integer 0–3600 (e.g. python3 -c "import random; print(random.randint(0,3600))") and sleep that many seconds (in chunks of ≤9 minutes), so the real start is uniformly random between 9:00 and 10:00am PT.
   2. At start, also draw a random stop time uniformly between 7:00pm and 8:00pm PT. Plan the day so you stop opening new page work with enough margin to finish cleanly by that time: send correct statuses (partial for anything unfinished — never a terminal status just to drain the queue), call run_summary, and write your report.

   PREFLIGHT:
   3. Verify the prospecting MCP tools are available. If they are not, STOP and report that — do nothing else.
   4. Check whether Claude-in-Chrome browser tools are reachable (they come through Evan's desktop app bridge). If they are NOT available, do only the non-browser work: fund portfolio research off LinkedIn, import_targets/import_funds seeding, and corrections if collection queues are empty. Report clearly that browser scanning was skipped and why.
   5. If the browser IS available, run the skill's preflight search (§5.3) before working any batch. Untrustworthy reads (empty/placeholder results, challenges, interstitials, login walls, layout drift) END the run per the skill: send partial where applicable, call run_summary, report what the page showed, and stop. Do not adapt around obstacles or retry beyond what the skill allows.

   WORK:
   6. Invoke the warm-path-map skill and follow it exactly. Ask next_work { kind: "all" } and work the queues in cascade order: research_fund, scan_company, enumerate_mutuals, then scan_fund_page (Evan has standing authorization for fund-page scans; pass fund on those ingests only). Open a run with record_run and pass its run_id to every ingest. Ingest after EACH company, never batched at the end.
   7. Never mix correction work into a collection pass. Only if all collection queues are empty, run one correction pass (find_suspect_entries → inspect before acting → smallest true correction). Never merge_people or delete_entry unless the evidence is unambiguous; if unsure, leave it and note it in the report.
   8. If the Chrome extension disconnects, follow the skill's recovery steps (sleep 30, retry tabs_context up to 4×, re-ask next_work under the same run_id; use list_runs if the run_id is lost).

   WRAP-UP:
   9. Before the stop time: run_summary, then report — companies scanned, cards seen, mutuals named, dropped counts (call out any non-zero droppedNoUrl as an incomplete pass), what remains in next_work, any companies left with the expansion button still showing, and anything you stopped on.

   Evan is not present during this run. Never wait for input. Any judgement the skill reserves for the user (e.g. resuming after an untrustworthy read) means: stop and report, not decide.
   ```

3. **Lead scout** — the lead-scout skill daily, each morning. When a weekly
   task already exists, change its schedule rather than adding another. Name
   `Lead scout`, cron `CRON_TZ=America/Los_Angeles 50 9 * * *`: after 9am,
   when the computer it requires is awake. Prompt:

   ```text
   Run the lead-scout skill for the daily lead scout run (central job, set up by Evan). Use the Composio Notion account "hsc" for the Rules page and Sponsors CRM, and the HSC prospecting connector for routes. Evan is not watching; follow the skill exactly.
   ```

Then confirm the Run log's "Watchdog heartbeat" row updates after the first
watchdog run. Wait for its first scheduled run rather than starting one: it
posts what it finds to the alerts channel, and nothing goes to that channel
without Evan's yes, a test message included.
