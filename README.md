# Rovian Bot

Discord bot + private Jems API for Rovian Client.

## Deploy to Railway
1. Create a Railway project and choose Deploy from GitHub repo.
2. Select `Heyhello358/rovianbot`.
3. Add a Volume mounted at `/data` so the SQLite database survives redeploys.
4. Add these Railway Variables:
   - `DISCORD_TOKEN`: your Discord bot token
   - `CLIENT_ID`: Discord application ID
   - `GUILD_ID`: Discord server ID
   - `API_SECRET`: a long random secret (keep private)
   - `DROP_ROLE_IDS`: comma-separated Discord role IDs allowed to create/cancel drops; blank means administrators only
   - `DB_FILE`: `/data/jems.sqlite`
5. Railway runs `npm start`. The service listens on Railway's `PORT`; health check is `/health`.

## Commands
- `/link code` — link Minecraft account; first link gives +200 Jems
- `/balance` — show Jems
- `/giftcode create` and `/giftcode redeem`
- `/jems give` and `/jems remove` — administrators
- `/jemdrop create`, `/jemdrop enter`, `/jemdrop cancel`

## API
- `GET /health`
- `POST /api/link/start` — creates a 10-minute linking code
- `GET /api/balance/:uuid`
Private API requests require the `x-rovian-secret` header.

## Security
Development starter only. Never put the bot token or API secret in the Minecraft JAR or public client. The link endpoint must only be called by a trusted backend that has verified the player's Microsoft/Minecraft identity. Use HTTPS and production storage before opening this to public players.
