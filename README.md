# Sixtyfour Codex Plugin

Codex plugin that connects to Sixtyfour's hosted [Intelligence MCP server](https://docs.sixtyfour.ai/developer-tools/mcp-setup). Search and enrich people and companies, find emails and phones, reverse-lookup contacts, and check credit balance — all from Codex.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## What you get

This plugin bundles the remote `sixtyfour-intelligence` MCP server at `https://mcp.sixtyfour.ai/mcp`. It does not ship server code; Codex talks to the hosted endpoint.

| Capability | Examples |
| --- | --- |
| Company search | Filterable company search, field exploration, detail expansion |
| People search | Filterable people search, field exploration, detail expansion |
| Enrichment | Company/people intelligence jobs, LinkedIn enrich |
| Contact lookup | Find email/phone, reverse email/phone |
| Account | Credit balance |

Full tool reference: [MCP setup docs](https://docs.sixtyfour.ai/developer-tools/mcp-setup).

## Install

### From this GitHub repo (marketplace source)

```bash
codex plugin marketplace add sixtyfour-ai/sixtyfour-codex-plugin
```

Then install **Sixtyfour** from the plugin directory in the ChatGPT desktop app (or your Codex plugin UI), and restart if prompted.

### Local / repo marketplace

This repo includes [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json). From a checkout:

```bash
codex plugin marketplace add ./path/to/sixtyfour-codex-plugin
```

Or point a personal marketplace entry at this plugin folder (see [Build plugins](https://developers.openai.com/codex/plugins/build)).

## Authentication

The intelligence server requires a Sixtyfour account. Two options:

### OAuth (default)

The bundled [`.mcp.json`](.mcp.json) uses the bare MCP URL. On first use, Codex prompts you to sign in via browser OAuth — same flow as the [Codex section of our MCP docs](https://docs.sixtyfour.ai/developer-tools/mcp-setup#codex).

### API key

If you prefer an API key instead of OAuth, override the server URL to include your key:

```text
https://mcp.sixtyfour.ai/mcp?api_key=YOUR_API_KEY
```

Get a key at [app.sixtyfour.ai/keys](https://app.sixtyfour.ai/keys).

Do not commit API keys. See [`.env.example`](.env.example) if you keep a local key for overrides.

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
