# Rovian Bot

Discord bot and Jems API for the Rovian Minecraft client.

## Railway setup
Set these variables:
- `DISCORD_TOKEN`, `CLIENT_ID`, `GUILD_ID`
- `API_SECRET` (keep private; never put it in the Minecraft JAR)
- `DROP_ROLE_IDS` (comma-separated role IDs allowed to schedule drops; administrators are also allowed)
- `SHOP_UPLOAD_USER_IDS` (comma-separated staff Discord user IDs allowed to upload shop images)
- `DB_FILE=/data/jems.sqlite` when using persistent storage
- Enable Message Content Intent in the Discord Developer Portal.

Use a Railway persistent volume mounted at `/data` or an external persistent database. Without persistent storage, SQLite balances and purchases can be lost when the service restarts.

## Commands
- `/link code` — link a Minecraft account; first successful link grants +200 Jems.
- `/balance` — check your balance.
- `/daily` — claim 20 Jems once per UTC day.
- `/giftcode create` and `/giftcode redeem` — create and redeem shareable codes.
- `/jems gift user amount` — transfer Jems to another linked Discord/Minecraft account.
- `/jems give uuid amount` and `/jems remove uuid amount` — administrator balance controls.
- `/jemdrop create amount winners min-delay max-delay` — staff schedule a drop that opens at a random time inside the selected delay range.
- `/jemdrop enter` — enter the currently open drop.
- `/jemdrop cancel` — cancel the latest scheduled/open drop.

## Cape uploads
A user ID listed in `SHOP_UPLOAD_USER_IDS` can DM the bot one PNG, JPG, or WEBP attachment (maximum 8 MB) with a caption such as `Cape "Fire Wings", 500 Jems`. This adds the image to the shop catalogue. The current Fabric client can retrieve shop item metadata, but it does **not** yet render or equip custom capes in-game.

## API
- `GET /health`
- `GET /api/shop/items`
- `GET /api/balance/:uuid`
- `POST /api/link/start`
- `POST /api/shop/purchase/request`

The Minecraft client uses the Railway public HTTPS domain. Purchases require confirmation from the linked Discord account.

## Security limitations
This is still a development build, not ready for public player accounts. The current public `/api/link/start` endpoint accepts a UUID and username supplied by the client; that does not prove ownership of the Minecraft account and can be spoofed. Before public launch, linking must be moved behind a trusted verification flow (for example, a server-side plugin on an online-mode server). Never ask players to paste Microsoft passwords, access tokens, or session tokens into Discord or the client.
