# TROA-Hangar

**TROA-Hangar** is a server-side Torch plugin for Space Engineers. It gives players a safe grid hangar and marketplace without requiring a client mod or external Discord bot. Optional private Discord confirmations can use a configured bot token.

> **Release:** `v2.0.0-alpha.4.38`
> **Platform:** Torch / .NET Framework 4.8  
> **Hosting:** Windows, Linux, and AMP/Wine-hosted Space Engineers servers

This public repository contains **only** the installable plugin ZIP, a configuration example, and documentation. It contains no source code, server files, player data, tokens, or live webhook URLs. The source project remains private to TROA.

## License and Use

TROA-Hangar is shared for **server use only**. You may install and configure the supplied, unmodified release ZIP on servers you operate. You may not modify, reverse engineer, repackage, redistribute altered builds, or claim it as your own. All fixes, enhancements, integrations, and public releases require TROA approval before distribution.

Read the complete terms in [LICENSE.md](LICENSE.md).

## Features

- Player-owned grid storage: look at a grid you major-own and store it.
- Safe retrieval near the player at a clear location.
- Faction grid storage for faction-owned ships.
- Buy-now live listings, timed auctions, automatic settlement, and short claim codes.
- Market custody: listed grid files move into `MarketHangers` until cancelled or purchased.
- Expired unsold listings return automatically to the seller's Steam-ID hangar.
- Configurable block, PCU, ownership, grid-size, distance, storage, and market limits.
- Optional Space Engineers economy transfers and peak-hour buyer surcharge.
- Steam-ID based storage layout and recovery tools for catalog or crash recovery.
- Optional Keen Grid Storage Services Terminal support; TROA storage works without it.
- Standalone Discord market embeds; TROA Discord Monitor is not required.
- Discord-native live countdowns on both live and timed market cards.
- Private in-game confirmations from **TROA Market Exchange**, with optional Discord DM embeds.
- Separate private Market Audit webhook configuration for server-owner auditing.
- Path-safe behavior for Windows and Linux/AMP/Wine installations.

## Installation

1. Download `TROA-Hangar-v2.0.0-alpha.4.38-market-lifecycle.zip`.
2. Install the ZIP through Torch's plugin installer. Do not unzip it into the Space Engineers client.
3. Restart Torch or reload plugins using your normal server workflow.
4. TROA-Hangar creates `TROA-Hanger.cfg` on first start.
5. Keep a private backup of the generated config and `TROA-HangerData` before upgrades.
6. In the **in-game chat**, run `!hangeradmin status` as an administrator to confirm the plugin is ready.

### Upgrade Safely

- Back up the existing config and storage root before replacing the ZIP.
- Keep your live config private: it may include Discord webhook URLs.
- New configuration settings default safely when missing; valid existing settings are retained.
- If a config reload reports XML errors, correct the named tag and try again. A failed reload does not overwrite the current valid config.

## Quick Test

1. Join as a player who is the major owner of a grid.
2. Look directly at the grid within the configured distance.
3. Run `!hanger store MyShip`.
4. Run `!hanger list` and note its number.
5. Move to a clear location, then run `!hanger load <number>`.
6. For a market test, look at another owned grid and run:  
   `!hanger sell 100000 Fighter live 0 "Combat-ready ship"`
7. A second player can use `!hanger buy <market-id>` on a live listing, or `!hanger bid <market-id> <price>` on a timed listing.
8. The buyer moves to a clear area and runs `!hanger claim <claim-code>`.

## Player Commands

Type these in **Space Engineers in-game chat**. Player commands do not require Torch administrator permission.

| Command | Description |
|---|---|
| `!hanger help` | Shows player command help. |
| `!hanger helper` | Shows a short storage workflow guide. |
| `!hanger status` | Shows active storage and market availability. |
| `!hanger store <name>` | Stores the grid you are looking at. You must be a major owner. |
| `!hanger list` | Lists your stored TROA grids. |
| `!hanger load <number>` | Retrieves a stored grid near your current clear location. |
| `!hanger claim <claim-code>` | Deploys a grid purchased from the market. |
| `!hanger clean` | Repairs your storage catalog and safely quarantines unreadable files. |
| `!hanger sell <price> <type> <live|timed> <minutes> <description>` | Stores the viewed grid and lists it in one step. |
| `!hanger market list` | Shows active offers. |
| `!hanger bid <market-id> <price>` | Places a bid. |
| `!hanger buy <market-id>` | Buys an active live offer at its buyer total. |
| `!hanger market cancel <market-id>` | Cancels your offer and returns the grid to your hangar. |
| `!hanger keen list` | Lists optional Keen Grid Storage grids. |
| `!hanger keen store <name>` | Stores the viewed grid in Keen Grid Storage, when enabled. |
| `!hanger keen retrieve <number>` | Retrieves an optional Keen grid at the bound terminal. |
| `!factionhanger help` | Shows faction-hangar help. |
| `!factionhanger store <name>` | Stores a faction-owned grid you are looking at. |
| `!factionhanger list` | Lists faction-stored grids. |
| `!factionhanger load <number>` | Retrieves a faction-stored grid. |

### Market Behavior

- Every listing has a five-character alphanumeric Market ID, such as `K7X4Q`.
- Use `live 0` for a buy-now live listing; the server uses `LiveBidDurationMinutes`.
- Use `timed <minutes>` for a timed auction within the owner-configured range. Use `0` for the server default duration.
- Live listings can be purchased until their deadline. Timed listings settle the highest valid bid when their timer expires.
- Unsold or failed listings return to the seller's Steam-ID hangar automatically.
- Market cards use green for live, blue for timed, and red for closed listings. Discord's relative timestamp visibly counts down in each viewer's local time.
- When economy is enabled, sellers receive the listed price. Any configured peak surcharge is paid by the buyer and goes to the configured server-owner faction.
- A listed grid stays in market custody until it is cancelled, purchased, settled, or returned after expiry.

## Server Owner Commands

These commands require Torch administrator permission and are entered in **Space Engineers in-game chat**.

| Command | Description |
|---|---|
| `!hangeradmin help` / `!hangeradmin helper` | Shows the complete server-owner help and workflow. |
| `!hangeradmin status` | Shows storage, market, economy, and Discord integration status. |
| `!hangeradmin reload` | Validates and reloads `TROA-Hanger.cfg`. |
| `!hangeradmin terminalhere` | Binds the nearby Keen Services Terminal. |
| `!hangeradmin terminal <entity-id>` | Binds a Keen Services Terminal by entity ID. |
| `!hangeradmin recover <steam-id>` | Rebuilds one player's catalog from `PlayersHangers`. |
| `!hangeradmin recoverall` | Rebuilds all player catalog records after a crash or catalog issue. |
| `!hangeradmin cleanhanger <steam-id>` | Repairs one player catalog and quarantines unreadable files. |
| `!hangeradmin marketrecover` | Returns recoverable orphaned market files to their sellers. |
| `!hangeradmin player <steam-id>` | Views player hangar information. |
| `!hangeradmin offers` | Reviews current market offers. |
| `!hangeradmin removeoffer <market-id>` | Removes an offer safely. |
| `!hangeradmin keenlist <steam-id>` | Shows current Keen Grid Storage IDs. |
| `!hangeradmin keenattach <steam-id> <keen-grid-id>` | Attaches a current Keen grid to a player record. |
| `!hangeradmin troastorage <true|false>` | Enables or disables TROA storage. |
| `!hangeradmin market <true|false>` | Enables or disables the market. |
| `!hangeradmin economy <true|false>` | Enables or disables credit transfers. |
| `!hangeradmin limit <count>` | Sets player grid limit. |
| `!hangeradmin minimumprice <credits>` | Sets the lowest allowed market listing price. |
| `!hangeradmin listingfee <true|false> <credits>` | Configures optional listing fees. |
| `!hangeradmin webhook status` | Shows standalone market webhook status. |
| `!hangeradmin webhook test` | Sends a test market embed to the configured market webhook. |

## Configuration

Use `TROA-Hangar.cfg.example` as the setup reference. Copy only the settings you want into the config generated by the plugin. Do not publish a live config.

> **Compatibility note:** the current plugin retains the legacy on-disk names `TROA-Hanger.cfg`, `TROA-HangerData`, and `MarketHangers` so existing installations and upgrades continue to work.

| Setting | Purpose |
|---|---|
| `StorageRootDirectory` | Optional storage location; blank uses the default data folder. |
| `MaxPlayerGrids` | Maximum stored player grids; `0` means unlimited. |
| `MinimumGridBlocks` | Minimum blocks required before a grid can be stored. |
| `MaximumBlocksPerGrid` / `MaximumPcuPerGrid` | Optional caps; `0` means unlimited. |
| `EnableMarket` | Enables player listings, bids, and purchases. |
| `EnableEconomyTransactions` | Enables real economy transfers. |
| `MinimumTimedBidDurationMinutes` | Lowest allowed timed auction. Set `5` to allow five-minute auctions. |
| `MaximumTimedBidDurationMinutes` | Highest allowed timed auction. |
| `EnablePeakHourMarketPricing` | Enables buyer-only peak-hour surcharge. |
| `PeakHourRevenueFactionTag` | Revenue faction; it is created as an NPC faction if missing. |
| `EnableDiscordMarketWebhook` | Enables player-facing standalone Discord market cards. |
| `DiscordMarketWebhookUrl` | Full Discord webhook URL for the market channel. |
| `EnableMarketInGameConfirmations` | Sends private in-game confirmations from TROA Market Exchange. Enabled by default. |
| `EnableDiscordMarketDirectMessages` | Enables optional Discord DM confirmation embeds. |
| `DiscordMarketBotToken` | Bot token used only for optional private Discord messages. Keep it private. |
| `DiscordMarketPlayerMappings` | Maps players with `SteamID:DiscordUserID` entries. |
| `EnableMarketAuditWebhook` | Enables the private Market Audit webhook section. |
| `MarketAuditWebhookUrl` | Full Discord webhook URL for the private admin audit channel. |

### Discord Webhooks

Use a separate webhook destination for each purpose:

- **Market webhook:** player-facing listings, bids, sales, cancellations, and card updates.
- **Market Audit webhook:** private server-owner channel. Treat this URL as confidential.

Paste the complete Discord webhook URL, **not only a webhook ID**. After changing the config, run `!hangeradmin reload`, then `!hangeradmin webhook status`. Run `!hangeradmin webhook test` to validate the player-facing market channel.

### Private Market Confirmations

Private in-game confirmations are enabled by default and identify the sender as **TROA Market Exchange**. Buyers, bidders, and sellers receive confirmations for listings, bids, purchases, auction results, cancellations, and expired returns.

Discord DM embeds are optional. A webhook cannot send private messages, so DMs require `EnableDiscordMarketDirectMessages`, `DiscordMarketBotToken`, and `DiscordMarketPlayerMappings`. Use one `SteamID:DiscordUserID` entry per player and never publish the bot token.

## Storage and Recovery

Default layout:

```text
TROA-HangerData/
  PlayersHangers/<SteamID>/
  FactionHangers/
  MarketHangers/
  MarketAudit.log
```

- Player storage uses Steam ID folders, helping recovery after catalog problems or world wipes.
- Keen Grid Storage IDs belong to the current world and can change after a wipe.
- Market filenames retain seller Steam-ID information so orphaned offers can be recovered.
- Cleanup does not delete unreadable `.sbc` files; it places them in that owner's `Quarantine` folder.

## Compatibility and Scope

- Targets Torch on .NET Framework 4.8.
- Supports Windows and Linux-hosted AMP/Wine server paths.
- Direct TROA storage does not need Keen Grid Storage.
- Keen Services Terminal integration is optional and must be bound by an administrator.
- TROA Discord Monitor is optional; standalone market embeds work without it.

## Alpha Notice

This is an active alpha release. Test it on a development server and back up your world, plugin config, and `TROA-HangerData` before production deployment. Cross-server transfers, alliance market escrow, automated cleanup, and in-game market-block projections are not included in this release.

## Support Checklist

Include these when reporting a problem:

1. Exact in-game command and response.
2. Relevant Torch log lines and server time.
3. Whether the player major-owns the target grid.
4. Market ID or claim code, if applicable.
5. A sanitized config with all webhook URLs removed.

Never share webhook URLs, account tokens, private server paths, or player data in public support channels.


## QC / Quantum Hangar Migration and Wipe Safety

TROA-Hangar is not a renamed Quantum Hangar install. It has its own storage layout, catalog, commands, and recovery process. The important difference is that direct TROA player grids are stored under the player’s stable Steam ID64:

`PlayersHangers/<SteamID64>/`

A Steam ID64 belongs to the player’s Steam account. It does not change when the server world is wiped, updated, restarted, or moved to another machine. When `TROA-HangerData` is retained or restored from backup, server owners can rebuild the player's hanger catalog with `!hangeradmin recover <steam-id>` or `!hangeradmin recoverall` instead of manually recreating player records.

- Direct TROA storage is designed for Steam-ID-based recovery after catalog issues, crashes, updates, or world wipes.
- Market files retain seller Steam-ID information so orphaned listings can be returned using `!hangeradmin marketrecover`.
- Unreadable grid files are quarantined for review rather than deleted.
- Keen Grid Storage IDs are world-specific and can change after a wipe; use TROA direct storage for wipe-resistant player hanger files.
- Steam-ID storage improves recovery, but it does not replace backups. Back up `TROA-HangerData` before wipes, major updates, migrations, or storage-path changes.

### Migrating from QC

Use the standalone [QC-to-TROA-Hangar Migrator](QC-to-TROA-Hangar-Migrator-v1.0.0.zip). It previews by default, never moves or deletes Quantum Hangar files, copies eligible player grid files only when `--copy` is supplied, creates compatible TROA catalog records, backs up the existing catalog, and writes a migration report. Files without a clear Steam ID are skipped until the server owner supplies an explicit player-to-Steam-ID mapping.


## No UI Required

TROA-Hangar is intentionally **configuration- and command-based**. It does not require a client mod, custom in-game UI, web panel, or separate terminal screen.

- **Server owners:** Manage limits, market rules, economy settings, Discord webhooks, and storage paths through the legacy-compatible `TROA-Hanger.cfg`; use `!hangeradmin` commands for live administration and recovery.
- **Players:** Use `!hanger`, `!factionhanger`, and market commands directly in Space Engineers in-game chat.
- **Why this helps:** No UI dependencies or client installation, fewer post-update compatibility issues, and simpler support across Windows, Linux, AMP, and Wine servers.

This streamlined command-and-config approach keeps TROA-Hangar portable and easy for server administrators to maintain.
