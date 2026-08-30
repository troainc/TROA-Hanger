# TROA-Hangar

**TROA-Hangar** is a server-side Torch plugin for Space Engineers. It gives players a safe grid hangar and marketplace without requiring a client mod or external Discord bot. Optional private Discord confirmations can use a configured bot token.

> **Release:** `v2.0.0-alpha.4.39`
> **SHA-256:** `B8EC66462757C4BB6C188F9A651E7DF58BC4E117D74CD6371C36E93980E0FA58`
> **Platform:** Torch / .NET Framework 4.8  
> **Hosting:** Windows, Linux, and AMP/Wine-hosted Space Engineers servers

This public repository contains release documentation, the configuration example, the QC migration tool, and licensing information. The plugin source remains private to TROA, and the current plugin ZIP is distributed separately through TROA-approved release channels. This repository contains no source code, server files, player data, tokens, or live webhook URLs.

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
- Persistent local market auditing in `TROA-HangerMarketAudit.log`; the audit-webhook settings are reserved and do not send Discord audit embeds in `.4.39`.
- Path-safe behavior for Windows and Linux/AMP/Wine installations.

## Installation

1. Obtain the approved `TROA-Hangar-v2.0.0-alpha.4.39-command-spelling.zip` release. Verify its SHA-256 against the value at the top of this README.
2. Install the ZIP through Torch's plugin installer. Do not unzip it into the Space Engineers client.
3. Restart Torch or reload plugins using your normal server workflow.
4. TROA-Hangar creates `TROA-Hanger.cfg` on first start.
5. Keep a private backup of the generated config and `TROA-HangerData` before upgrades.
6. In the **in-game chat**, run `!hangaradmin status` as an administrator to confirm the plugin is ready.

### Upgrade Safely

- Back up the existing config and storage root before replacing the ZIP.
- Keep your live config private: it may include Discord webhook URLs.
- New configuration settings default safely when missing; valid existing settings are retained.
- If a config reload reports XML errors, correct the named tag and try again. A failed reload does not overwrite the current valid config.

## Quick Test

1. Join as a player who is the major owner of a grid.
2. Look directly at the grid within the configured distance.
3. Run `!hangar store MyShip`.
4. Run `!hangar list` and note its number.
5. Move to a clear location, then run `!hangar load <number>`.
6. For a market test, look at another owned grid and run:  
   `!hangar sell 100000 Fighter live 0 "Combat-ready ship"`
7. A second player can use `!hangar buy <market-id>` on a live listing, or `!hangar bid <market-id> <price>` on a timed listing.
8. The buyer moves to a clear area and runs `!hangar claim <claim-code>`.

## Player Commands

Type these in **Space Engineers in-game chat**. Player commands do not require Torch administrator permission.

| Command | Description |
|---|---|
| `!hangar help` | Shows player command help. |
| `!hangar helper` | Shows a short storage workflow guide. |
| `!hangar status` | Shows active storage and market availability. |
| `!hangar store <name>` | Stores the grid you are looking at. You must be a major owner. |
| `!hangar list` | Lists your stored TROA grids. |
| `!hangar load <number>` | Retrieves a stored grid near your current clear location. |
| `!hangar claim <claim-code>` | Deploys a grid purchased from the market. |
| `!hangar clean` | Repairs your storage catalog and safely quarantines unreadable files. |
| `!hangar sell <price> <type> <live|timed> <minutes> <description>` | Stores the viewed grid and lists it in one step. |
| `!hangar market` | Alias for `!hangar market list`. |
| `!hangar market list` | Shows active offers. |
| `!hangar market offer <grid-number> <price>` | Lists one of your existing tracked grids; it starts with the default live settings. |
| `!hangar market details <market-id> <type> <live|timed> <minutes> <description>` | Updates your listing's class, bid mode, duration, and description. |
| `!hangar bid <market-id> <price>` | Places a bid. |
| `!hangar market bid <market-id> <price>` | Alias for `!hangar bid`. |
| `!hangar buy <market-id>` | Buys an active live offer at its buyer total. |
| `!hangar market buy <market-id>` | Alias for `!hangar buy`. |
| `!hangar market cancel <market-id>` | Cancels your offer and returns the grid to your hangar. |
| `!hangar keen list` | Lists optional Keen Grid Storage grids. |
| `!hangar keen store <name>` | Stores the viewed grid in Keen Grid Storage, when enabled. |
| `!hangar keen retrieve <number>` | Retrieves an optional Keen grid at the bound terminal. |
| `!factionhangar help` | Shows faction-hangar help. |
| `!factionhangar store <name>` | Stores a faction-owned grid you are looking at. |
| `!factionhangar list` | Lists faction-stored grids. |
| `!factionhangar load <number>` | Retrieves a faction-stored grid. |

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
| `!hangaradmin help` / `!hangaradmin helper` | Shows the complete server-owner help and workflow. |
| `!hangaradmin status` | Shows storage, market, economy, and Discord integration status. |
| `!hangaradmin reload` | Validates and reloads `TROA-Hanger.cfg`. |
| `!hangar storeid <entity-id> <name>` | Troubleshooting command that stores an owned grid by entity ID. |
| `!hangar storage` | Shows the resolved player, faction, and market storage folders. |
| `!hangaradmin terminalhere` | Binds the nearby Keen Services Terminal. |
| `!hangaradmin terminal <entity-id>` | Binds a Keen Services Terminal by entity ID. |
| `!hangaradmin keen <true|false>` | Enables or disables optional Keen Grid Storage commands. |
| `!hangaradmin recover <steam-id>` | Rebuilds one player's catalog from `PlayersHangers`. |
| `!hangaradmin recoverall` | Rebuilds all player catalog records after a crash or catalog issue. |
| `!hangaradmin cleanhangar <steam-id>` | Repairs one player catalog and quarantines unreadable files. |
| `!hangaradmin marketrecover` | Returns recoverable orphaned market files to their sellers. |
| `!hangaradmin player <steam-id>` | Views player hangar information. |
| `!hangaradmin offers` | Reviews current market offers. |
| `!hangaradmin removeoffer <market-id>` | Removes an offer safely. |
| `!hangaradmin keenlist <steam-id>` | Shows current Keen Grid Storage IDs. |
| `!hangaradmin keenattach <steam-id> <keen-grid-id>` | Attaches a current Keen grid to a player record. |
| `!hangaradmin troastorage <true|false>` | Enables or disables TROA storage. |
| `!hangaradmin market <true|false>` | Enables or disables the market. |
| `!hangaradmin economy <true|false>` | Enables or disables credit transfers. |
| `!hangaradmin limit <count>` | Sets player grid limit. |
| `!hangaradmin minimumprice <credits>` | Sets the lowest allowed market listing price. |
| `!hangaradmin listingfee <true|false> <credits>` | Configures optional listing fees. |
| `!hangaradmin bidminimum <minutes>` | Sets the minimum timed-auction duration within the configured maximum. |
| `!hangaradmin webhook status` | Shows standalone market webhook status. |
| `!hangaradmin webhook test` | Sends a test market embed to the configured market webhook. |

## Configuration

Use `TROA-Hangar.cfg.example` as the setup reference. Copy only the settings you want into the config generated by the plugin. Do not publish a live config.

> **Compatibility note:** the current plugin retains the legacy on-disk names `TROA-Hanger.cfg`, `TROA-HangerData`, and `MarketHangers` so existing installations and upgrades continue to work.

| Setting | Purpose |
|---|---|
| `Enabled` | Master plugin enable switch. |
| `StorageRootDirectory` | Optional storage location; blank uses the default `TROA-HangerData` folder. |
| `EnableTroaStorage` | Enables normal Steam-ID-based TROA file storage. |
| `EnableCrossServerStorage` | Reserved for future support; keep `false` in this release. |
| `EnableKeenGridStorage` / `KeenGridStorageTerminalEntityId` | Enables optional Keen storage and identifies its bound Services Terminal. |
| `MaxPlayerGrids` | Maximum stored player grids; `0` means unlimited. |
| `LookTargetDistanceMeters` | Maximum look-target distance used by store and sell commands. |
| `MinimumGridBlocks` | Minimum blocks required before a grid can be stored. |
| `MaximumBlocksPerGrid` / `MaximumPcuPerGrid` | Optional caps; `0` means unlimited. |
| `AllowSmallGrids` / `AllowLargeGrids` / `AllowStaticGrids` | Controls allowed grid sizes and whether stations can be stored. |
| `EnableMarket` | Enables player listings, bids, and purchases. |
| `MarketCommandCooldownSeconds` | Delay between market commands for each player. |
| `MaxMarketOffersPerPlayer` | Active offer limit; `0` means unlimited. |
| `MinimumMarketListingPrice` | Lowest valid listing price. |
| `ChargeMarketListingFee` / `MarketListingFeeCredits` | Optional listing fee, collected through the native economy. |
| `EnableEconomyTransactions` | Enables credit transfers and purchases; listings and bids can remain available when disabled. |
| `LiveBidDurationMinutes` | Lifetime of live buy-now listings; `0` means no live expiry. |
| `DefaultTimedBidDurationMinutes` | Duration used when a timed seller enters `0`. |
| `MinimumTimedBidDurationMinutes` / `MaximumTimedBidDurationMinutes` | Allowed seller-selected timed-auction range. |
| `EnablePeakHourMarketPricing` | Enables buyer-only peak-hour surcharge. |
| `PeakHourStart` / `PeakHourEnd` | Peak window in whole hours, including start and excluding end. |
| `PeakHourPriceIncreasePercent` | Percentage added to the buyer total during peak hours. |
| `PeakHourTimeZoneId` | Blank uses server local time; otherwise supply the host's timezone ID. |
| `PeakHourRevenueFactionTag` | Revenue faction; it is created as an NPC faction if missing. |
| `EnableDiscordMarketWebhook` | Enables player-facing standalone Discord market cards. |
| `DiscordMarketWebhookUrl` | Full Discord webhook URL for the market channel. |
| `DiscordMarketWebhookName` | Display name used by player-facing market webhook posts. |
| `EnableMarketInGameConfirmations` | Sends private in-game confirmations from TROA Market Exchange. Enabled by default. |
| `EnableDiscordMarketDirectMessages` | Enables optional Discord DM confirmation embeds. |
| `DiscordMarketBotToken` | Bot token used only for optional private Discord messages. Keep it private. |
| `DiscordMarketPlayerMappings` | Maps players with `SteamID:DiscordUserID` entries. |
| `EnableMarketAuditWebhook` / `MarketAuditWebhookUrl` | Reserved configuration fields; `.4.39` records local audits but does not deliver Discord audit embeds. |
| `ChargeForStorage` / `StorageFeeCredits` | Reserved for future use; no storage fee is charged in `.4.39`. |
| `ChargeForRetrieval` / `RetrievalFeeCredits` | Reserved for future use; no retrieval fee is charged in `.4.39`. |

### Discord Webhooks

The **market webhook** sends player-facing listings, bids, sales, cancellations, and card updates. `EnableMarketAuditWebhook` and `MarketAuditWebhookUrl` are reserved in `.4.39`; market audit events are written locally to `TROA-HangerMarketAudit.log` and are not sent to Discord by this build.

Paste the complete Discord webhook URL, **not only a webhook ID**. After changing the config, run `!hangaradmin reload`, then `!hangaradmin webhook status`. Run `!hangaradmin webhook test` to validate the player-facing market channel.

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
  TROA-HangerMarketAudit.log
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

A Steam ID64 belongs to the player’s Steam account. It does not change when the server world is wiped, updated, restarted, or moved to another machine. When `TROA-HangerData` is retained or restored from backup, server owners can rebuild the player's hangar catalog with `!hangaradmin recover <steam-id>` or `!hangaradmin recoverall` instead of manually recreating player records.

- Direct TROA storage is designed for Steam-ID-based recovery after catalog issues, crashes, updates, or world wipes.
- Market files retain seller Steam-ID information so orphaned listings can be returned using `!hangaradmin marketrecover`.
- Unreadable grid files are quarantined for review rather than deleted.
- Keen Grid Storage IDs are world-specific and can change after a wipe; use TROA direct storage for wipe-resistant player hangar files.
- Steam-ID storage improves recovery, but it does not replace backups. Back up `TROA-HangerData` before wipes, major updates, migrations, or storage-path changes.

### Migrating from QC

Use the standalone [QC-to-TROA-Hangar Migrator](QC-to-TROA-Hangar-Migrator-v1.0.0.zip). It previews by default, never moves or deletes Quantum Hangar files, copies eligible player grid files only when `--copy` is supplied, creates compatible TROA catalog records, backs up the existing catalog, and writes a migration report. Files without a clear Steam ID are skipped until the server owner supplies an explicit player-to-Steam-ID mapping.


## No UI Required

TROA-Hangar is intentionally **configuration- and command-based**. It does not require a client mod, custom in-game UI, web panel, or separate terminal screen.

- **Server owners:** Manage limits, market rules, economy settings, Discord webhooks, and storage paths through the legacy-compatible `TROA-Hanger.cfg`; use `!hangaradmin` commands for live administration and recovery.
- **Players:** Use `!hangar`, `!factionhangar`, and market commands directly in Space Engineers in-game chat.
- **Why this helps:** No UI dependencies or client installation, fewer post-update compatibility issues, and simpler support across Windows, Linux, AMP, and Wine servers.

This streamlined command-and-config approach keeps TROA-Hangar portable and easy for server administrators to maintain.

