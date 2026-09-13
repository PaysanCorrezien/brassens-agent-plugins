# Changelog

All notable changes to the Brassens agent plugins (Claude Code and Codex) are
recorded here. Versions follow [Semantic Versioning](https://semver.org). One
version is shared by both plugin manifests and both marketplace entries; the
validator enforces it, and a release is cut by pushing the tag
`agent-plugins-v<version>` (see `docs/publishing.md`).

## [1.0.0] - 2026-09-13

### Added

- First public release, published to `PaysanCorrezien/brassens-agent-plugins`.
- Claude Code plugin `brassens` (marketplace `brassens`): remote MCP connector
  and the `brassens-history` skill.
- Codex plugin `brassens` (marketplace `brassens`): the same connector with
  `oauth_resource`, the same skill, and a plain `config.toml` fallback.

### Changed

- Canonical MCP URL is now `https://mcp.brassens.app/mcp` (was
  `https://api.brassens.app/mcp` before release). It is also the OAuth resource
  indicator.
