# HSC automations

The Claude plugin marketplace for Haight Street Compute. Every file here is
generated from the prospecting repo and published on each change, so edit it
there, not here.

## Add it

In Claude: Customize → Plugins → Add marketplace, and paste
`https://github.com/EvanFabry/automations`. It shows up as **hsc**. Turn on Sync
automatically, then install **hsc-outreach**.

In Claude Code:

```
/plugin marketplace add EvanFabry/automations
/plugin install hsc-outreach@hsc
```

## Plugins

- **hsc-outreach** — sponsor outreach for the Haight Street Compute Market.
  See [its README](./plugins/hsc-outreach/README.md).
