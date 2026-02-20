# mcp-sbb-transport

MCP server for Swiss public transport queries via the [opendata.ch Transport API](https://transport.opendata.ch).
Covers trains, buses, trams, and boats across Switzerland — no API key required.

**Status:** Ready

---

## Prerequisites

- [Bun](https://bun.sh) runtime (`curl -fsSL https://bun.sh/install | bash`)
- No API keys needed — uses the public opendata.ch transport API

---

## Install

```bash
git clone git@github.com:McCullonas/mcp-sbb-transport.git
cd mcp-sbb-transport
bun install
```

---

## Available Tools

### `sbb_search_stations`
Search for Swiss public transport stations by name.

**Parameters:**
- `query` — Station name (e.g. `"Zürich HB"`, `"Bern"`, `"Luzern"`)
- `limit` *(optional)* — Max results (default 10)

Returns: station ID, name, and coordinates.

### `sbb_get_connections`
Get train/public transport connections between two Swiss stations.

**Parameters:**
- `from` — Origin station name or ID
- `to` — Destination station name or ID
- `date` *(optional)* — Date as `YYYY-MM-DD` (default: today)
- `time` *(optional)* — Time as `HH:MM` (default: now)
- `isArrivalTime` *(optional)* — If true, `time` is the desired arrival time
- `limit` *(optional)* — Number of connections to return (default 4)

Returns: departure/arrival times, duration, number of transfers, platform numbers, journey sections.

### `sbb_get_stationboard`
Get the departure or arrival board for a Swiss station.

**Parameters:**
- `station` — Station name or ID
- `datetime` *(optional)* — ISO 8601 datetime (default: now)
- `type` *(optional)* — `"departure"` or `"arrival"` (default: departure)
- `limit` *(optional)* — Number of entries (default 10)

Returns: transport category (IC, IR, S, Bus, etc.), line number, destination/origin, scheduled time, platform.

---

## MCP Client Configuration

Add to your `settings.json`:

```json
{
  "mcpServers": {
    "sbb-transport": {
      "command": "bun",
      "args": ["run", "/path/to/mcp-sbb-transport/src/index.ts"]
    }
  }
}
```

No environment variables required.

---

## Development

```bash
bun run src/index.ts
bun build src/index.ts --outdir dist --target node
```

---

## Data Source

Uses the [opendata.ch Swiss Transport API](https://transport.opendata.ch), a community-run
open data service. Free to use, no registration required. Data comes from SBB, PostBus,
cantonal operators, and other Swiss public transport providers.

---

## Licence

MIT
