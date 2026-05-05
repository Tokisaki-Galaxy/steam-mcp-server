# Steam MCP Server

An MCP (Model Context Protocol) server that provides tools for interacting with the Steam Web API.

## Setup

### 1. Get a Steam API Key

Obtain an API key from [Steam's developer portal](https://steamcommunity.com/dev/apikey).

### 2. Install Dependencies

```bash
npm install
```

### 3. Build

```bash
npm run build
```

### 4. Run Locally (Cloudflare Worker)

Create a `.dev.vars` file for Wrangler:

```bash
STEAM_API_KEY=your-api-key-here
STEAM_ID=your-64-bit-steam-id
```

Start the Worker:

```bash
npx wrangler dev
```

### 5. Deploy

```bash
npx wrangler secret put STEAM_API_KEY
# Optional default Steam ID
npx wrangler secret put STEAM_ID
npx wrangler deploy
```

### 6. Configure Your MCP Client

Use the HTTP MCP endpoint exposed by the Worker:

- Local dev: `http://127.0.0.1:8787/mcp`
- Production: `https://<your-worker>.workers.dev/mcp`

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `STEAM_API_KEY` | Yes | Steam Web API key (Wrangler secret/var) |
| `STEAM_ID` | No | Default Steam ID to use when not specified in tool calls |
| `STEAM_MCP_ALLOWED_ORIGINS` | No | Comma-separated list of allowed CORS origins |

When `STEAM_ID` is set, you can call tools like `get_owned_games` without passing a Steam ID - it will use your default profile automatically.

When `STEAM_MCP_ALLOWED_ORIGINS` is set, CORS headers are only returned for matching origins.

## Available Tools

### Social & Profile
| Tool | Description |
|------|-------------|
| `get_player_summary` | Get player profile info (name, avatar, status, current game) |
| `get_friends_list` | Get a player's friends list |
| `get_steam_level` | Get player's Steam account level |
| `get_badges` | Get player's badges, XP, and level progression |
| `get_badge_progress` | Get trading card collection progress |
| `get_player_bans` | Check for VAC bans, game bans, or trade bans |
| `get_user_groups` | Get Steam groups a player belongs to |
| `resolve_vanity_url` | Convert vanity URL to 64-bit Steam ID |

### Game Library
| Tool | Description |
|------|-------------|
| `get_owned_games` | Get all games owned with playtime stats (supports pagination) |
| `get_recently_played` | Get games played in last 2 weeks |
| `get_game_details` | Get detailed game info (description, price, requirements) |
| `is_playing_shared_game` | Check if playing via Steam Family Sharing |
| `search_apps` | Search Steam catalog by game name |

### Achievements & Stats
| Tool | Description |
|------|-------------|
| `get_achievements` | Get player's achievements for a game |
| `get_game_stats` | Get player's statistics for a game |
| `get_global_achievement_percentages` | Get global achievement unlock rates |
| `get_global_game_stats` | Get global aggregated stats for a game |
| `get_perfect_games` | Get games where player has 100% achievements |
| `get_achievement_summary` | Get condensed achievement progress across games |
| `get_game_schema` | Get achievement/stat definitions for a game |

### Game Info
| Tool | Description |
|------|-------------|
| `get_game_news` | Get latest news and patch notes for a game |
| `get_player_count` | Get current number of players in a game |
| `get_servers_at_address` | Get game servers at a specific IP |
| `check_app_update` | Check if an app version is up to date |

### Inventory
| Tool | Description |
|------|-------------|
| `get_inventory` | Get inventory for any game (requires public profile) |
| `get_tf2_inventory` | Get Team Fortress 2 inventory |
| `get_csgo_inventory` | Get CS2/CSGO inventory |
| `get_dota2_inventory` | Get Dota 2 inventory |

## Finding Your Steam ID

Use `resolve_vanity_url` with your custom profile URL, or find your 64-bit Steam ID at [steamid.io](https://steamid.io/).

## License

MIT
