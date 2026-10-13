---
name: lead-scout
description: Use to find new sponsor leads for the Haight Street Compute Market and add them to the Sponsors CRM for the team to approve — companies the team can already reach, plus recent AI-infrastructure news — screened against the Rules page and ranked by warmest route. Also use for "find more sponsors", "who else should we pitch", or the daily lead scout run. Requires Notion through Composio (the HSC workspace connected as `hsc`) and the prospecting server.
---

# Lead scout

Add a short list of good new sponsor leads to the Sponsors CRM each day,
each with a reason and the warmest way in, for a person to approve or drop.
Each lead is proposed once, ever (§3.1).

## 0. How to read this file

**Sections are numbered in precedence order; the lower number wins.** Policy
— the sponsor screen, do-not-contact list, stage meanings — lives on the
Rules page (§2). Read it first, every run.

## 1. Safety — non-negotiable

- **Never send.** No email, message or post to anyone outside the team. The
  only post is this run's summary to the team (§4.6).
- **Do-not-contact and the sponsor screen are absolute.** Never add, research
  further or route through anyone the Rules page excludes.
- **Only the `hsc` account.** Read and write Notion only through the Composio
  Notion account `hsc` (§2), never by search, and never in another workspace:
  a person's own CRM or task tracker is never touched. If `hsc` cannot be
  selected, or the Sponsors CRM is not found through it, stop: log the run
  failed with Fix "Connect the HSC Notion workspace in Composio as `hsc`".
  When the Run log is what cannot be reached, end with that Fix in the post
  instead.
- **Connections are a person's job.** Never connect, reconnect or disconnect
  an account in Composio or Claude.
- **New rows only.** Never edit or move an existing row. A company already in
  the CRM, under any spelling, is not a lead.
- **Leads, not commitments.** Every row you add has Status Lead and no Account
  Owner. A person approves by giving it an owner, or drops it by setting Lost.
- **At most ten rows per run.** Fewer, better leads beat a long list.
- **Public facts only in Notes.** One line on why, with a source link. Never
  a person's email address, phone number or anything from a private message.
- **State holds keys, timestamps and IDs only.** What `put_state` stores
  (§4.6) is §3.1 keys and dates: never a person's name, an email address, a
  phone number or message text.

## 2. Locations

| What | Where | ID for Composio's Notion tools |
| --- | --- | --- |
| Rules page | https://app.notion.com/p/3f81ec23f3cf81a6b3d6fe73edd7cdcb | page `3f81ec23f3cf81a6b3d6fe73edd7cdcb` |
| Sponsors CRM | collection://6451ec23-f3cf-83f1-a4f7-87bab3ce5292 | data source `6451ec23-f3cf-83f1-a4f7-87bab3ce5292`, database `e4d1ec23f3cf8227bddd01abe67cc8d9` |
| Run log | collection://9c977f46-c7b2-4126-9357-781d56a057f4 | data source `9c977f46-c7b2-4126-9357-781d56a057f4`, database `64efc12133db449187e39aae74145619` |
| Warm-path map | the `prospecting` MCP server (prospecting.hscompute.com) | |

**Notion goes through the Composio connector, one connected account per
workspace.** The HSC workspace, which holds everything above, is the account
named `hsc`. Select it on every call: run Notion tools through
`COMPOSIO_MULTI_EXECUTE_TOOL` with `account: "hsc"` on each item in `tools`,
never as a Notion tool called on its own, which may not carry the account.
`NOTION_GET_PAGE_MARKDOWN` reads the Rules page, `NOTION_QUERY_DATA_SOURCE`
reads rows by data source ID, and `NOTION_INSERT_ROW_DATABASE` adds one by
database ID. Check arguments with `COMPOSIO_GET_TOOL_SCHEMAS`; if a slug is
gone, find its replacement with `COMPOSIO_SEARCH_TOOLS`.

## 3. What a good lead is

A company that would plausibly pay to be in the room where compute is bought
and sold: AI labs and AI-heavy startups buying compute, neoclouds and GPU
clouds selling it, datacenter operators and developers, hardware makers,
funds with AI-heavy portfolios, and the banks, lenders and law firms that
finance and paper those deals. The CRM's Category options name the kinds the
team already tracks; use them.

Rank by, in order: a warm route exists (`best_routes` tier 1–3); a recent
reason to talk now (a raise, a capacity announcement, a datacenter deal in
the last 60 days); fit with the sponsor screen.

### 3.1 Same lead, same key

The daily outreach run proposes leads too, from each owner's conversations,
under this same rule, so the two jobs never propose one company twice.

<!-- shared: lead-key — identical in daily-outreach-run and lead-scout; a test holds them equal -->
A lead's key is its normalized name and, when known, its domain, written
`<name>|<domain>`. Normalize a name by lowercasing it, turning "&" into
"and", dropping punctuation and a leading "the", collapsing spaces, then
dropping trailing words from this list until none is left at the end: inc,
incorporated, llc, ltd, limited, corp, corporation, co, company, gmbh, plc,
labs, ventures, technologies, group, holdings. The domain is the company's
own web domain, lowercased, without "www." or a path; a free-mail domain
(gmail.com, outlook.com, icloud.com, yahoo.com, proton.me) is no domain.

Two leads are the same when their names match after normalizing, or their
domains match. A lead is proposed once: skip it when it is the same as an
entry in this job's state `seen.leads`, or as any row of the database it
would be written to, under any status, Lost included. Look that database up
before every write; state alone is not enough, because another job or a
person may have added the row. After judging a lead, whether it was written
or screened out, add `{ key, at }` to `seen.leads`, keeping the newest 500.
<!-- /shared: lead-key -->

## 4. Procedure

### 4.1 Read

`get_state`, job `lead-scout`: `{ "seen": { "leads": [ { "key", "at" } ] } }`,
or null on the first run. Then, through `hsc`: the Rules page, and every row
of the CRM under any Status, as §3.1 keys.

### 4.2 Candidates

1. **From the map.** `list_companies` (follow `nextCursor` to the end):
   companies with a 1st-degree contact or a decision-maker, not in the CRM.
2. **From the news.** Search for the last 60 days of AI-infrastructure news:
   funding rounds, GPU cloud capacity, datacenter financings and leases.
   Keep companies, not people.

### 4.3 Screen

Drop anything the Rules page excludes, anything already in the CRM or in
`seen.leads` (§3.1), and anything you cannot say in one line why it fits §3.

### 4.4 Routes

Call `best_routes` once with every remaining candidate. Keep the best ten by
§3's order.

### 4.5 Write

Create one CRM row per lead, through `hsc`: Sponsor (the company's name),
Category, Status Lead, Priority (High when the route is tier 1–3, else
Medium), Best route and Route holder from `best_routes` when there is one,
and Notes: `Lead scout <date>: <why, one line> <source link>`. Look the CRM
up again just before each write (§3.1). Add every candidate judged in §4.3
and §4.4, written or dropped, to `seen.leads`.

### 4.6 Log and report

`put_state` with `seen.leads` (newest 500) and `expected_updated_at` set to
the `updated_at` that `get_state` returned. An answer starting "conflict:"
means another run wrote first: call `get_state` again, merge `seen.leads` as
the union by key, newest `at` kept, and retry once; if that also conflicts,
log partial with Error "state conflict".

Write one Run log row per day through `hsc`, finding it by title before
creating it: Run "Lead scout · <date>", Job "Lead scout", Result, Started,
Finished, Next run due (one day after Started), Rows touched, Summary,
Error, Fix. Then post to `alerts` with
`post_to_slack`: one line per lead (name, why, route), and "Give a lead an
Account Owner to take it, or set Lost to drop it." End the run with the same
lines.
