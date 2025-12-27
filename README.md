# Spicy Chicken Nuggets Bot

A comprehensive Discord bot for managing World of Warcraft raid teams, featuring BiS item tracking, loot council voting, raid satisfaction polling, wipe tracking, and roster management.

## Features

- 🔍 **BIS Item Lookup** - Check which specs need items as Best-in-Slot
- 🎲 **Loot Council System** - Democratic loot distribution with tiebreaker support
- 📊 **Satisfaction Polling** - Raid satisfaction surveys
- 💀 **Wipe Tracking** - Track raid wipes by boss, mechanic, and player
- 👥 **Roster Management** - Comprehensive raid roster with stats and Raider.IO integration

---

## Table of Contents

- [BIS Lookup Commands](#bis-lookup-commands)
- [Loot Council Commands](#loot-council-commands)
- [Polling Commands](#polling-commands)
- [Raid Tracking Commands](#raid-tracking-commands)
- [Roster Management Commands](#roster-management-commands)

---

## BIS Lookup Commands

### `!bis <item_name>`
Look up which specs need an item as BiS.

**Example:**
```
!bis House of Cards
```

**Response:** Displays an embed showing which specs consider this item BIS.

---

### `!biscount <item_name>`
Get a count of how many specs need an item as BIS, along with the full list.

**Example:**
```
!biscount House of Cards
```

**Response:** Shows the number of specs and lists all specs that need the item.

---

### `!allitems`
List all items in the BIS database.

⚠️ **Warning:** This command will spam the channel with ~228 item names! You'll be asked to confirm with ✅ or ❌ reactions before it sends.

**Use case:** Check spelling when an item lookup fails.

---

## Loot Council Commands

### `!loot <item_name> [role_filter]`
Start a loot distribution poll with loot council voting system.

**Parameters:**
- `item_name` - Name of the item to distribute
- `role_filter` - (Optional, but recommended) Filter eligible players: "all", "HEALER", "MDPS", "RDPS", "TANK" or combined like "RDPS-MDPS"

**Example:**
```
!loot "Melee Only Item" MDPS
!loot "Ranged Only Item" RDPS
!loot "Tank Only Item" TANK
!loot "Healer Only Item" HEALER
!loot "Ring of Power" all
```

**How it works:**
1. Bot DMs all eligible raid members (this is based on the `role_filter` applied to the command) with interest buttons
2. Players select their interest level: Pass, Mog, Offspec, Tier, Mainspec, or BiS
3. After 5 minutes, loot council members receive voting buttons
4. Council votes for a winner
5. If there's a tie, the Master Looter receives a DM to break it
6. Winner is announced with statistics

---

### `!loot_priority`
Show the loot priority list based on who has received the least loot and longest time since last loot.

**Response:** Top 15 players who should be prioritized for loot.

---

### `!loot_history [@player] [limit]`
View loot distribution history.

**Examples:**
```
!loot_history                  # Last 10 items awarded
!loot_history @PlayerName      # Player's loot history
!loot_history @PlayerName 20   # Last 20 items for player
```

---

### `!loot_stats [limit]`
View the loot leaderboard showing who has received the most items.

**Example:**
```
!loot_stats      # Top 10
!loot_stats 20   # Top 20
```

---

### `!loot_report [week|month]`
Generate a comprehensive loot distribution report.

**Examples:**
```
!loot_report week   # Weekly report
!loot_report month  # Monthly report
```

**Report includes:**
- Total items awarded
- Average interest level
- Top receivers
- Loot distribution by class and spec
- Most contested items
- Players who haven't received loot recently

---

## Polling Commands

### `!poll [question]`
Create a raid satisfaction poll and DM all raid members.

**Example:**
```
!poll How satisfied were you with this week's raids?
!poll                # Uses default question with current week
```

**Response Methods:**
Players can respond in three ways:
1. **Click reaction emojis** (1️⃣ 2️⃣ 3️⃣ 4️⃣ 5️⃣ ❌)
2. **Type poll ID + rating**: `poll_20241222_183045 4`
3. **Type poll ID + rating + comment**: `poll_20241222_183045 4 Great raid night!`

**Rating Scale:**
- 1️⃣ Very Unsatisfied
- 2️⃣ Unsatisfied
- 3️⃣ Neutral
- 4️⃣ Satisfied
- 5️⃣ Very Satisfied
- ❌ Did not Raid

**Duration:** 24 hours

---

### `!pollresults [poll_id]`
View poll results and statistics.

**Examples:**
```
!pollresults                           # Show all active polls
!pollresults poll_20241222_183045      # Show specific poll results
```

**Statistics shown:**
- Average satisfaction rating
- Total responses
- Participation count
- Rating breakdown with percentages
- Recent comments

---

### `!closepoll <poll_id>`
Close a poll early (creator or admin only).

**Example:**
```
!closepoll poll_20241222_183045
```

---

### `!closeall` (Admin Only)
Close all active polls at once.


---


## Raid Tracking Commands

### `!raid_start`
Start a new raid wipe tracking session.

**How it works:**
1. Creates a new tracking session
2. Displays boss selection buttons
3. Click a boss to see its mechanics
4. Click a mechanic when a wipe occurs
5. Select the player who failed
6. Wipe is recorded with full context

---

### `!raid_end`
End the current raid tracking session.

**Response:** Shows session summary with duration and total wipes tracked.

---

### `!raid_status`
Check the current raid session status and statistics.

---

### `!wipe_stats [filter]`
View wipe statistics with optional filters.

**Examples:**
```
!wipe_stats                          # General guild stats
!wipe_stats player:PlayerName        # Stats for specific player
!wipe_stats boss:Ulgrax              # Stats for specific boss
!wipe_stats mechanic:Swallowing      # Stats for specific mechanic
!wipe_stats session:20241222_180000  # Stats for specific session
```

**General Stats Include:**
- Top players with most wipes
- Hardest mechanics
- Boss difficulty rankings

**Player Stats Include:**
- Total wipes
- Most failed mechanics
- Boss breakdown

**Boss Stats Include:**
- Total wipes on boss
- Mechanic breakdown
- Players struggling most

**Mechanic Stats Include:**
- Total failures
- Players struggling
- Role breakdown
- Which bosses have this mechanic

**Session Stats Include:**
- Duration
- Total wipes
- Problem players/mechanics
- Boss breakdown

---

### `!bosses`
Display all available bosses and their trackable mechanics.

---


### `!raid_help`
Display comprehensive help for raid tracking commands.

---

## Roster Management Commands

### `!roster_add <name> <role> <main_spec> <realm> [class] [@discord_user] [ilvl]`
Add a new player to the raid roster.

**Parameters:**
- `name` - Player name (use quotes if it contains spaces)
- `role` - Tank, Healer, MDPS, or RDPS
- `main_spec` - Player's main specialization
- `realm` - WoW realm name (e.g., "Area-52")
- `class` - (Optional) Class name
- `@discord_user` - (Optional) Mention Discord user to link
- `ilvl` - (Optional) Item level

**Example:**
```
!roster_add "John Doe" Healer Holy Area-52 Priest @John 650
!roster_add Tank "Protection" Illidan Paladin 645
```

---

### `!roster_remove <name>`
Remove a player from the roster.

**Example:**
```
!roster_remove "John Doe"
```

---

### `!roster_update_ilvl <name> <ilvl>`
Update a player's item level.

**Example:**
```
!roster_update_ilvl "John Doe" 655
```

---

### `!roster [role]`
Display the current raid roster, optionally filtered by role.

**Examples:**
```
!roster           # Show entire roster
!roster Tank      # Show only tanks
!roster Healer    # Show only healers
!roster MDPS      # Show only melee DPS
!roster RDPS      # Show only ranged DPS
```

**Display includes:**
- Status indicator (🟢 active / 🔴 inactive)
- Item level
- Player name
- Spec and class
- Grouped by role

---

### `!roster_info <name>` or `!rinfo <name>`
Get detailed information about a specific player.

**Example:**
```
!roster_info "John Doe"
!rinfo John
```

**Information shown:**
- Role and specializations (main/off)
- Class and realm
- Item level
- Status (active/inactive)
- Join date and last active date
- Notes
- Loot statistics (total items, last loot date)

---

### `!roster_bench <name>`
Set a player to inactive/benched status.

**Example:**
```
!roster_bench "John Doe"
```

---

### `!roster_activate <name>`
Restore a player to active status.

**Example:**
```
!roster_activate "John Doe"
```

---

### `!roster_benched`
Show all currently benched (inactive) players.

---

### `!roster_stats`
Display comprehensive roster statistics.

**Statistics shown:**
- Role distribution (tanks, healers, melee DPS, ranged DPS)
- Active vs benched player counts
- Average, highest, and lowest item levels

---

### `!roster_sync_ilvls`
Sync item levels from Raider.IO API for all active players.

**Requirements:**
- Players must have realm set in roster
- Raider.IO must have character data

**Response:** Shows which players were successfully updated and which failed.

---

### `!roster_link_discord <player_name> @member`
Link a Discord account to a roster player for better integration.

**Example:**
```
!roster_link_discord "John Doe" @John
```

**Benefits:**
- Better loot tracking attribution
- Easier player identification
- Integration with other bot features

---

### `!roster_help`
Display help information for all roster commands.

---


## Support & Contributing

For bugs, feature requests, or questions, please make a post in the spoocy-nugs forum channel in the discord.

---

## License

This bot is intended for use by the owner only.
