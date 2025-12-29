# Enhanced Discord Bot Features - Design Document

## 1. Attendance Tracking System

### Commands

#### `!attendance_start [raid_name]`
Start attendance tracking for current raid.

**Example:**
```
!attendance_start "Nerub-ar Palace Heroic"
!attendance_start              # Uses default: "Raid - [Date]"
```

**How it works:**
1. Creates attendance session with timestamp
2. Automatically marks all players in voice channel as present
3. Displays attendance panel with reaction buttons
4. Players can react with ✅ if auto-detection missed them

---

#### `!attendance_mark <@player> [status]`
Manually mark attendance for a player.

**Statuses:**
- `present` (default) - Player attended
- `late` - Player arrived late (specify minutes)
- `left_early` - Player left early
- `excused` - Excused absence
- `unexcused` - No-show

**Examples:**
```
!attendance_mark @John present
!attendance_mark @Jane late 15
!attendance_mark @Bob left_early
!attendance_mark @Alice excused
```

---

#### `!attendance_end`
End current attendance session.

**Response:** Shows session summary:
- Total attendees
- Late arrivals
- Early departures
- Duration

---

#### `!attendance_stats [@player] [timeframe]`
View attendance statistics.

**Timeframes:** `week`, `month`, `season`, `all` (default: month)

**Examples:**
```
!attendance_stats                    # Guild-wide monthly stats
!attendance_stats @John             # John's monthly attendance
!attendance_stats @John season      # John's season attendance
!attendance_stats month             # Guild monthly stats
```

**Player Stats Include:**
- Attendance rate (%)
- Raids attended / Total raids
- Times late
- Times left early
- Trend indicator (📈 improving, 📉 declining, ➡️ steady)

**Guild Stats Include:**
- Average attendance rate
- Most consistent raiders (top 10)
- Players needing improvement
- Attendance by role breakdown

---

#### `!attendance_report [week|month|custom start_date end_date]`
Generate detailed attendance report.

**Examples:**
```
!attendance_report week
!attendance_report month
!attendance_report custom 2024-12-01 2024-12-15
```

**Report includes:**
- Total raids held
- Average attendance rate
- Perfect attendance players
- Role attendance breakdown (tanks/healers/dps)
- Players below threshold (configurable, default 75%)
- Attendance heatmap (calendar view)

---

#### `!attendance_excuse @player <date> [reason]`
Mark an absence as excused (Officer only).

**Example:**
```
!attendance_excuse @John 2024-12-25 "Family holiday"
```

---

#### `!attendance_threshold <percentage>`
Set minimum attendance threshold for loot priority (Officer only).

**Example:**
```
!attendance_threshold 75    # Require 75% attendance for normal loot priority
```

**Impact:** Players below threshold are flagged in loot priority list.

---

#### `!attendance_history [limit]`
View recent raid sessions.

**Example:**
```
!attendance_history      # Last 10 sessions
!attendance_history 20   # Last 20 sessions
```

---

## 2. Raid Scheduling & Reminders

### Commands

#### `!raid_schedule <date> <time> <raid_name> [description]`
Schedule a raid event.

**Date formats:** `YYYY-MM-DD`, `tomorrow`, `friday`, `next tuesday`
**Time format:** `HH:MM` in server time or with timezone (e.g., `8:00 PM EST`)

**Examples:**
```
!raid_schedule 2024-12-30 8:00PM "Nerub-ar Palace Heroic" "Full clear attempt"
!raid_schedule friday 7:30PM "M+ Night"
!raid_schedule tomorrow 8:00PM "Mythic Progression"
```

**Creates:**
- Discord event (if permissions allow)
- Signup sheet with role buttons
- Automatic reminders (24h, 2h, 30m before)

---

#### `!raid_list [timeframe]`
List scheduled raids.

**Timeframes:** `week` (default), `month`, `all`

**Example:**
```
!raid_list        # This week's raids
!raid_list month  # This month's raids
```

---

#### `!raid_cancel <raid_id>`
Cancel a scheduled raid (Officer only).

**Example:**
```
!raid_cancel raid_20241230_200000
```

**Action:** Sends notification to all signed up players.

---

#### `!raid_reschedule <raid_id> <new_date> <new_time>`
Reschedule a raid (Officer only).

**Example:**
```
!raid_reschedule raid_20241230_200000 2024-12-31 8:00PM
```

---

#### `!signup <raid_id> <role> [spec]`
Sign up for a scheduled raid.

**Roles:** Tank, Healer, MDPS, RDPS

**Examples:**
```
!signup raid_20241230_200000 Tank Protection
!signup raid_20241230_200000 Healer
```

**Alternative:** Click role buttons in raid announcement message.

---

#### `!signup_bench <raid_id>`
Sign up as backup/bench.

**Example:**
```
!signup_bench raid_20241230_200000
```

---

#### `!decline <raid_id> [reason]`
Decline raid attendance.

**Example:**
```
!decline raid_20241230_200000 "Out of town"
```

---

#### `!tentative <raid_id> [reason]`
Mark attendance as tentative.

**Example:**
```
!tentative raid_20241230_200000 "Might have to work late"
```

---

#### `!signups <raid_id>`
View current signups for a raid.

**Display shows:**
- Confirmed players by role
- Tentative players
- Declined players
- Players who haven't responded
- Current composition viability

---

#### `!roster_suggest <raid_id>`
Auto-suggest optimal raid composition from signups (Officer only).

**Features:**
- Prioritizes by attendance rate
- Balances roles (2 tanks, 4-5 healers, rest DPS)
- Considers loot priority
- Flags composition issues

---

#### `!reminder_set <raid_id> <time_before> [message]`
Set custom reminder for raid (Officer only).

**Examples:**
```
!reminder_set raid_20241230_200000 1h "Don't forget consumables!"
!reminder_set raid_20241230_200000 30m "Invites going out soon"
```

---

#### `!reminder_test`
Send yourself a test reminder to check formatting.

---

## 3. Performance Metrics (Warcraft Logs Integration)

### Commands

#### `!logs_link <character_name> <realm> <region>`
Link your character to Warcraft Logs.

**Regions:** `us`, `eu`, `kr`, `tw`, `cn`

**Example:**
```
!logs_link Johndoe Area-52 us
```

---

#### `!parses [@player] [boss] [metric]`
View parse rankings.

**Metrics:** `dps`, `hps`, `tank` (default: role-appropriate)

**Examples:**
```
!parses                          # Your recent parses
!parses @John                    # John's recent parses
!parses @John Ulgrax            # John's Ulgrax parses
!parses @John Ulgrax dps        # John's DPS parses on Ulgrax
```

**Display shows:**
- Best parse (color-coded: grey, green, blue, purple, orange, gold, pink)
- Average parse across all bosses
- Recent trend (📈 improving, 📉 declining)
- Median ilvl parse (parse relative to item level)
- Best/worst bosses

---

#### `!raid_performance [raid_id]`
Post-raid performance summary.

**Example:**
```
!raid_performance raid_20241230_200000  # Specific raid
!raid_performance                       # Most recent raid
```

**Summary includes:**
- Top DPS performers (with parse colors)
- Top HPS performers
- Tank performance highlights
- Deaths analysis (avoidable vs unavoidable)
- Mechanic execution (interrupts, dispels, soaks)
- Best improved players vs last week

---

#### `!rankings [role] [boss]`
View guild rankings for specific encounters.

**Examples:**
```
!rankings                # All roles, all bosses
!rankings dps           # DPS rankings across all bosses
!rankings healer Ulgrax # Healer rankings for Ulgrax
```

---

#### `!compare <@player1> <@player2> [boss]`
Compare two players' performance.

**Example:**
```
!compare @John @Jane            # Overall comparison
!compare @John @Jane Ulgrax     # Ulgrax-specific comparison
```

**Shows:**
- Parse comparison
- DPS/HPS difference
- Death counts
- Mechanic execution
- Uptime differences

---

#### `!weekly_recap`
Generate weekly performance recap (Auto-posts on reset day).

**Includes:**
- Top performers of the week
- Most improved players
- Parse color distribution
- Guild average parse percentile
- Killed bosses with guild best times

---

#### `!player_of_week`
Nominate and vote for player of the week.

**How it works:**
1. Officers nominate players with reasons
2. Guild members vote via reactions
3. Winner announced Monday (configurable)
4. Receives special role for the week

**Example:**
```
!player_of_week nominate @John "Clutch CR on Mythic progression"
```

---

#### `!logs_sync`
Sync latest raid logs for all linked characters (Officer only).

**Action:** Fetches latest logs from Warcraft Logs API.

---

#### `!parse_requirements set <role> <minimum_parse>`
Set minimum parse requirements (Officer only).

**Example:**
```
!parse_requirements set dps 50    # DPS should parse at 50+ percentile
!parse_requirements set healer 40 # Healers should parse at 40+
```

---

## 4. Substitution Management

### Commands

#### `!sub_needed <role> <date> <time> [description]`
Broadcast need for substitute.

**Example:**
```
!sub_needed Tank 2024-12-30 8:00PM "Main tank out sick"
!sub_needed Healer friday 7:30PM "Need 1 more healer"
```

**Actions:**
- Posts in substitutes channel
- Pings @Substitute role
- Shows "I can sub!" button
- Lists potential subs from bench

---

#### `!sub_available [dates]`
Mark yourself available as substitute.

**Examples:**
```
!sub_available                           # Available for all upcoming raids
!sub_available 2024-12-30               # Available specific date
!sub_available fridays                   # Available all Fridays
!sub_available "weekends in January"    # Available date range
```

---

#### `!sub_unavailable [dates]`
Mark yourself unavailable.

**Example:**
```
!sub_unavailable 2024-12-30
!sub_unavailable "next week"
```

---

#### `!sub_list [role] [date]`
View available substitutes.

**Examples:**
```
!sub_list                    # All available subs
!sub_list Tank              # Available tanks
!sub_list Healer 2024-12-30 # Healers available on specific date
```

**Display shows:**
- Role and spec
- Item level
- Recent parse average
- Availability notes
- Last sub date

---

#### `!sub_request @player <date> <time> [raid_name]`
Request specific player to sub (Officer only).

**Example:**
```
!sub_request @John 2024-12-30 8:00PM "Heroic clear"
```

**Actions:**
- DMs player with request
- Accept/Decline buttons
- If declined, asks for reason

---

#### `!sub_history [@player]`
View substitution history.

**Examples:**
```
!sub_history        # Guild sub history
!sub_history @John  # John's sub history
```

---

#### `!pug_add <player_name> <class> <spec> <role> [realm] [notes]`
Add PUG player who performed well.

**Example:**
```
!pug_add "SuperDPS" Mage Fire RDPS Area-52 "Great DPS, knew mechanics"
```

---

#### `!pug_list [role]`
View saved PUG contacts.

**Example:**
```
!pug_list        # All PUGs
!pug_list Tank   # Tank PUGs only
```

---

#### `!pug_rate <player_name> <rating> [comment]`
Rate a PUG player's performance.

**Rating:** 1-5 stars

**Example:**
```
!pug_rate "SuperDPS" 5 "Excellent, would invite again"
```

---

## 5. Raid Cooldown Tracker

### Commands

#### `!cd_setup <boss_name>`
Set up cooldown rotation for a boss (Officer only).

**Example:**
```
!cd_setup "Ulgrax the Devourer"
```

**Actions:**
- Lists common raid CDs (Heroism, Tank CDs, Raid defensives)
- Allows assignment to players
- Creates timeline

---

#### `!cd_assign <boss> <cooldown> <@player> <timing>`
Assign a cooldown to a player.

**Timings:** `pull`, `30s`, `1m`, `1:30`, `phase2`, etc.

**Examples:**
```
!cd_assign Ulgrax "Rally" @John pull
!cd_assign Ulgrax "Barrier" @Jane 1:30
!cd_assign Ulgrax "Aura Mastery" @Bob phase2
```

---

#### `!cd_plan <boss>`
Display cooldown rotation for a boss.

**Example:**
```
!cd_plan Ulgrax
```

**Display shows:**
- Timeline from pull to kill
- Assigned cooldowns with player names
- Role grouping (Tank CDs, Healing CDs, Raid CDs)
- Notes on when major damage occurs

---

#### `!cd_my <boss>`
Show YOUR assigned cooldowns for a boss.

**Example:**
```
!cd_my Ulgrax
```

---

#### `!cd_call <boss> <timing>`
Announce cooldown timing in raid chat.

**Example:**
```
!cd_call Ulgrax pull
```

**Posts:** "Ulgrax Pull CDs: @John (Rally), @Jane (Barrier)"

---

#### `!cd_check <boss>`
Check which cooldowns are ready (compares with current logs).

**Example:**
```
!cd_check Ulgrax
```

**Shows:** Which assigned players are present and have CDs available.

---

#### `!cd_template <boss>`
Load a preset cooldown template (Officer only).

**Example:**
```
!cd_template Ulgrax
```

**Templates include:**
- Common timings from strategy guides
- Can be customized after loading

---

#### `!cd_clear <boss>`
Clear all cooldown assignments for a boss (Officer only).

---

## 6. Role Check with Alts

### Commands

#### `!alt_register <alt_name> <class> <spec> <role> <realm> [ilvl]`
Register an alt character.

**Example:**
```
!alt_register "Johnalt" Warrior Arms MDPS Area-52 645
!alt_register "Healbot" Priest Holy Healer Illidan 650
```

**Actions:**
- Links alt to your Discord account
- Alt appears in role checks
- Tracks alt's loot separately

---

#### `!alt_list [@player]`
List your alts or another player's alts.

**Examples:**
```
!alt_list          # Your alts
!alt_list @John    # John's alts
```

**Display shows:**
- Character name
- Class and spec
- Role
- Item level
- Status (active/benched)

---

#### `!alt_remove <alt_name>`
Remove an alt from your roster.

**Example:**
```
!alt_remove "Johnalt"
```

---

#### `!alt_main <alt_name>`
Switch an alt to be your main character.

**Example:**
```
!alt_main "Johnalt"
```

**Actions:**
- Previous main becomes an alt
- Updates roster automatically
- Transfers loot history

---

#### `!alt_update_ilvl <alt_name> <ilvl>`
Update an alt's item level.

**Example:**
```
!alt_update_ilvl "Johnalt" 655
```

---

#### `!role_check [role]`
Check who can play what roles (includes alts).

**Examples:**
```
!role_check        # All roles
!role_check Tank   # Who can tank
!role_check Healer # Who can heal
```

**Display shows:**
- Main characters by role
- Available alts by role
- Item levels
- Spec details
- Status indicators

**Example output:**
```
=== TANKS (4 available) ===
Main Characters:
🟢 645 - JohnMain (Protection Paladin) - @John
🟢 650 - TankBoss (Protection Warrior) - @Jane

Available Alts:
🔵 640 - Johnalt (Guardian Druid) - @John [ALT]
🔵 635 - Bobtank (Brewmaster Monk) - @Bob [ALT]
```

---

#### `!can_play <role> [@player]`
Check if you or someone can play a specific role.

**Examples:**
```
!can_play Tank          # Check if you can tank
!can_play Healer @John  # Check if John can heal
```

---

#### `!flex_players`
List players who can play multiple roles.

**Display shows:**
- Players with alts in different roles
- Players with multiple specs
- Useful for filling gaps in roster

---

## 7. Wishlist System

### Commands

#### `!wishlist_add <item_name> [priority]`
Add an item to your personal wishlist.

**Priority:** `bis`, `upgrade`, `nice` (default: upgrade)

**Examples:**
```
!wishlist_add "House of Cards" bis
!wishlist_add "Seal of Broken Fate" upgrade
!wishlist_add "Ring of Power" nice
```

---

#### `!wishlist_remove <item_name>`
Remove an item from your wishlist.

**Example:**
```
!wishlist_remove "House of Cards"
```

---

#### `!wishlist [@player]`
View your wishlist or another player's wishlist.

**Examples:**
```
!wishlist          # Your wishlist
!wishlist @John    # John's wishlist
```

**Display shows:**
- Items sorted by priority
- Whether item is BiS for your spec
- Current drop sources

---

#### `!who_needs <item_name>`
See who has an item on their wishlist.

**Example:**
```
!who_needs "House of Cards"
```

**Display shows:**
- Players who want the item
- Their priority level (BiS/Upgrade/Nice)
- Whether it's BiS for their spec
- Their current loot priority score
- Last time they received loot

**Example output:**
```
📋 Who Needs: House of Cards

BIS Priority (3 players):
🔥 @John (Fire Mage) - BiS for spec | Priority: 85 | Last loot: 5 days ago
🔥 @Jane (Havoc DH) - BiS for spec | Priority: 92 | Last loot: 2 days ago
🔥 @Bob (Arms Warrior) - BiS for spec | Priority: 78 | Last loot: 12 days ago

Upgrade Priority (2 players):
📈 @Alice (Frost Mage) | Priority: 45 | Last loot: 7 days ago
📈 @Charlie (Fury Warrior) | Priority: 52 | Last loot: 4 days ago
```

---

#### `!wishlist_import <wowhead_link>`
Import wishlist from Wowhead gear planner.

**Example:**
```
!wishlist_import https://www.wowhead.com/gear-planner/...
```

---

#### `!wishlist_sync`
Sync your wishlist with BiS database.

**Actions:**
- Automatically marks BiS items for your spec
- Suggests items you might be missing
- Updates priorities based on BiS status

---

#### `!wishlist_compare <item1> <item2>`
Compare two items on your wishlist.

**Example:**
```
!wishlist_compare "House of Cards" "Seal of Broken Fate"
```

**Shows:**
- Stat differences
- BiS status for your spec
- Which bosses drop them
- Competition (how many others want each)

---

#### `!wishlist_stats`
View wishlist statistics for the guild.

**Shows:**
- Most wanted items
- Items with most competition
- Items nobody wants
- Average wishlist size
- Wishlist completion rate

---

#### `!wishlist_cleared <item_name>`
Mark when you've obtained an item (auto-removes from wishlist).

**Example:**
```
!wishlist_cleared "House of Cards"
```

**Actions:**
- Removes from wishlist
- Records in loot history
- Updates priority scores

---

#### `!loot_wishlist <item_name>`
Combined command: Start loot council AND show wishlists.

**Example:**
```
!loot_wishlist "House of Cards"
```

**Actions:**
- Starts normal loot council process
- Additionally shows who has item wishlisted
- Shows priority suggestions based on:
  - Wishlist priority
  - Loot priority score
  - Whether it's BiS for spec
  - Time since last loot

---

## Integration Notes

### Attendance + Loot Priority
- Attendance below threshold reduces loot priority
- Perfect attendance bonus in loot priority calculation

### Performance + Loot Priority
- Consistently high parsers get slight priority boost (configurable)
- Players performing below requirements may receive loot coaching

### Signups + Attendance
- Auto-marks attendance for confirmed signups
- Tracks signup reliability (signed up vs actually attended)

### Alts + Loot
- Alt loot tracked separately
- Main always has priority over alt
- Alt wishlist separate from main wishlist

### Wishlist + Loot Council
- Loot council sees wishlists during voting
- Priority levels inform council decisions
- Integration with BiS system

### Substitutes + Attendance
- Subs receive attendance credit
- Subs can build loot priority
- Track sub reliability for future consideration
