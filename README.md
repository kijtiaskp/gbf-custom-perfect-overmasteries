# GBF Custom Perfect Overmasteries

Custom mod tool for **Granblue Fantasy Relink** — choose exactly which Over Mastery (Meditation) bonuses appear. All 4 slots guaranteed, perfect values, every roll. Duplicates supported where the game allows.

**🌐 Web editor: https://kijtiaskp.github.io/gbf-custom-perfect-overmasteries/**

## What it does

Instead of rolling random Meditation bonuses, every Meditate gives you exactly the 4 bonuses you picked — all at perfect values, every time.

## Files

| File | Description |
|------|-------------|
| `index.html` | Web editor — pick bonuses on an interactive diamond grid and download the .tbl |
| `limit_bonus_meditation_category.tbl` | Ready-to-use table file (ATK, Crit, Normal Atk Cap, Skill Cap) |
| `bg.jpg` | Editor background art |

## Requirements

1. [Reloaded-II](https://reloaded-project.github.io/Reloaded-II/) — mod loader
2. [GBFR Mod Manager](https://www.nexusmods.com/granbluefantasyrelink/mods/17) — `gbfrelink.utility.manager`
3. [Perfect Overmasteries](https://www.nexusmods.com/granbluefantasyrelink/mods/444) — base mod

## Usage

### Quick start

1. Copy `limit_bonus_meditation_category.tbl` to:
   ```
   <Reloaded-II>\Mods\gbfrelink.perfect.overmasteries\GBFR\data\system\table\
   ```
2. Launch the game through Reloaded-II with **Perfect Overmasteries** enabled

### Custom selection

1. Open the [web editor](https://kijtiaskp.github.io/gbf-custom-perfect-overmasteries/) (or `index.html` locally)
2. Fill the 4 slots — **left-click** a diamond to add it, **right-click** to remove. The same bonus can be picked more than once (see limits below). Your picks are saved in the browser automatically
3. Click **Download .tbl** and place the file in the table folder above
4. Launch the game

### Duplicate limits

The game identifies each bonus by its tier ID hash, and merges entries that share a hash. Max copies per bonus = number of tier IDs that exist in the game data:

| Max copies | Bonuses |
|------------|---------|
| ×4 | Attack Power Up, HP Up (5 tier IDs) |
| ×3 | Critical Hit Rate Up, Stun Power Up (3 tier IDs) |
| ×1 | Everything else (1 tier ID) |

The editor lets you pick past these limits, but marks the extra copies — they won't appear in-game. Each duplicate copy uses the next tier's hash, so values differ slightly between copies (each tier has its own perfect value).

## Available bonuses

| Color | Bonuses |
|-------|---------|
| 🔴 Red | Attack Power Up, Normal Attack Damage Cap Up |
| 🟢 Green | HP Up, Skill Healing Cap Up |
| 🟣 Purple | Skill Damage Up, Skill Damage Cap Up |
| 🟡 Yellow | Chain Burst Damage Up, Skybound Art Damage Up, Skybound Art Damage Cap Up |
| ⚪ White | Critical Hit Rate Up, Stun Power Up |

## How it works

The mod patches `limit_bonus_meditation_category.tbl`, which defines the Meditation bonus pool. The file is a little-endian int32 table: a `[count, 0]` header followed by `[hash, slot, perfect]` entries. Putting exactly 4 entries in the pool (all as `slot=0, perfect=1`) makes the game sample without replacement and fill all 4 slots with exactly those entries at perfect values. Duplicates of a bonus are encoded as different tier hashes of the same stat.

Bonus name hashes use XXHash32 with seed `0x178A54A4` — hash database from [GBFRDataTools](https://github.com/Nenkai/GBFRDataTools) by Nenkai.
