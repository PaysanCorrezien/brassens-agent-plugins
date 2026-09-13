# Brassens agent plugins

Official plugins that connect Claude Code and Codex to your
[Brassens](https://brassens.app) history (dictations, meetings, media imports)
through the Brassens remote MCP server at `https://mcp.brassens.app/mcp`. Read-only.

Current version: **1.0.0** (see [CHANGELOG.md](CHANGELOG.md)).

> This repository is generated from the Brassens monorepo on each release.
> Do not open pull requests here; changes are overwritten by the next release.

## Requirements

- An active paid Brassens Pro subscription (free trials are not included).
- Cloud data retention turned on in Brassens Settings > Data & Privacy.
- Agent access enabled for your account (rolling out progressively).

## Claude Code

Inside Claude Code:

```
/plugin marketplace add PaysanCorrezien/brassens-agent-plugins
/plugin install brassens@brassens
```

or from a shell:

```sh
claude plugin marketplace add PaysanCorrezien/brassens-agent-plugins
claude plugin install brassens@brassens
```

Then run `/mcp`, select `brassens` and sign in with your Brassens account.

## Codex

```sh
codex plugin marketplace add PaysanCorrezien/brassens-agent-plugins
codex plugin add brassens@brassens
codex mcp login brassens
```

Without plugins, copy [`codex/config.toml.example`](codex/config.toml.example)
into `~/.codex/config.toml` and run `codex mcp login brassens`. Keep
`oauth_resource`: the server rejects tokens minted for any other audience.

## Any other MCP client (Claude.ai, ChatGPT, ...)

Add `https://mcp.brassens.app/mcp` as a custom connector (Claude.ai: Settings > Connectors >
Add custom connector; ChatGPT: developer mode connector). Sign-in is discovered
automatically through OAuth; no client id or secret is needed.

Always use the `mcp.brassens.app` host. Other Brassens hostnames are not
registered as an OAuth resource and sign-in will fail there.

## License

See [LICENSE](LICENSE).
