# QuotedPart plugins

Plugins that connect AI assistants to your [QuotedPart](https://quotedpart.com) account: price a CNC
machined, turned, laser-cut or 3D-printed part from its STEP/IGES model or PDF drawing, get a job-shop
style cost breakdown with price breaks by quantity, and open the saved quote in your account.

This repository holds only the plugin packaging. The plugin connects to the hosted QuotedPart MCP
server at `https://quotedpart.com/mcp` (Streamable HTTP). It requires signing in with an existing
QuotedPart account (OAuth); the assistant asks you to sign in the first time it uses QuotedPart.

| Assistant | Install |
| --- | --- |
| Claude Code | `/plugin marketplace add Helios-One-Oy/quotedpart-plugins` then `/plugin install quotedpart@quotedpart` |
| Codex | `codex plugin marketplace add Helios-One-Oy/quotedpart-plugins` then `codex plugin add quotedpart@quotedpart` |
| Claude.ai, ChatGPT | Add `https://quotedpart.com/mcp` as a custom connector and sign in (OAuth) |
| Any MCP client | Streamable HTTP endpoint `https://quotedpart.com/mcp` with MCP authorization (OAuth) |

<!-- Demo video and screenshots: re-recorded against the signed-in flow before release. -->

Details, tools and options: [plugins/quotedpart/README.md](plugins/quotedpart/README.md).

## Layout

```
.claude-plugin/marketplace.json     Claude Code marketplace "quotedpart"
.agents/plugins/marketplace.json    Codex / Agent Plugins marketplace "quotedpart"
plugins/quotedpart/
  .claude-plugin/plugin.json        Claude Code plugin manifest
  .mcp.json                         Claude Code MCP config (URL overridable with QUOTEDPART_MCP_URL)
  plugin.json, mcp.json             Agent Plugins package for ChatGPT and Codex
  skills/quote-a-part/SKILL.md      how the assistant should use the tools
  assets/icon.png                   icon
  codex/config.toml                 plain Codex MCP config, without the plugin
```

## Privacy, terms, support

Parts you price are sent to QuotedPart and saved as quotes in your account. Privacy: https://quotedpart.com/privacy.
Terms: https://quotedpart.com/terms. Support: support@quotedpart.com.

## License

[MIT](LICENSE) © Helios One Oy. The license covers the files in this repository; the QuotedPart
service is covered by its terms of service.
