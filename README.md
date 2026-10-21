# Local Rank and Map Pack Tracker MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-local-rank-map-pack-tracker)](https://smithery.ai/servers/mambabuilt/mcp-local-rank-map-pack-tracker) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-local-rank-map-pack-tracker/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-local-rank-map-pack-tracker) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-local-rank-map-pack-tracker%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-local-rank-map-pack-tracker&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-local-rank-map-pack-tracker)](https://www.npmjs.com/package/@mambalabsdev/mcp-local-rank-map-pack-tracker) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-local-rank-map-pack-tracker)](https://www.npmjs.com/package/@mambalabsdev/mcp-local-rank-map-pack-tracker) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-local-rank-map-pack-tracker)](https://github.com/mambalabsdev/mcp-local-rank-map-pack-tracker/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-local-rank-map-pack-tracker)

An MCP server for the Mamba Labs Local Rank and Map Pack Tracker actor on Apify. Give it keywords, locations, and a domain or business name. Each run searches Google from each location and returns one row per keyword and location with the organic position, the map pack position, and the local finder position.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

- Each place name is turned into coordinates and the Google search is placed at that point, so a check for Austin returns the Austin map pack.
- Per row: `organic_position` (up to 50 deep), `local_pack_position` (1 to 3), `local_finder_position` (top 20), and the matched listing's rating, review count, category, phone, website, and address.
- `top_organic_domains` and `top_local_businesses` name who holds the top spots.
- With `state_key`, each row carries the previous positions and the change, so a weekly schedule becomes a rank history.
- An empty position means not found in the positions checked. Blocked rows come back free with a `row_status` saying so.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-local-rank-map-pack-tracker": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-local-rank-map-pack-tracker"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `track_local_rankings` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Where does weplumbatx.com rank for plumber and emergency plumber in Austin and Round Rock?"
- "Check the map pack position of We Plumb LLC for drain cleaning in 30.2672,-97.7431."
- "Track my dental clinic for dentist near me in five London boroughs every week and show what changed."

## Tool

`track_local_rankings`. One tool. Example arguments:

```json
{
  "keywords": ["plumber", "emergency plumber"],
  "locations": ["Austin, Texas, United States", "Round Rock, Texas, United States"],
  "target_domain": "weplumbatx.com",
  "business_name": "We Plumb LLC"
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and booleans are sent as strings.

| Input | Type | Description |
|---|---|---|
| `keywords` | array | Search terms. Required |
| `locations` | array | Place names or coordinates. Required |
| `target_domain` | string | Your website. Give it, `business_name`, or both |
| `business_name` | string | The name on your Google Business Profile |
| `country` | string | Google market and proxy exit. Default US |
| `language` | string | Google interface language. Default en |
| `organic_depth` | string | 10, 20, 30, or 50. Default 10 |
| `include_local_finder` | string | true or false. Default true |
| `state_key` | string | Schedule name for change tracking |
| `max_charge_usd` | string | Optional cost cap for the run |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `rank-check`, one keyword in one location, Google page one included | $0.02 | $0.019 | $0.018 | $0.017 |
| `extra-page`, organic page 2 and up, or the local finder | $0.018 | $0.0171 | $0.0162 | $0.0153 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

Blocked and error rows are free.

## Your keys

The actor needs no key. It reads Google result pages through the Apify Google SERP proxy and holds no key of any kind. Mamba Labs pays the platform and proxy costs of the run.

## Full actor documentation

[apify.com/mambalabs/local-rank-map-pack-tracker](https://apify.com/mambalabs/local-rank-map-pack-tracker)

## License

MIT. See [LICENSE](LICENSE).
