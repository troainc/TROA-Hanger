# TROA-Hangar Changelog

## v2.0.0-alpha.4.39 - Command Spelling Correction

- Changes player commands from `!hanger` to the correctly spelled `!hangar`.
- Changes administrator commands from `!hangeradmin` to `!hangaradmin`.
- Changes faction commands from `!factionhanger` to `!factionhangar`.
- Renames `!hangaradmin cleanhanger` to `!hangaradmin cleanhangar`.
- Updates all README instructions and configuration tips to match the working in-game commands.
- Keeps legacy plugin filenames and storage folders unchanged so existing server data upgrades safely.

## v2.0.0-alpha.4.38 - Market Lifecycle

- Allows players to buy active live listings until their configured deadline.
- Keeps timed listings as auctions that settle the highest valid bid when time expires.
- Adds Discord-native relative countdowns to both live and timed market cards.
- Closes expired live listings and returns unsold ships to the seller's Steam-ID hangar.
- Returns timed listings when no valid sale can be completed.
- Runs expiry settlement on Torch's game thread and prevents duplicate timer processing.
- Sends private in-game confirmations from **TROA Market Exchange** to sellers, buyers, and bidders.
- Adds optional Discord DM confirmation embeds using a bot token and `SteamID:DiscordUserID` mappings.
- Refreshes existing active market cards once after startup so they receive the new countdown format.
