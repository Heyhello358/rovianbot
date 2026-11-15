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
   - `SHOP_UPLOAD_USER_IDS`: comma-separated Discord user IDs allowed to upload shop capes by DM
   - Enable the **Message Content Intent** in the Discord Developer Portal for the bot
   - `DB_FILE`: `/data/jems.sqlite`
5. Railway runs `npm start`. The service listens on Railway's `PORT`; health check is `/health`.

## Commands
- `/link code` — link Minecraft account; first link gives +200 Jems
- `/balance` — show Jems\n- `/daily` — claim 20 Jems once per UTC calendar day
- `/giftcode create` and `/giftcode redeem`
- `/jems give` and `/jems remove` — administrators
- `/jemdrop create`, `/jemdrop enter`, `/jemdrop cancel`

## API
- `GET /health`
- `POST /api/link/start` — creates a 10-minute linking code
- `GET /api/balance/:uuid`
- `GET /api/shop/items` — public active shop items and cape image URLs
- `POST /api/shop/purchase` — atomically charges a linked account and sends a Discord DM receipt; requires `x-rovian-secret`

## Upload capes by DM
1. Add your Discord user ID to `SHOP_UPLOAD_USER_IDS` in Railway Variables (comma-separated for multiple staff).
2. Enable Message Content Intent for the bot in the Discord Developer Portal.
3. DM the bot one PNG, JPG, or WEBP image (max 8 MB) with: `Cape "Fire Wings", 500 Jems`.
4. The bot adds it to the catalog; the Rovian launcher loads cape items from `/api/shop/items`.

Uploads must be sent as a DM attachment. The image URL is the Discord attachment CDN URL.
Private API requests require the `x-rovian-secret` header.

## Security
Development starter only. Never put the bot token or API secret in the Minecraft JAR or public client. The link endpoint must only be called by a trusted backend that has verified the player's Microsoft/Minecraft identity. Use HTTPS and production storage before opening this to public players.
