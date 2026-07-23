# Sixtyfour Codex Plugin

Codex plugin that connects to Sixtyfour's hosted [Intelligence MCP server](https://docs.sixtyfour.ai/developer-tools/mcp-setup). Search and enrich people and companies, find emails and phones, reverse-lookup contacts, and check credit balance — all from Codex.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## Tools

This plugin bundles the remote `sixtyfour-intelligence` MCP server at `https://mcp.sixtyfour.ai/mcp`. It does not ship server code; Codex talks to the hosted endpoint.

See the full tool reference in our [MCP setup docs](https://docs.sixtyfour.ai/developer-tools/mcp-setup#intelligence-mcp-reference).

## Install

Installing has two steps: register this repo as a plugin marketplace, then install the plugin from it. This repo is public, so both steps work directly against GitHub — no listing in OpenAI's curated Plugins Directory required.

### Codex CLI

1. Register the marketplace:
   ```bash
   codex plugin marketplace add sixtyfour-ai/sixtyfour-codex-plugin
   ```
2. Install the plugin from it:
   ```bash
   codex plugin add sixtyfour@sixtyfour
   ```
3. Start a **new** `codex` session — plugins load at session start, so a session already running before you installed won't pick it up.

Check anytime with `codex plugin list`.

### ChatGPT desktop app

1. Run step 1 above from a terminal to register the marketplace (or add a personal marketplace entry — see [Build plugins](https://developers.openai.com/codex/plugins/build)).
2. Open the plugin directory (Work or Codex → Plugins), find **Sixtyfour**, and install it.
3. Restart the app if prompted.

## Authentication

The intelligence server requires a Sixtyfour account. Two options:

### OAuth (default)

The bundled [`.mcp.json`](.mcp.json) uses the bare MCP URL, so on first use Codex needs to sign you in via browser OAuth — same flow as the [Codex section of our MCP docs](https://docs.sixtyfour.ai/developer-tools/mcp-setup#codex).

**Codex CLI:** you'll see a warning like `sixtyfour-intelligence MCP server is not logged in` the first time it tries to start. Run:

```bash
codex mcp login sixtyfour-intelligence
```

This opens the browser sign-in flow. Once it completes, start a new `codex` session so the MCP server starts up with the stored credentials.

**ChatGPT desktop app:** the sign-in prompt appears automatically the first time you use a Sixtyfour tool — no manual login command needed.

### API key

If you prefer an API key instead of OAuth, override the server URL to include your key:

```text
https://mcp.sixtyfour.ai/mcp?api_key=YOUR_API_KEY
```

Get a key at [app.sixtyfour.ai/keys](https://app.sixtyfour.ai/keys).

Do not commit API keys. See [`.env.example`](.env.example) if you keep a local key for overrides.

### Reconnecting / refreshing a stale session

How you re-authenticate depends on how you installed:

- **Plugin install (Codex CLI):** run `codex mcp logout sixtyfour-intelligence` followed by `codex mcp login sixtyfour-intelligence`, then start a new session.
- **Plugin install (ChatGPT desktop app):** there's no in-chat reconnect command. Open **Apps/Connectors**, find Sixtyfour, and choose **Reconnect/Sign in**.
- **Manual `config.toml` install (`command = "npx"`, `args = [... "mcp-remote" ...]`):** this path doesn't go through Codex's own auth manager — `mcp-remote` caches its OAuth token locally at `~/.mcp-auth/mcp-remote-<version>/`, so `codex mcp login`/`logout` don't apply. If sign-in seems stuck or the token has gone stale, clear only the Sixtyfour entry (files are prefixed with an MD5 hash of the server URL, so this won't log out any other `mcp-remote` integrations you have configured) and restart Codex to trigger a fresh browser flow:

  ```bash
  find ~/.mcp-auth -type f -name "e7e3f47f4a14aca4e636f38d171a9178_*" -delete
  ```

  If you'd rather reset every `mcp-remote` integration at once (this logs all of them out, not just Sixtyfour), use `rm -rf ~/.mcp-auth` instead. This also fixes the common case where an `mcp-remote@latest` version bump silently orphans the previous token cache.

## Example prompts

- "Research Sixtyfour — return headquarters, headcount, funding stage, and key executives."
- "Enrich this person: Saarth Shah, CEO & Co-Founder at Sixtyfour."
- "Who are the founders of Sixtyfour?"
- "Find the work email for Saarth Shah at Sixtyfour."
- "How many Sixtyfour credits do I have left?"

Enrichment `_start` tools create billable jobs. If a call times out, check status with `sixtyfour_enrichment_job_status` before retrying.

## Documentation

- [MCP setup](https://docs.sixtyfour.ai/developer-tools/mcp-setup)
- [API docs](https://docs.sixtyfour.ai/introduction)
- [Get an API key](https://docs.sixtyfour.ai/get-api-key)

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating you agree to abide by its terms.

## License

Distributed under the MIT License. See [LICENSE](LICENSE) for details.
