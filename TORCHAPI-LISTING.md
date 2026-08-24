# TROA-Hanger — Grid Storage & Market Exchange

## Short Description

TROA-Hanger is a server-side Torch plugin for Space Engineers that provides player grid storage, faction hangars, a live/timed ship market, optional economy support, safe deployment, recovery tools, and standalone Discord market embeds — without requiring a client mod or TROA Discord Monitor.

**Current release:** `v2.0.0-alpha.4.37`  
**Requirements:** Torch on .NET Framework 4.8  
**Server support:** Windows, Linux, AMP, and Wine-hosted servers

---

## Overview

TROA-Hanger gives server owners a practical way to manage player-owned grids while keeping ships recoverable and market transactions visible. Players can store the grid they are looking at, list it for sale, bid or buy from the market, and claim purchased ships using a short code.

The plugin uses Steam-ID based storage folders, helping server owners recover player hangars after a catalog issue, crash, or wipe-related cleanup. It also supports optional Keen Grid Storage terminal integration, but TROA Storage works fully on its own.

## Main Features

- **Player Storage:** Players look directly at a major-owned grid and store it under their own hangar.
- **Safe Retrieval:** Stored and purchased ships are deployed at a nearby clear location instead of directly on a player or another entity.
- **Faction Hangars:** Store and retrieve faction-owned grids for faction use.
- **Market Exchange:** List, bid on, buy, cancel, and claim ships with five-character market IDs and four-character claim codes.
- **Live and Timed Bids:** Live listings use server timing; timed listings use a seller-selected duration within server-owner limits.
- **Market Custody:** Listed ships move to protected market storage until they are cancelled or bought.
- **Economy Support:** Optional native Space Engineers economy transfers, optional listing fees, minimum prices, and peak-hour buyer surcharge.
- **Recovery Tools:** Rebuild player records from Steam-ID storage folders and recover orphaned market files.
- **Safe Cleanup:** Unreadable `.sbc` files are quarantined instead of deleted.
- **Keen Storage Optional:** Bind a Keen Services Terminal if wanted. Direct TROA Storage does not require Keen storage.
- **Standalone Discord Market Cards:** Optional webhook embeds for listings, bids, sales, cancellations, and updates without requiring TROA Discord Monitor.
- **Admin Audit Setup:** Dedicated Market Audit webhook settings are included for private server-owner configuration.

## Installation

1. Download `TROA-Hanger-v2.0.0-alpha.4.37-market-audit-config.zip`.
2. Install the ZIP using Torch's plugin installer.
3. Restart Torch or reload plugins using your normal server workflow.
4. TROA-Hanger creates `TROA-Hanger.cfg` on first startup.
5. Run `!hangeradmin status` in Space Engineers in-game chat to confirm the plugin is ready.
6. Configure optional market and Discord settings in `TROA-Hanger.cfg`, then run `!hangeradmin reload`.

> Keep `TROA-Hanger.cfg` private. Do not publish webhook URLs, account tokens, private storage paths, or player data.

## Player Commands

All player commands are typed in **Space Engineers in-game chat**.

```text
!hanger help
!hanger helper
!hanger status
!hanger store <name>
!hanger list
!hanger load <number>
!hanger claim <claim-code>
!hanger clean

!hanger sell <price> <type> <live|timed> <minutes> <description>
!hanger market list
!hanger bid <market-id> <price>
!hanger buy <market-id>
!hanger market cancel <market-id>

!factionhanger help
!factionhanger store <name>
!factionhanger list
!factionhanger load <number>
```

## Server Owner Commands

Server-owner commands require Torch administrator permission and are also entered in **Space Engineers in-game chat**.

```text
!hangeradmin help
!hangeradmin status
!hangeradmin reload
!hangeradmin terminalhere
!hangeradmin terminal <entity-id>

!hangeradmin recover <steam-id>
!hangeradmin recoverall
!hangeradmin cleanhanger <steam-id>
!hangeradmin marketrecover
!hangeradmin player <steam-id>
!hangeradmin offers
!hangeradmin removeoffer <market-id>

!hangeradmin troastorage <true|false>
!hangeradmin market <true|false>
!hangeradmin economy <true|false>
!hangeradmin limit <count>
!hangeradmin minimumprice <credits>
!hangeradmin listingfee <true|false> <credits>
!hangeradmin webhook status
!hangeradmin webhook test
```

## Important Configuration

The release includes `TROA-Hanger.cfg.example` with comments for each setting. Common settings include:

- Player grid limits, minimum block count, PCU limits, and look distance.
- Market enablement, minimum price, listing fee, player offer cap, and cooldown.
- Live and timed bid duration limits.
- Native economy transfers and peak-hour surcharge settings.
- Public market Discord webhook URL.
- Private Market Audit webhook configuration.
- Optional Keen Grid Storage Services Terminal binding.

## Backup and Recovery

Back up your world, `TROA-Hanger.cfg`, and `TROA-HangerData` before upgrading or using it in production. Player hangars are organized by Steam ID. If a catalog record is lost, use `!hangeradmin recover <steam-id>` or `!hangeradmin recoverall`. Unreadable grid files are placed in a `Quarantine` folder rather than deleted.

### QC / Quantum Hangar Migration and Wipe Safety

TROA-Hanger uses a different storage model from legacy QC / Quantum Hangar. Player grids are placed in `PlayersHangers/<SteamID64>/`, with a compatible TROA catalog record beside the storage folders. Steam ID64 belongs to the player's Steam account and does not change when a world is wiped, updated, restarted, or moved.

This means direct TROA-Hanger grids remain recoverable after a crash, catalog problem, update, or world wipe **when the `TROA-HangerData` folder is retained or restored from backup**. Server owners can rebuild player records with `!hangeradmin recover <steam-id>` or `!hangeradmin recoverall` instead of manually recreating a player's hanger list.

Keen Grid Storage IDs are world-specific and may change after a wipe. Use TROA direct storage for Steam-ID-based player grid files; use the Keen attach tools only for current-world Keen storage. Always back up `TROA-HangerData` before wipes, major updates, or server moves.

## Alpha Status

TROA-Hanger is currently an active alpha release. Test updates on a development server first and keep current backups. Cross-server transfers, alliance escrow, automated cleanup, and in-game market-block projections are not included in this release.

## License and Use

TROA-Hanger is shared for **server use only**. Server owners may install and configure the supplied, unmodified plugin package on servers they operate. Modification, reverse engineering, repackaging, redistribution of altered builds, removal of TROA branding, or claiming the plugin as your own is not allowed.

All changes, enhancements, integrations, and public releases must be reviewed and approved by TROA before distribution.

---

**Project page:** https://github.com/troainc/TROA-Hanger  
**Release package:** `TROA-Hanger-v2.0.0-alpha.4.37-market-audit-config.zip`
