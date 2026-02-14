<p align="center">
  <img src="https://github.com/user-attachments/assets/f627c110-ed9a-45ff-9631-f3ef5d426959" alt="TurboLoot Banner" width="100%">
</p>

# Quickstart Guide

> **TurboLoot** - Automated INI based corpse looting, selling, banking, tributing, and destroying for EverQuest EMUs (E3Next / MacroQuest)

## 📥 Download

For most users, the easiest way to get started is to download the latest release:

👉 https://github.com/drel-git/TurboLoot/releases/latest

This includes the latest macros and a starter INI.

---

## What's In the Box

| File | What It Does |
|---|---|
| `TurboLoot.mac` | The main macro - runs all looting and inventory management |
| `TurboKey.mac` | Quick-tag helper - pick up an item, run one command, and it's categorized in your INI |
| `TurboLoot.ini` | Your personal rulebook - tells TurboLoot what to keep, sell, bank, tribute, trash, or ignore example INI |

---

## 1. Installation (2 Minutes)

1. Drop **`TurboLoot.mac`** and **`TurboKey.mac`** into your MQ **`Macros`** folder (or **`Config`** folder - the macro checks both).
2. Drop **`TurboLoot.ini`** into the same folder.
3. In-game, type: `/mac TurboLoot` - if it runs without errors, you're set.

---

## 2. How TurboLoot Decides What to Loot

TurboLoot checks items in this order:

1. **Exact item name in `[ItemLimits]`** - if it finds a rule for the item, it follows that rule.
2. **Wildcard matches** - spell scrolls (`SPELL:`), skill-ups (`SKILL:`), tomes, songs are handled by wildcard settings.
3. **Value-based rules** - if the item isn't listed, it checks against your minimum platinum thresholds:
   - `lootHighValueMinPP` - loot non-stackable items worth this many pp or more (default: 50)
   - `lootStackableMinPP` - loot stackable items worth this many pp or more (default: 50)
4. **Everything else** - skipped and announced (or silently ignored, depending on your settings).

---

## 3. The INI at a Glance

The INI has two sections: **`[Settings]`** and **`[ItemLimits]`**.

### [Settings] - Recommended Defaults

```ini
[Settings]
; --Core--
lootRadiusFeet=50
inventoryWarnSlots=5
debug=OFF

; --Looting & Selling Rules--
lootHighValueMinPP=50
lootStackableMinPP=50
sellUnlistedStackable=OFF
sellUnlistedItems=OFF
sellWildcards=OFF
bankWildcards=ON
StopLootWhenAttacked=OFF

; --Announcements (where messages go)--
announceDefaultTo=e3bc
announceSkipTo=gsay
announceBankSellPerItem=ON
announceLoot=ON
announceDestroy=ON
announceRunSummary=ON
autoRsayInRaid=OFF

; --Advanced--
followRestoreMode=NONE
followRestoreDriver=AUTO
lootNoDropPrompt=never
lootNoDropPromptReset=always
```

### What Each Setting Does

| Setting | Default | What It Does |
|---|---|---|
| `lootRadiusFeet` | 50 | How far (in feet) TurboLoot will search for corpses |
| `inventoryWarnSlots` | 5 | Warns you when you have this many free inventory slots left |
| `lootHighValueMinPP` | 50 | Loot non-stackables worth ≥ this many pp. `0` = disabled, `1` = loot everything |
| `lootStackableMinPP` | 50 | Same idea but for stackable items |
| `sellUnlistedItems` | OFF | When selling, also sell items not in your INI (be careful with this!) |
| `sellUnlistedStackable` | OFF | When selling, also sell unlisted stackable items |
| `sellWildcards` | OFF | Auto-sell wildcard items (spell scrolls, etc.) unless marked otherwise |
| `bankWildcards` | ON | Auto-bank wildcard items (spell scrolls, etc.) unless marked otherwise |
| `StopLootWhenAttacked` | OFF | Stop looting if hostile mobs are detected nearby |
| `announceDefaultTo` | e3bc | Where messages go: `echo` (local MQ window), `e3bc` (MQ network), `say`, `gsay`, `rsay`, or `t CharName` |
| `announceSkipTo` | gsay | Where skip messages go - `gsay` lets your group see what was left behind |
| `announceLoot` | ON | Announce when items are looted |
| `announceDestroy` | ON | Announce when items are destroyed |
| `announceRunSummary` | ON | Show a summary at the end of each loot run |
| `announceBankSellPerItem` | ON | Announce each item individually during bank/sell/tribute operations |
| `autoRsayInRaid` | OFF | Auto-switch announcements to `/rsay` when in a raid |
| `lootNoDropPrompt` | never | No-drop behavior: `prompt` (ask), `always` (grab it), `never` (leave it) |
| `followRestoreMode` | NONE | Options: `FOLLOWME`, `CHASEME`, `NONE` - restore follow after looting |
| `followRestoreDriver` | AUTO | Who to follow: `AUTO` (group leader) or a specific character name |

### [ItemLimits] - Your Loot Rules

This is where you tell TurboLoot exactly what to do with specific items. Add one line per item:

```ini
[ItemLimits]
Item Name=RULE
```

| Rule | What Happens |
|---|---|
| `KEEP` | Always loot, never sell/bank/destroy |
| `5` (any number) | Loot up to that many, then stop - alerts you when the limit is reached |
| `SELL` | Loot it, then sell it when you run the sell command |
| `BANK` | Loot it, then bank it when you run the bank command |
| `TRIBUTE` | Loot it, then tribute it when you run the tribute command |
| `DESTROY` | Loot it and destroy it immediately |
| `IGNORE` | Skip it completely - no announcement, no looting |

> **Note:** `ALL` still works as a legacy alias for `KEEP`, but `KEEP` is the preferred keyword going forward.

**Examples:**
```ini
[ItemLimits]
Kunark Green Pinot Gris=5
Emerald=KEEP
Junk Item=SELL
Mystery Orb=BANK
Some Tribute Item=TRIBUTE
Useless Trash=DESTROY
Rusty Dagger=IGNORE
```

---

## 4. Commands

Use `/e3bct LOOTERCHARACTERNAME` before any command below to have a bot run it instead of you.

| Command | What It Does |
|---|---|
| `/mac turboloot` | **Loot mode** - runs to nearby corpses and loots based on your rules |
| `/mac turboloot sell` | Sells all items marked `SELL` (must be at a vendor) |
| `/mac turboloot bank` | Banks all items marked `BANK` (must be at a banker) |
| `/mac turboloot tribute` | Tributes all items marked `TRIBUTE` (must be at a tribute master) |
| `/mac turboloot unload` | **Full dump:** bank → tribute → sell → destroy (all in one when you're in town) |
| `/mac turboloot report` | Preview what *would* be sold - no items are touched |
| `/mac turboloot help` | Show the command list in-game |

### Set Up Aliases (Recommended)

Instead of typing the full commands every time, set up short aliases. Open your **`MacroQuest.ini`** in your E3Next Config folder and add these under `[Aliases]`:

```ini
[Aliases]
/turboloot=/squelch /mac turboloot
/turbosell=/squelch /mac turboloot sell
/turbobank=/squelch /mac turboloot bank
/turbotribute=/squelch /mac turboloot tribute
/turbounload=/squelch /mac turboloot unload
/turboreport=/squelch /mac turboloot report
```

Now you can just type `/turboloot`, `/turbosell`, `/turboreport`, etc.

---

## 5. TurboKey - Tag Items on the Fly

### Instead of manually editing your INI, set up hotkeys for each rule. 
Pick up an item, hit the hotkey, and it's tagged in your INI automatically.
```
/mac TurboKey RULE
```

Where `RULE` is one of: 
- `/mac TurboKey KEEP`
- `/mac TurboKey SELL`
- `/mac TurboKey IGNORE`
- `/mac TurboKey BANK`
- `/mac TurboKey TRIBUTE`
- `/mac TurboKey DESTROY`

**What happens:**
- The item on your cursor gets written into `turboloot.ini` under `[ItemLimits]` with that rule.
- If the rule is `DESTROY` or `IGNORE`, the item is destroyed off your cursor.
- For anything else, the item goes into your inventory via `/autoinv`.
- If the item already had a rule, it gets overwritten and you'll see the old → new change in chat.

**Example:** You loot a "Cracked Staff" and want to sell them from now on:
1. Pick it up (it's on your cursor)
2. Type `/mac TurboKey SELL`
3. Done - next time TurboLoot sees a Cracked Staff, it marks it for selling.

---

## 6. Typical Workflow

1. **Kill mobs** → Run `/turboloot` (or set up auto-looting below)
2. **While grinding**, use `/mac TurboKey RULE` to categorize new items as you see them
3. **Before selling for the first time**, run `/turboreport` to preview what would be sold
4. **When bags are full**, head to town:
   - Run `/turbounload` near a banker, tribute master, and vendor to handle everything at once
   - Or run the individual commands (`/turbobank`, `/turbotribute`, `/turbosell`) one at a time

---

## 7. Auto-Looting Setup (E3Next)

This is the real power - TurboLoot runs automatically whenever your tank kills something.

### Prerequisites
- `TurboLoot.mac` and `turboloot.ini` are installed (you can verify with `/mac turboLoot`)

### Step 1: Add the Event Trigger to Your Tank's E3Next INI

Paste these into your **tank's** character INI:

**Under `[EventRegMatches]`:**
```ini
Tloot=slain
```

**Under `[Events]`:**
```ini
Tloot=/timed 10 /if (!${Spawn[npc radius 50].Aggressive} && ${Turbo} && ${SpawnCount[npccorpse radius 50]}) /e3bct YOUR_LOOTER_NAME /mac turboLoot
```
> Replace `YOUR_LOOTER_NAME` with the character name of whoever should do the looting.

**How it works:** When the game says something was "slain," the tank waits 1 second, checks that no aggressive mobs are within 50 units, checks that the `Turbo` toggle is on, checks that there's a corpse nearby, and then tells the looter character to run TurboLoot.

### Step 2: Optional - Auto-Enable on Startup

If you want auto-loot **on by default** when you load E3Next, add this to your tank's INI:

**Under `[Startup Commands]`:**
```ini
Command=/e3varset Turbo True
```

### Step 3: Toggle Auto-Loot On/Off

You need a way to flip auto-looting on and off. Choose **one** of these methods:

---

#### Option A: Button Master (Fancy Toggle Button)

If you use Button Master, create a button with:

**Button Label** (enable "Evaluate Label"):
```lua
return 'Auto: ' .. (tostring(mq.TLO.MQ2Mono.Query('e3, Turbo')()) == 'true' and "Loot ON" or "Loot OFF")
```

**Button Body:**
```
/docommand ${If[${Bool[${MQ2Mono.Query[e3,Turbo]}]},/e3bcaa /e3varset Turbo false,/e3bcaa /e3varset Turbo true]}
```

This gives you a single button that shows "Auto: Loot ON" or "Auto: Loot OFF" and toggles the state across all characters when clicked.

---

#### Option B: Two Hotkeys (No Button Master Needed)

Just make two in-game hotkeys:

**Hotkey 1 - Enable Auto-Loot:**
```
/e3varset Turbo true
```

**Hotkey 2 - Disable Auto-Loot:**
```
/e3varset Turbo false
```

Press one to turn it on, the other to turn it off. Simple.

---

### Step 4: Test It

1. **Check the toggle status:**
   ```
   /echo ${Bool[${MQ2Mono.Query[e3,Turbo]}]}
   ```
   Should return `TRUE` or `FALSE`.

2. **Toggle to Loot ON** (via button or hotkey).

3. **Kill a mob** - when you see the "slain" message, the looter should automatically run TurboLoot and start grabbing items.

4. **Toggle to Loot OFF** - kill another mob and confirm looting does *not* trigger.

---

## 8. Multiple Looters

By default, every character shares the same `turboloot.ini`. That works fine if you only have one looter or if all your looters follow the same rules. But if you want **multiple characters looting with different rules** (e.g., your bard loots gems to sell, your mage banks tradeskill mats), you can run separate copies of the macro with their own INI files.

### Setup: Duplicate Files Per Looter

1. **Copy the three files** and rename them with a suffix:
   - `TurboLoot.mac` -> `TurboLoot2.mac`
   - `TurboKey.mac` -> `TurboKey2.mac`
   - `turboloot.ini` -> `turboloot2.ini`

2. **Edit `TurboLoot2.mac`** - find the lines that reference `turboloot.ini` and change them to `turboloot2.ini`:
   ```
   ../Config/turboloot.ini  ->  ../Config/turboloot2.ini
   ../Macros/turboloot.ini  ->  ../Macros/turboloot2.ini
   ```

3. **Edit `TurboKey2.mac`** - same thing, update the INI path to `turboloot2.ini`.

4. **Edit `turboloot2.ini`** with your second looter's specific `[Settings]` and `[ItemLimits]`.

5. **Run it:** Your second looter uses `/mac turboloot2` instead of `/mac turboloot`, and `/mac TurboKey2 RULE` to tag items into their own INI.

All files live in the same `Macros` (or `Config`) folder - no need for separate MQ installations.

### Auto-Loot with Multiple Looters

If you want the tank to trigger multiple looters, add a separate event for each. In your tank's E3Next INI:

**Under `[EventRegMatches]`:**
```ini
Tloot=slain
Tloot2=slain
```

**Under `[Events]`:**
```ini
Tloot=/timed 10 /if (!${Spawn[npc radius 50].Aggressive} && ${Turbo} && ${SpawnCount[npccorpse radius 50]}) /e3bct LOOTER_ONE /mac turboLoot
Tloot2=/timed 20 /if (!${Spawn[npc radius 50].Aggressive} && ${Turbo} && ${SpawnCount[npccorpse radius 50]}) /e3bct LOOTER_TWO /mac turboLoot2
```

> The second looter uses `/timed 20` (2 seconds) so they don't both try to loot the same corpse at the exact same time. Note the second event runs `/mac turboLoot2` so it uses the second looter's INI and rules.

### If You Only Need One INI

If all your looters should follow the same rules, you don't need any of this. Just use one set of files and point the auto-loot event at whichever character you want doing the looting.

---

## 9. Tips

- **Start simple.** Don't fill out the whole INI upfront if you're overwhelmed. Just set your `lootHighValueMinPP` threshold and use TurboKey to categorize items as you encounter them.
- **Use `/turboreport` first.** Before your first `/turbosell`, run the report to preview what would be sold. Better safe than sorry.
- **`announceDefaultTo=e3bc`** is great for multiboxing - all your characters see loot announcements in the MQ window.
- **`announceSkipTo=gsay`** lets your group see what's being left on corpses, handy for anyone who wants to grab something. Also good to trigger lazbis.
- **`IGNORE` is your friend** for spammy items you never want to see again (Bone Chips in your 50s, etc.).
- **`/turbounload` is the town command.** Park near a banker, tribute master, and vendor, then run it to handle bank -> tribute -> sell -> destroy all at once.
- **`bankWildcards=ON`** and **`sellWildcards=ON`** means spell scrolls, tomes, and skill-ups get auto banked or sold unless you've given them a different rule - great for alts.
