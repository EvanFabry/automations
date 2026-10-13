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

    /plugin install hsc-outreach --marketplace EvanFabry/automations

This plugin brings two connectors: prospecting (the HSC panel) and Composio,
which replaces Claude's built-in Notion connector. Connect Notion through
Composio once per workspace, labelling the shared HSC workspace exactly
"hsc", then disconnect the built-in Notion connector.

## Use it

Say "set up my outreach", or run /outreach-setup, once. It checks your
connectors, confirms you can see the CRM, records your LinkedIn connections as
routes, schedules your daily outreach run and runs it once. After that, a
digest arrives each morning.

## Data

Claude reads your Gmail, Granola and (on your computer) Beeper inside your own
account, and Notion through your own Composio account, which holds that
connection. The prospecting connector at
prospecting.hscompute.com stores who you are in touch with — name, company,
channel and date, never message text, phone numbers or email addresses —
visible to you, and to teammates you share your map with only where it matches
a sponsor. Nothing is ever sent on your behalf.
