# Lucive plugin

Connects a Lucive Planning account (https://lucive.app) to Claude and
ChatGPT/Codex. Tasks, intents, and scratchpads; the Journal is never part of
the connection.

One directory, both formats: `.claude-plugin/plugin.json` (Claude) and
`.codex-plugin/plugin.json` (ChatGPT/Codex) wrap the same skill and the same
remote MCP server (`https://mcp.lucive.app/api/mcp`, OAuth discovered).

- Claude: `/plugin marketplace add <this repo's plugins dir>` then install
  `lucive@lucive`, or `claude --plugin-dir ./plugins/lucive` for a session.
- ChatGPT/Codex: `codex plugin marketplace add <plugins dir>`, then install
  from the app's Plugins screen (Personal tab).
- Before OpenAI submission, `.app.json` gains the platform-registered app id.
