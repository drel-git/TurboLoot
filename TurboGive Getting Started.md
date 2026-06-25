# TurboGive- Quick Start Guide

## What it does
TurboGive moves items between your characters using rules in `turboloot.ini`. You define who gets what, then run one command to distribute.

**Patterns:** In `[GiveList]`, `_prefixN=Char:Pattern` can use **`Text*`** (starts with), **`*Text`** (ends with), and **`*Text*`** (contains). TurboLoot’s **`[Wildcards]`** section does **not** use that syntax- it only matches **name prefixes** for **looting**.

---

## 3 steps to get started

### 1. Add items to your give-list
- Target the character who should receive the item
- Pick up the item (from inventory or loot)
- Run: `/mac turbogive add` (or `/mac turbogive add target`)

Repeat for each item/character pair. TurboGive writes these to `turboloot.ini` under `[GiveList]`.

### 2. Distribute
- Run: `/mac turbogive`

You give your items to the group, and each group member gives theirs. Everyone runs the macro; the first person to run it coordinates.

### 3. (Optional) Collect or bank
- **Collect** - Get your items from others: `/mac turbogive collect` (group) or `collect all` (all bots in zone)
- **Bank** - Pull from bank: `/mac turbogive bank solo` (your items) or `bank` (tell group to pull theirs)

---

## Command reference (tables)

Use `/mac turbogive …` (case-insensitive). Names like `CHARNAME`, `NAME`, and `COUNT` are placeholders.

### Distribute

Everyone participates, one at a time.

| Command | What it does |
|--------|--------------|
| `/mac turbogive` | You + each group bot gives to group |
| `/mac turbogive all` | You + each e3 bot gives to all in zone |
| `/mac turbogive solo` | Only you give to group (no broadcast) |
| `/mac turbogive solo all` | Only you give to all in zone (no broadcast) |
| `/mac turbogive CHARNAME` | Give your items to one character |
| `/mac turbogive tell NAME` | Tell one bot to give their items |

### Hand out

Quantity-based distribution (`COUNT` = number per recipient).

| Command | What it does |
|--------|--------------|
| `/mac turbogive hand COUNT` | Give COUNT of **cursor** item to each recipient (group) |
| `/mac turbogive hand COUNT ItemName` | Give COUNT of named item to bots in **group** |
| `/mac turbogive hand COUNT ItemName all` | Give COUNT of named item to all e3 bots in **zone** |
| `/mac turbogive hand COUNT all` | Give COUNT of **cursor** item to all bots in zone |
| `/mac turbogive hand tell NAME COUNT ItemName` | Tell one bot to hand out to all bots in **group** |
| `/mac turbogive hand tell NAME COUNT ItemName all` | Tell one bot to hand out to all bots in **zone** |

### Collect

Others send **your** listed items to you.

| Command | What it does |
|--------|--------------|
| `/mac turbogive collect` | Group sends you your items |
| `/mac turbogive collect all` | All e3 bots send you your items (alias: `collect raid`) |
| `/mac turbogive collect tell NAME` | Tell one bot to collect from all bots in **group** |
| `/mac turbogive collect tell NAME all` | Tell one bot to collect from all bots in **zone** |

### Bank

Bank pulls into bags (or tell others to pull).

| Command | What it does |
|--------|--------------|
| `/mac turbogive bank` | Tell **group** to pull (you do not pull) |
| `/mac turbogive bank all` | Tell **all** e3 bots to pull (you do not) |
| `/mac turbogive bank solo` | **You** pull your items from your bank |
| `/mac turbogive bank pull` | **You** pull from bank for others (after broadcast or run directly) |
| `/mac turbogive bank NAME` | **You** pull NAME’s items from **your** bank |
| `/mac turbogive bank tell NAME` | Tell one bot to pull **their** items from **their** bank |

### Coin & Diamond Coins

Send or collect plat and Diamond Coins.

| Command                               | What it does                                     |
| ------------------------------------- | ------------------------------------------------ |
| `/mac turbogive CHARNAME cash`        | Send all available plat to CHARNAME              |
| `/mac turbogive CHARNAME plat`        | Send all available plat to CHARNAME              |
| `/mac turbogive CHARNAME pp`          | Send all available plat to CHARNAME              |
| `/mac turbogive CHARNAME cash 500`    | Send exactly 500 plat to CHARNAME                |
| `/mac turbogive CHARNAME pp 500`      | Send exactly 500 plat to CHARNAME                |
| `/mac turbogive CHARNAME dc`          | Send all available Diamond Coins to CHARNAME     |
| `/mac turbogive CHARNAME dc 100`      | Send exactly 100 Diamond Coins to CHARNAME       |
| `/mac turbogive collect cash`         | Group sends you their available plat             |
| `/mac turbogive collect cash 500`     | Group sends you 500 plat each                    |
| `/mac turbogive collect cash all`     | All online e3 bots send you their available plat |
| `/mac turbogive collect cash all 500` | All online e3 bots send you 500 plat each        |
| `/lua run turbo_collect_dc`           | Collect all Diamond Coins from online bots       |

### NPC Hand-in

Hand items or coins to your current target, usually an NPC.

| Command                                                     | What it does                                                                                   |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `/mac turbogive handin "Item Name" #`                       | Hand # of Item Name to your current target                                                     |
| `/mac turbogive handin "Item Name" # destroy`               | Hand # of Item Name to your current target, then destroy leftovers if needed                   |
| `/mac turbogive handin pp #`                                | Hand # plat to your current target                                                             |
| `/mac turbogive handin gp #`                                | Hand # gold to your current target                                                             |
| `/mac turbogive handin sp #`                                | Hand # silver to your current target                                                           |
| `/mac turbogive handin cp #`                                | Hand # copper to your current target                                                           |
| `/mac turbogive handin tell CHARNAME "Item Name" #`         | Tell CHARNAME to hand # of Item Name to their current target                                   |
| `/mac turbogive handin tell CHARNAME "Item Name" # destroy` | Tell CHARNAME to hand # of Item Name to their current target, then destroy leftovers if needed |

### Setup & info

| Command | What it does |
|--------|--------------|
| `/mac turbogive add` | Add **cursor** item to **current target**’s give-list |
| `/mac turbogive add NAME` | Same, explicit character name |
| `/mac turbogive list` | Show **your** give-list |
| `/mac turbogive list NAME` | Show give-list for **NAME** |
| `/mac turbogive list target` | Show give-list for **current target** (`list t` also works in-game) |
| `/mac turbogive status` | Show config, recipients, zone |
| `/mac turbogive help` | Full command list (alias: `commands`) |

---

## Excluding items (wildcard / prefix only)

If an item matches a `[GiveList]` **_prefix** pattern or shared **`[Wildcards]`** rules but you **do not** want TurboGive to move it, add a **`[GiveExclude]`** section in `turboloot.ini`:

```ini
[GiveExclude]
_list="Tome of Nife's Mercy"|"Some Other Item Name"
```

Use **exact** in-game names, separated by **`|`**. This only blocks **pattern** matches; a normal **`ItemName=Receiver`** line under `[GiveList]` still assigns that item. `/mac turbogive status` shows your `_list` when set.

---

## Tips
- **Aliases** - Create aliases (e.g. `/tg` for `/mac turbogive`) for faster use
- **Shared INI** - TurboGive uses the same `turboloot.ini` as TurboLoot
- **Help** - Run `/mac turbogive help` anytime for the full command list
