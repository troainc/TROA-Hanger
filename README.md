# Hangar+

**Hangar+** (repository and compatibility name: TROA-Hangar) is a server-side Torch plugin for Space Engineers. It gives players a safe grid hangar and marketplace without requiring a client mod or external Discord bot. Optional private Discord confirmations can use a configured bot token.

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
- Community-neutral **Hangar+** in-game branding by default, configurable with `!hangaradmin name <display-name>`.
- Private in-game confirmations from the configured Hangar+ market identity, with optional Discord DM embeds.
- Persistent local market auditing in `TROA-HangerMarketAudit.log`; the audit-webhook settings are reserved and do not send Discord audit embeds in `.4.39`.
- Path-safe behavior for Windows and Linux/AMP/Wine installations.
- Durable transaction journals and explicit market states for recovery-safe settlement.
- In-game LCD and trade-station feeds using existing text-surface blocks, with no client UI.
- Market search, filtering, categories, station markets, and access-controlled Blackmarket listings.
- Optional Nexus v3 discovery, read-only remote catalogs, and locked cross-server purchasing.
- Physical commodity sell custody, escrowed buy orders, claimable vaults, reputation, and analytics.
- Discord webhook coverage for grid-market, Blackmarket, commodity, and cross-server lifecycle events.

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
| `!hangar market search <query> <category> <station> <page>` | Searches and filters public station listings. |
| `!hangar market classify <market-id> <category> <station>` | Assigns a category and station to your listing. |
| `!hangar market remotebuy <server-id> <market-id>` | Starts a durable Nexus cross-server reservation. |
| `!hangar market remotecommit <transaction-id>` | Escrows credits after the source server confirms a reservation. |
| `!blackmarket list` | Lists Blackmarket offers when the player has access. |
| `!blackmarket listoffer <market-id> <category>` | Moves one of your listings into the Blackmarket. |
| `!market commodity list <query> <category> <station> <page>` | Searches commodity sell listings and buy orders. |
| `!market commodity sell <definition-id> <quantity> <unit-price> <category> <station>` | Places physical items into durable sell custody. |
| `!market commodity buyorder <definition-id> <quantity> <unit-price> <category> <station>` | Opens a fully escrowed buy order. |
| `!market commodity fill <order-id> <quantity>` | Fills a commodity listing or buy order. |
| `!market commodity claim <definition-id> <quantity>` | Claims purchased items from the durable commodity vault. |
| `!market reputation` | Shows the player's market reputation. |
| `!market analytics` | Shows aggregate exchange activity. |
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


## Blackmarket setup and use

The Blackmarket is an optional, access-controlled part of the same market system. It is disabled by default.

### Server-owner setup

Add or update these settings in `TROA-Hanger.cfg`:

```xml
<EnableBlackmarket>true</EnableBlackmarket>
<BlackmarketListingFeeCredits>5000</BlackmarketListingFeeCredits>
<BlackmarketRevenueFactionTag>ADMIN</BlackmarketRevenueFactionTag>
<BlackmarketAccessSteamIds>
  <string>76561198000000001</string>
  <string>76561198000000002</string>
</BlackmarketAccessSteamIds>
```

Then run:

```text
!hangaradmin reload
```

Setting behavior:

- `EnableBlackmarket` turns Blackmarket commands and protected purchases on or off.
- `BlackmarketAccessSteamIds` is a Steam ID64 allowlist. An empty list allows every player; a populated list allows only those entries.
- `BlackmarketListingFeeCredits` is charged once when a normal listing is moved into the Blackmarket. Use `0` for no fee.
- `BlackmarketRevenueFactionTag` receives listing fees through the Space Engineers economy. If the fee cannot be collected or deposited, the listing is returned to the normal market instead of being left in a partial state.

### Player workflow

1. Store and list a grid normally:
   ```text
   !hangar market offer <grid-number> <price>
   ```
2. Move that listing into a Blackmarket category:
   ```text
   !blackmarket listoffer <market-id> <category>
   ```
   Example:
   ```text
   !blackmarket listoffer K7X4Q Restricted
   ```
3. Eligible players browse Blackmarket listings:
   ```text
   !blackmarket list
   ```
4. Eligible players bid on timed listings or buy live listings using the normal Market ID:
   ```text
   !hangar bid <market-id> <credits>
   !hangar buy <market-id>
   ```

Blackmarket access is checked again when bidding or purchasing, so knowing a Market ID does not bypass the allowlist.

### Privacy, Discord, LCDs, and auditing

- Player-facing Blackmarket lists and Discord posts identify the seller as **Anonymous**.
- Full seller Steam IDs, category, fee, action, and Market ID remain in the private local market audit for staff recovery and investigation.
- Blackmarket listings are excluded from the normal public market list, public search results, and Nexus public catalog broadcasts.
- When the market webhook is enabled, new Blackmarket listings generate an anonymous Discord embed using the existing `DiscordMarketWebhookName`.
- Name an LCD block `[HANGAR+ BLACKMARKET]`, or add `[Hangar+ Blackmarket Display]` to Custom Data, to show the Blackmarket feed.
- LCD visibility is physical rather than player-specific. Place Blackmarket LCDs inside secured areas if the listing feed should not be visible to everyone who can approach the screen.

## In-game LCD and trade-station setup

Hangar+ uses ordinary Space Engineers text-surface blocks. Players do not install a client mod, programmable-block script, or custom UI.

### Quick setup

1. Confirm `EnableMarketLcdDisplays` is `true` in `TROA-Hanger.cfg`.
2. Run `!hangaradmin reload` after changing the configuration.
3. Place an LCD panel or another block that provides a text surface.
4. Rename the block with one of these tags:
   - `[HANGAR+ MARKET]` — public ship and grid listings.
   - `[HANGAR+ BLACKMARKET]` — Blackmarket listings.
   - `[HANGAR+ COMMODITY]` — commodity sell listings and buy orders.
5. Wait for the configured refresh interval. Hangar+ automatically changes the surface to text-and-image mode and writes the matching market feed.

Examples:

```text
Trade Station [HANGAR+ MARKET]
Restricted Exchange [HANGAR+ BLACKMARKET]
Ore Prices [HANGAR+ COMMODITY]
```

The tag can appear anywhere in the block's Custom Name. A matching block with multiple text surfaces receives the feed on every surface, so use a dedicated display block if you do not want its other surfaces replaced.

### Custom Data alternative

Instead of renaming the block, add one marker to its Custom Data:

```text
[Hangar+ Market Display]
```

```text
[Hangar+ Blackmarket Display]
```

```text
[Hangar+ Commodity Display]
```

For a normal market display, adding `Enabled=false` disables updates without removing the Custom Data marker.

### Branding and refresh settings

```xml
<InGameDisplayName>Hangar+</InGameDisplayName>
<EnableMarketLcdDisplays>true</EnableMarketLcdDisplays>
<MarketLcdNameTag>[HANGAR+ MARKET]</MarketLcdNameTag>
<MarketLcdRefreshSeconds>15</MarketLcdRefreshSeconds>
<MarketLcdRowsPerPage>10</MarketLcdRowsPerPage>
```

Use `!hangaradmin name <display-name>` to change the player-facing brand. For example, `!hangaradmin name Orion Exchange` changes the generated normal-market tag to `[ORION EXCHANGE MARKET]`; the corresponding Blackmarket and commodity tags become `[ORION EXCHANGE BLACKMARKET]` and `[ORION EXCHANGE COMMODITY]`.

If you edit `InGameDisplayName` directly, also update `MarketLcdNameTag` for the public market display. Discord is unaffected: it continues using `DiscordMarketWebhookName`.

LCD pages rotate automatically when listings exceed `MarketLcdRowsPerPage`. The refresh interval has a five-second minimum. Older TROA-prefixed LCD names remain recognized for upgrade compatibility.

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
| `!hangaradmin name <display-name>` | Changes player-facing chat, notification, and LCD branding without changing Discord. |

## Configuration

Use `TROA-Hangar.cfg.example` as the setup reference. Copy only the settings you want into the config generated by the plugin. Do not publish a live config.

> **Compatibility note:** the current plugin retains the legacy on-disk names `TROA-Hanger.cfg`, `TROA-HangerData`, and `MarketHangers` so existing installations and upgrades continue to work.

| Setting | Purpose |
|---|---|
| `Enabled` | Master plugin enable switch. |
| `InGameDisplayName` | Player-facing in-game name; defaults to `Hangar+`. It does not rename Discord webhooks. |
| `StorageRootDirectory` | Optional storage location; blank uses the default `TROA-HangerData` folder. |
| `EnableTroaStorage` | Enables normal Steam-ID-based TROA file storage. |
| `EnableCrossServerStorage` / `CrossServerSharedStorageDirectory` / `CrossServerLockTimeoutMinutes` | Enables shared Nexus custody, its shared root, and stale-lock recovery timeout. `StorageRootDirectory` must use the same shared location. |
| `EnableNexusIntegration` / `NexusMarketChannelId` / `NexusCatalogRefreshSeconds` | Enables Nexus v3 discovery, read-only catalog sync, and purchase messaging. |
| `EnableKeenGridStorage` / `KeenGridStorageTerminalEntityId` | Enables optional Keen storage and identifies its bound Services Terminal. |
| `MaxPlayerGrids` | Maximum stored player grids; `0` means unlimited. |
| `LookTargetDistanceMeters` | Maximum look-target distance used by store and sell commands. |
| `MinimumGridBlocks` | Minimum blocks required before a grid can be stored. |
| `MaximumBlocksPerGrid` / `MaximumPcuPerGrid` | Optional caps; `0` means unlimited. |
| `AllowSmallGrids` / `AllowLargeGrids` / `AllowStaticGrids` | Controls allowed grid sizes and whether stations can be stored. |
| `EnableMarket` | Enables player listings, bids, and purchases. |
| `EnableMarketLcdDisplays` / `MarketLcdNameTag` / `MarketLcdRefreshSeconds` / `MarketLcdRowsPerPage` | Configures server-driven LCD and trade-station feeds. |
| `EnableBlackmarket` / `BlackmarketListingFeeCredits` / `BlackmarketRevenueFactionTag` / `BlackmarketAccessSteamIds` | Controls Blackmarket access, fees, revenue, and eligibility. |
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
| `EnableMarketInGameConfirmations` | Sends private in-game confirmations using `InGameDisplayName`. Enabled by default. |
| `EnableDiscordMarketDirectMessages` | Enables optional Discord DM confirmation embeds. |
| `DiscordMarketBotToken` | Bot token used only for optional private Discord messages. Keep it private. |
| `DiscordMarketPlayerMappings` | Maps players with `SteamID:DiscordUserID` entries. |
| `EnableMarketAuditWebhook` / `MarketAuditWebhookUrl` | Reserved configuration fields; `.4.39` records local audits but does not deliver Discord audit embeds. |
| `ChargeForStorage` / `StorageFeeCredits` | Reserved for future use; no storage fee is charged in `.4.39`. |
| `ChargeForRetrieval` / `RetrievalFeeCredits` | Reserved for future use; no retrieval fee is charged in `.4.39`. |

### Discord Webhooks

The **market webhook** sends player-facing grid listings, bids, sales, cancellations, Blackmarket listings, commodity activity, cross-server lifecycle events, and card updates. `DiscordMarketWebhookName` controls the Discord username, author, and footer independently of the in-game name. `EnableMarketAuditWebhook` and `MarketAuditWebhookUrl` are reserved in `.4.39`; market audit events are written locally to `TROA-HangerMarketAudit.log` and are not sent to Discord by this build.

Paste the complete Discord webhook URL, **not only a webhook ID**. After changing the config, run `!hangaradmin reload`, then `!hangaradmin webhook status`. Run `!hangaradmin webhook test` to validate the player-facing market channel.

### Private Market Confirmations

Private in-game confirmations are enabled by default and identify the sender using the configured `InGameDisplayName` (default: **Hangar+ Market Exchange**). Buyers, bidders, and sellers receive confirmations for listings, bids, purchases, auction results, cancellations, and expired returns.

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

This is active alpha development. Test it on a development server and back up your world, plugin config, and `TROA-HangerData` before production deployment. Nexus cross-server purchasing and in-game text-surface market feeds are now implemented; alliance market escrow and automated cleanup remain outside the current scope.

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

