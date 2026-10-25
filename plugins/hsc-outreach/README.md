# HSC Sponsor Outreach

Keeps the Haight Street Compute Sponsors CRM current from your own email,
meetings and chats, ranks the warmest route into every sponsor across the
team's shared network, and leaves follow-up drafts in your Gmail for you to
send.

## Install

The full instructions, with every download, are the install guide:
https://prospecting.hscompute.com/settings/install

It installs from the EvanFabry/automations marketplace, which keeps it current.
In Claude Code:

    /plugin marketplace add EvanFabry/automations
    /plugin install hsc-outreach@hsc

To update: `claude plugin marketplace update hsc`, then
`claude plugin update hsc-outreach@hsc`. In the Claude app,
Customize → Plugins → Check for updates, or turn on Sync automatically.

This plugin carries the skills. Add the two connectors it needs from the
guide's links: HSC (this panel) and Composio, which replaces Claude's built-in
Notion connector. Connect Notion through Composio, labelling the shared HSC
workspace exactly "hsc", then disconnect the built-in
Notion connector.

## Use it

Say "set up my outreach", or run /outreach-setup, once. It checks your
connectors, confirms you can see the CRM, records your LinkedIn connections as
routes, schedules your outreach run hourly and runs it once. After that, your
CRM rows stay current through the day, a digest arrives once each morning,
and you hear in between only when a sponsor is waiting on you or a draft is
ready.

## Data

Claude reads your Gmail, Granola and (on your computer) Beeper inside your own
account, and Notion through your own Composio account, which holds that
connection. The prospecting connector at
prospecting.hscompute.com stores who you are in touch with — name, company,
channel and date, never message text, phone numbers or email addresses —
visible to you, and to teammates you share your map with only where it matches
a sponsor. Nothing is ever sent on your behalf.
