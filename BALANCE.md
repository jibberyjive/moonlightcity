# ⚖️ Moonlight City — Progression & Balance Document

> Companion to [DESIGN.md](./DESIGN.md). Covers Zone 1 (The Sewers, floors 1–25) and player levels 1–30.
> All values are shipping-candidate numbers, tuned for a **parent + child co-play audience**: simple to read, forgiving to play, deep enough to optimise.

---

## 0. Core Formulas (the maths everything else assumes)

These four formulas make every number in this document verifiable. Implement these first.

| Formula | Definition |
|---------|------------|
| **Primary attribute** | Blade Dancer & Guardian → **STR**, Rogue → **AGI**, Mage → **INT** |
| **Physical damage** | `(STR × 1.2  +  WeaponATK) × SkillMultiplier × (1 − DEF ÷ (DEF + 60))` |
| **Rogue damage** | `(AGI × 1.0  +  WeaponATK) × SkillMultiplier × (1 − DEF ÷ (DEF + 60))` |
| **Magic damage** | `(INT × 1.5  +  WeaponMATK) × SkillMultiplier × (1 − DEF ÷ (DEF + 120))` |
| **Attack speed** | `1.20 + (AGI × 0.006)` attacks/sec (cap 2.20) |
| **Dodge chance** | `AGI × 0.25%` (cap 35%) |
| **Crit chance** | `5% + (AGI × 0.15%)` (cap 50%), crit = ×1.75 damage |
| **DoT / poison** | Ignores 50% of the target's DEF |

**Design intent:** DEF is a soft mitigation curve, never a wall. At DEF 30 you take 67% damage, at DEF 60 you take 50%, at DEF 120 you take 33%. A child who dumps everything into DEF still deals damage; a child who dumps everything into STR still survives 3 hits.

---

## 1. Player Level Progression — Levels 1 → 30

### 1.1 The XP Curve

**Shape:** Near-quadratic (`XP ≈ 50 + 40(n−1) + 6(n−1)²`, hand-rounded to readable numbers).
**Pacing target:** Levels 1–10 arrive fast (dopamine ramp for a young player), 11–20 track the Sewers 1:1, 21–30 are the "overflow" band that carries into Zone 2.

| Lv | XP to Next | Total XP | Stat Pts | Unlock |
|----|-----------:|---------:|:--------:|--------|
| 1  | 50     | 0      | — | **Skill 1** (class starter) + Basic Attack + Dodge |
| 2  | 100    | 50     | 3 | — |
| 3  | 160    | 150    | 3 | Equipment slots: Weapon + Armour |
| 4  | 230    | 310    | 3 | **Skill 2** |
| 5  | 310    | 540    | 3 **+3** | **Class Trait I** (passive) |
| 6  | 400    | 850    | 3 | Accessory slot unlocked |
| 7  | 500    | 1,250  | 3 | — |
| 8  | 620    | 1,750  | 3 | **Skill 3** |
| 9  | 750    | 2,370  | 3 | — |
| 10 | 900    | 3,120  | 3 **+3** | **Class Trait II** + **Technique slot** (charged moves) |
| 11 | 1,070  | 4,020  | 3 | — |
| 12 | 1,260  | 5,090  | 3 | **Skill 4** (class passive) |
| 13 | 1,470  | 6,350  | 3 | — |
| 14 | 1,700  | 7,820  | 3 | — |
| 15 | 1,950  | 9,520  | 3 **+3** | **Class Trait III** + 2nd Accessory slot |
| 16 | 2,220  | 11,470 | 3 | **Skill 5** |
| 17 | 2,520  | 13,690 | 3 | *Recommended Slime King level* |
| 18 | 2,840  | 16,210 | 3 | — |
| 19 | 3,200  | 19,050 | 3 | — |
| 20 | 3,600  | 22,250 | 3 **+3** | **Skill 6 — ULTIMATE** |
| 21 | 4,050  | 25,850 | 3 **+1 SR** | Skill Ranks unlock (see 1.4) |
| 22 | 4,550  | 29,900 | 3 +1 SR | — |
| 23 | 5,100  | 34,450 | 3 +1 SR | — |
| 24 | 5,700  | 39,550 | 3 +1 SR | — |
| 25 | 6,400  | 45,250 | 3 **+3** +1 SR | **Class Trait IV** — Zone 2 gear tier equippable |
| 26 | 7,150  | 51,650 | 3 +1 SR | — |
| 27 | 8,000  | 58,800 | 3 +1 SR | — |
| 28 | 8,900  | 66,800 | 3 +1 SR | — |
| 29 | 9,900  | 75,700 | 3 +1 SR | — |
| 30 | —      | 85,600 | 3 **+3** +1 SR | **Zone 1 Mastery** — title, +5% gold in Sewers |

**Stat points** are spent only on **STR / AGI / INT / DEF** (1 point = +1). HP and MP grow automatically by class — this keeps the choice simple for a child (4 buttons, no "wasted" HP investment) while preserving class identity.

### 1.2 XP Income — where the levels come from

| Floors | XP per enemy | Enemies/floor | First-clear bonus | **Floor total** |
|--------|-------------:|--------------:|------------------:|----------------:|
| 1–5    | 5–11   | 10–12 | 25–85    | **75–255** |
| 6–10   | 13–22  | 12–14 | 100–160  | **300–480** |
| 11–15  | 24–33  | 13–15 | 175–235  | **525–705** |
| 16–20  | 35–45  | 14–16 | 250–310  | **750–930** |
| 21–24  | 47–56  | 15–18 | 325–370  | **975–1,110** |
| 25 (Boss) | — | — | — | **3,000** |

Formula: `FloorXP(f) = 30 + 45f`. **Cumulative floors 1–24 = 14,220 XP → level 17–18 at the boss door.**

**Anti-grind rule (critical):** re-clearing an already-cleared floor pays **50%** on the 2nd clear and **25%** on the 3rd+ clear, resetting daily. Grinding floor 12 forever is strictly worse than descending. Quests and side rooms are the intended top-up.

| Route to Floor 25 | Expected Level |
|-------------------|:--------------:|
| Speedrun — skip side rooms, skip quests | **15** (floor) |
| Normal — clear floors, do bulletin quests | **17–18** (target) |
| Completionist — all hidden rooms + all quests | **20** (ceiling) |

This lands exactly on the design brief's 15–20 window without any forced repetition.

### 1.3 Stat Growth Per Level (automatic, by class)

| Class | HP/lv | MP/lv | STR/lv | AGI/lv | INT/lv | DEF/lv |
|-------|------:|------:|-------:|-------:|-------:|-------:|
| ⚔️ Blade Dancer | +16 | +4 | +2 | +2 | +1 per 3 lv | +1 |
| 🔮 Mage        | +10 | +8 | +1 per 3 lv | +1 | +3 | +1 per 2 lv |
| 🛡️ Guardian    | +22 | +2 | +1 | +1 per 2 lv | +1 per 3 lv | +2 |
| 🗡️ Rogue       | +12 | +4 | +1 | +3 | +1 per 3 lv | +1 |

**Level 1 base stats**

| Class | HP | MP | STR | AGI | INT | DEF |
|-------|---:|---:|----:|----:|----:|----:|
| ⚔️ Blade Dancer | 120 | 30 | 8 | 8 | 4 | 5 |
| 🔮 Mage | 90 | 60 | 4 | 5 | 10 | 3 |
| 🛡️ Guardian | 150 | 25 | 7 | 4 | 3 | 9 |
| 🗡️ Rogue | 100 | 35 | 6 | 10 | 4 | 4 |

**Benchmark totals (automatic growth only — before spending stat points)**

| Class | Lv 5 | Lv 10 | Lv 15 | Lv 20 | Lv 25 | Lv 30 |
|-------|------|-------|-------|-------|-------|-------|
| ⚔️ Blade Dancer | 184 HP / 16 STR / 16 AGI / 9 DEF | 264 / 26 / 26 / 14 | 344 / 36 / 36 / 19 | 424 / 46 / 46 / 24 | 504 / 56 / 56 / 29 | 584 / 66 / 66 / 34 |
| 🔮 Mage | 130 HP / 92 MP / 22 INT / 5 DEF | 180 / 132 / 37 / 7 | 230 / 172 / 52 / 10 | 280 / 212 / 67 / 12 | 330 / 252 / 82 / 15 | 380 / 292 / 97 / 17 |
| 🛡️ Guardian | 238 HP / 11 STR / 17 DEF | 348 / 16 / 27 | 458 / 21 / 37 | 568 / 26 / 47 | 678 / 31 / 57 | 788 / 36 / 67 |
| 🗡️ Rogue | 148 HP / 22 AGI / 8 DEF | 208 / 37 / 13 | 268 / 52 / 18 | 328 / 67 / 23 | 388 / 82 / 28 | 448 / 97 / 33 |

**Cumulative free stat points:** Lv 5 = 15 · Lv 10 = 33 · Lv 15 = 51 · Lv 20 = 69 · Lv 25 = 87 · Lv 30 = 105.

### 1.4 Skill Ranks (levels 21–30 — depth without new buttons)

From level 21, each level grants **1 Skill Rank point**. Every skill has **3 ranks**; ranks cost 1 / 2 / 3 points. Ten levels = 10 points = roughly three skills fully maxed, or six skills at rank 2. This is the "depth" layer for the parent, and is entirely ignorable by the child (auto-assign button available).

| Rank | Effect |
|------|--------|
| I (default) | Base values |
| II (1 pt) | +20% damage/healing, −10% cooldown |
| III (3 pts total) | +45% damage/healing, −20% cooldown, **unlocks the skill's bonus rider** (listed per skill in §5) |

---

## 2. Zone 1 Boss — 👑 The Slime King (Floor 25)

> *"From the deepest filth of the sewers, a consciousness has formed — ancient, patient, and absolutely disgusting."*

### 2.1 Core Statline

| Attribute | Value | Note |
|-----------|------:|------|
| Boss Level | 20 | |
| **Total HP** | **18,000** | Split 40 / 30 / 30 across phases |
| DEF | 30 (P1) → 36 (P2) → **22 (P3)** | DEF *drops* in phase 3 — the reward for surviving |
| Move speed | 2.0 → 2.6 → 3.0 tiles/s | Player base = 3.4 tiles/s (you can always outrun him) |
| Arena | 20 × 16 tiles, 4 pillars, 2 side alcoves (safe from puddles) | |
| Status immunity | Poison, Slow (all phases) · Stun/Knockback (Phase 3 only) | Stuns work in P1/P2 — Guardian and Blade Dancer feel strong early |
| Target fight length | **2:30 – 4:00** | 4:00 at Lv 15, 2:30 at Lv 17, 1:45 at Lv 20 with rare gear |
| Enrage | After **8:00**, +40% boss damage | Failsafe only — a soft "you've stalled" nudge, not a wipe timer |

### 2.2 Phase Breakdown

#### Phase 1 — "The Wallowing" (18,000 → 10,800 HP · 7,200 HP)

| Attack | Damage | Telegraph | Cooldown | Notes |
|--------|-------:|----------:|---------:|-------|
| Slime Ball Throw | 45 | 0.8 s (arc shadow on ground) | 3 s | 2 balls, 1-tile splash |
| Body Stomp | 70 | 1.0 s (he rears up) | 8 s | 3-tile radius shockwave |
| Summon Small Slimes | contact 20 | — | 20 s | 3 slimes, HP 120, cap 6 alive |

**Teaching goal:** telegraph reading. Every attack has a ≥0.8 s wind-up and a floor decal.

#### Phase 2 — "The Swelling" (10,800 → 5,400 HP · 5,400 HP)

Transition: 4 s **vulnerable stagger** (takes **+25% damage**) as he inflates. Rewards phase-watching.

| Attack | Damage | Telegraph | Cooldown | Notes |
|--------|-------:|----------:|---------:|-------|
| Slime Ball Volley | 50 ×4 | 0.9 s | 5 s | Fan pattern, gaps between balls |
| **Charge** | 110 | 1.2 s (he squishes flat) | 12 s | 6-tile dash, leaves slime trail (12 dmg/s for 8 s) |
| Body Stomp (upgraded) | 85 | 1.0 s | 7 s | 4-tile radius |
| Summon Toxic Slimes | contact 28 + poison 10/s for 4 s | — | 18 s | 3 slimes, HP 180, cap 6 |

**Teaching goal:** movement under pressure + "kill the adds or drown".

#### Phase 3 — "The Berserk Crown" (5,400 → 0 HP)

Transition: 5 s vulnerable stagger (**+25% damage**), crown cracks, DEF drops 36 → 22.

| Attack | Damage | Telegraph | Cooldown | Notes |
|--------|-------:|----------:|---------:|-------|
| Rapid Slam ×3 | 90 each | 0.9 s then 0.6 s between slams | 9 s | Dodgeable as a rhythm — 3 dashes |
| Slime Eruption | 15/s standing | 1.5 s (bubbling floor decals) | 15 s | 6 puddles, 8 s duration, alcoves stay clean |
| **Crown Toss** | 130 (hits on throw **and** return) | 1.1 s | 20 s | Signature move — sidestep once, twice |
| Berserk aura | passive | — | — | Attack speed +35%, no new summons |

**Design note:** phase 3 deliberately has *fewer* mechanics but *faster* ones, and the boss is squishier. The fight ends on a high, not a slog.

### 2.3 Recommended Player Stats at Floor 25

Target: level **17**, one Uncommon/Rare weapon, full Tier-2 armour.

| Class | HP | MP | STR | AGI | INT | DEF | Weapon | Survives Crown Toss? |
|-------|---:|---:|----:|----:|----:|----:|-------:|---|
| ⚔️ Blade Dancer | 376 | 94 | 67 | 58 | 9 | 33 | ATK 45 | 83 dmg → **4 hits** |
| 🔮 Mage | 250 | 188 | 9 | 30 | 92 | 25 | MATK 48 | 92 dmg → **2 hits** ⚠️ use Arcane Shield |
| 🛡️ Guardian | 502 | 57 | 51 | 22 | 8 | 60 | ATK 40 | 65 dmg → **7 hits** |
| 🗡️ Rogue | 292 | 99 | 36 | 90 | 9 | 31 | ATK 42 | 85 dmg → **3 hits** (22% dodge) |

**Expected sustained DPS (with skills, realistic uptime):** Blade Dancer ~117 · Mage ~110 · Guardian ~102 · Rogue ~110. All four clear 18,000 HP in 2:30–3:00. **No class is gated out of the fight.**

**Recommended loadout:** 5 × Medium Potion, 2 × Antidote (Phase 2 poison), 1 × Revive Charm (≈475 gold total — an affordable, not mandatory, safety net).

**Accessibility valve:** an optional **"Family Mode"** toggle in Settings reduces boss damage by 35% and grants a 3 s invulnerability after being hit. It does **not** reduce rewards. A 7-year-old should never be hard-blocked from the Slime Crown.

### 2.4 Boss Rewards

| Reward | Value |
|--------|-------|
| Gold | **2,200** (+1,500 first-clear bonus) |
| XP | **3,000** |
| Guaranteed | 1 × **Rare** equipment (Tier 4 chest), 3 × Kingslime Gel (sells 60 each) |
| 15% chance | 1 × **Legendary** equipment |
| 100% cosmetic | 👑 **The Slime Crown** (wearable headpiece) |
| Key item | 🔑 Crypt Key — unlocks Zone 2 |

---

## 3. Gold Economy

### 3.1 Income by Floor Range

`FloorGold(f) = 15 + 10f` from enemies + breakables, plus hidden-chest rolls.

| Floors | Gold/enemy | Gold/floor clear | Hidden chest (60% spawn) | Net gold/hour* |
|--------|-----------:|-----------------:|-------------------------:|---------------:|
| 1–5    | 3–8   | 25–65   | 40–80   | ~900 |
| 6–10   | 8–15  | 70–115  | 80–150  | ~1,700 |
| 11–15  | 15–25 | 120–165 | 150–250 | ~2,500 |
| 16–20  | 25–40 | 170–215 | 250–400 | ~3,000 |
| 21–24  | 40–60 | 220–255 | 400–600 | ~3,700 |
| 25     | — | **2,200** | Boss chest | — |

\* *Net of expected potion spend. Assumes 3–5 min per floor, includes selling vendor trash.*

### 3.2 Full Zone 1 Income Budget

| Source | Total |
|--------|------:|
| Floor gold, floors 1–24 | 3,360 |
| Hidden chests (~14 found) | ~1,200 |
| Selling materials & unwanted gear | ~1,900 |
| Bulletin-board quests (~16 @ 120–400) | ~3,200 |
| Slime King + first-clear | 3,700 |
| **Gross total** | **≈ 13,400** |
| Expected consumable/gear spend | −4,500 |
| **Net savings across Zone 1** | **≈ 8,900** |

### 3.3 Shop Prices

**🧪 Downtown Market — Consumables**

| Item | Price | Effect | Sells for |
|------|------:|--------|----------:|
| Small Potion | 25 | Heal 80 HP | 8 |
| Medium Potion | 70 | Heal 200 HP | 22 |
| Large Potion | 180 | Heal 450 HP | 55 |
| Mana Vial | 40 | Restore 60 MP | 12 |
| Greater Mana Vial | 110 | Restore 160 MP | 35 |
| Antidote | 30 | Cure poison, 60 s immunity | 9 |
| Revive Charm | 250 | Auto-revive once at 40% HP | 75 |
| Torch Bundle | 15 | Reveals hidden rooms for 5 min | 4 |
| Slice of Moon Cake | 60 | +10% XP for 30 min | — |
| Fish Stew | 45 | +50 max HP for 20 min | — |

**⚔️ Weapon & Armour Shop** *(shop stock is deliberately one tier behind dungeon drops — the dungeon is always the better source, the shop is the catch-up safety net)*

| Tier | Weapon | Armour | Accessory |
|------|-------:|-------:|----------:|
| Common (Lv 1–8) | 120–300 | 100–260 | 90–200 |
| Uncommon (Lv 9–16) | 400–900 | 350–800 | 300–650 |
| Rare (Lv 17–24) | 1,200–2,500 | 1,000–2,200 | 900–1,800 |
| Legendary | *not sold* | *not sold* | Black Market only: 6,000 |

**🪑 Furniture Shop**

| Item | Price | | Item | Price |
|------|------:|-|------|------:|
| Wooden Chair | 40 | | Bookshelf | 200 |
| Small Table | 90 | | Cosy Rug | 120 |
| Simple Bed | 250 | | Wall Lantern | 80 |
| Storage Chest | 300 | | Fancy Sofa | 600 |
| Kitchen Counter | 380 | | Magic Light Orb | 900 |
| Wallpaper / Flooring (per room) | 150 | | Grand Fireplace | 1,400 |

**👗 Clothier:** everyday clothing 80–250 · statement pieces 300–600 · seasonal/event 700–1,200 (cosmetic only, zero stats — never pay-to-win over a sibling).

### 3.4 Housing — the long-term gold sink

| Tier | Cost | Rooms | Unlocks at | Time to afford |
|------|-----:|-------|------------|----------------|
| 1 · Starter Shack | **500** | 1 | Floor 3 / first 3 quests | ~25 min |
| 2 · Cottage | **3,500** | 2 + yard | Floors 8–10 | ~1.5 h cumulative |
| 3 · Townhouse | **9,000** | 3 + upstairs | Floors 18–22 | ~3.5 h cumulative |
| 4 · Manor | **30,000** | Full house + garden + basement | Zone 2 (floors 26–40) | Post-Zone 1 goal |

**The pacing story:** you own a Shack within the first session. The Cottage lands right when the Sewers get properly dangerous (floor ~10). The Townhouse is a realistic Zone 1 finish-line prize *if* you save. The Manor is deliberately out of reach — it's the reason to open the Crypt Key.

### 3.5 Anti-Grind Guarantees

1. **Diminishing repeat clears** — 50% then 25% gold/XP on re-runs (daily reset).
2. **First-clear bonuses** are the biggest single payout per floor — descending always beats farming.
3. **Quests are uncapped-value, capped-frequency** — 4–6 fresh bulletin quests daily, 120–400 gold each. A player who only has 20 minutes gets meaningful progress without a dungeon run.
4. **Vendor trash auto-stacks and sells in one click** — no inventory chores.
5. **No consumable is mandatory.** Every fight is winnable at target level with zero potions; potions buy comfort, not victory.

---

## 4. Zone 1 Loot Table — The Sewers

### 4.1 Drop Sources

| Source | Equipment drop chance | Gold | Materials |
|--------|----------------------:|------|-----------|
| Normal enemy | 6% | Always | 45% |
| Giant Slime (mini-boss, floors 20–25) | 25% | Always (×4) | 100% |
| Barrel / Crate | 4% | 60% | 30% |
| Hidden-room Chest | **100%** | Always | 100% |
| Slime King | **100% Rare** + 15% Legendary | 2,200 | 100% |

### 4.2 Rarity Distribution by Depth

| Floors | ⬜ Common | 🟩 Uncommon | 🟦 Rare | 🟨 Legendary |
|--------|---------:|------------:|--------:|-------------:|
| 1–8 (Tier 1) | 75% | 22% | 3% | — |
| 9–16 (Tier 2) | 62% | 30% | 7.5% | 0.5% |
| 17–24 (Tier 3) | 48% | 37% | 13.5% | 1.5% |
| 25 Boss chest (Tier 4) | — | — | 85% | 15% |

**Rarity power multiplier:** Common ×1.00 · Uncommon ×1.35 (+1 affix) · Rare ×1.80 (+2 affixes) · Legendary ×2.60 (+3 affixes **and** a unique effect).

### 4.3 Weapons

| Tier / Floors | Rarity | Item Name | ATK / MATK | Bonus |
|---------------|--------|-----------|-----------:|-------|
| T1 · 1–8 | ⬜ Common | Rusty Pipe | 8 | — |
| T1 | ⬜ Common | Cracked Apprentice Wand | 7 MATK | +5 MP |
| T1 | 🟩 Uncommon | Ratcatcher's Dagger | 11 | +2 AGI |
| T1 | 🟩 Uncommon | Drainkeeper's Shortsword | 12 | +2 STR |
| T1 | 🟦 Rare | **Gutter Fang** | 15 | +3 AGI, +2 STR, +5% crit |
| T2 · 9–16 | ⬜ Common | Sewer Cleaver | 20 | — |
| T2 | ⬜ Common | Pipewood Staff | 18 MATK | +10 MP |
| T2 | 🟩 Uncommon | Drainblade | 27 | +4 STR |
| T2 | 🟩 Uncommon | Ratfang Kris | 25 | +5 AGI |
| T2 | 🟦 Rare | **Tidecaller Staff** | 36 MATK | +8 INT, +15 MP, spells slow 15% for 2 s |
| T2 | 🟦 Rare | **Bulwark Hammer** | 33 | +5 STR, +4 DEF, Shield Bash stun +0.5 s |
| T3 · 17–24 | ⬜ Common | Ironpipe Maul | 34 | — |
| T3 | 🟩 Uncommon | Spider-Silk Rapier | 45 | +6 AGI, +5% crit |
| T3 | 🟩 Uncommon | Toxinbrand | 43 | +5 STR, attacks apply weak poison (5/s, 3 s) |
| T3 | 🟦 Rare | **Kingslime Cleaver** | 60 | +8 STR, +3 DEF, +12% damage vs slimes |
| T3 | 🟦 Rare | **Web-Weaver Focus** | 58 MATK | +11 INT, −8% spell cooldowns |
| T4 · Boss | 🟨 **Legendary** | **Sovereign's Ooze Blade** | 78 | +12 STR, +6 AGI, +4 DEF · *8% on-hit: slow target 30% for 2 s* |
| T4 | 🟨 **Legendary** | **Sceptre of the Drowned Crown** | 74 MATK | +14 INT, +40 MP, +3 DEF · *every 6th spell splits into 2* |

### 4.4 Armour

| Tier | Rarity | Item Name | DEF | Bonus |
|------|--------|-----------|----:|-------|
| T1 | ⬜ Common | Patched Tunic | 4 | — |
| T1 | 🟩 Uncommon | Ratskin Vest | 7 | +12 HP |
| T1 | 🟦 Rare | **Lanternkeeper's Coat** | 10 | +20 HP, +2 AGI, small light radius |
| T2 | ⬜ Common | Sewer Guard Mail | 12 | — |
| T2 | 🟩 Uncommon | Slimeproof Jacket | 17 | +30 HP, −20% slime puddle damage |
| T2 | 🟦 Rare | **Pipewarden Plate** | 24 | +55 HP, +4 DEF, −10% stun duration |
| T3 | ⬜ Common | Ironscale Vest | 22 | — |
| T3 | 🟩 Uncommon | Silkweave Robe | 26 | +45 HP, +25 MP, +5 INT |
| T3 | 🟦 Rare | **Giant Slime Carapace** | 38 | +90 HP, +6 DEF, reflects 8% contact damage |
| T4 | 🟨 **Legendary** | **Regalia of the Slime King** | 52 | +150 HP, +8 DEF, +4 all stats · *below 30% HP, gain 25% damage reduction* |

### 4.5 Accessories

| Tier | Rarity | Item Name | Bonus |
|------|--------|-----------|-------|
| T1 | ⬜ Common | Copper Band | +1 to one random stat |
| T1 | 🟩 Uncommon | Ratbone Charm | +3 AGI, +5% gold from enemies |
| T2 | 🟩 Uncommon | Sewer Diver's Amulet | +25 HP, poison duration −30% |
| T2 | 🟦 Rare | **Drowned Sailor's Locket** | +6 DEF, +20 MP, regen 2 HP/s out of combat |
| T3 | 🟦 Rare | **Spider's Eye Pendant** | +8 AGI, +8% crit, see hidden rooms within 6 tiles |
| T4 | 🟨 **Legendary** | 👑 **The Slime Crown** *(cosmetic)* | Pure cosmetic. +10% gold in the Sewers as a keepsake bonus. |

### 4.6 Affix Pool (rolled on Uncommon+)

| Affix | Common Roll | Rare Roll |
|-------|------------:|----------:|
| of the Rat | +2–4 AGI | +6–9 AGI |
| of the Ox | +2–4 STR | +6–9 STR |
| of the Scholar | +2–4 INT | +6–9 INT |
| of the Wall | +2–4 DEF | +6–9 DEF |
| of Vitality | +15–30 HP | +50–90 HP |
| of the Wellspring | +10–20 MP | +30–55 MP |
| Keen | +3% crit | +8% crit |
| Swift | +4% attack speed | +9% attack speed |
| Greedy | +5% gold | +12% gold |
| Learned | +4% XP | +9% XP |

### 4.7 Materials & Vendor Trash

| Material | Source | Sell | Future crafting use |
|----------|--------|-----:|---------------------|
| Slime Gel | Slimes, floors 1–20 | 6 | Potions, sticky traps |
| Rat Tail | Sewer Rats | 4 | Cheap accessories |
| Cracked Pipe Segment | Barrels, breakables | 10 | Furniture, weapon hafts |
| Lantern Oil | Side rooms | 14 | Light sources, burn coating |
| Spider Silk | Pipe Spiders, floors 15–25 | 18 | Robes, rope, silkweave gear |
| Toxic Sac | Toxic Slimes | 22 | Poison Blade upgrades, antidotes |
| Kingslime Gel | Slime King only | 60 | Zone 2 gear unlock recipe |

---

## 5. Class Skill Trees

Every class unlocks **6 skills at levels 1, 4, 8, 12, 16, 20** — the same cadence, so a parent and child levelling together always unlock together. Slot 4 is always a **passive** (a "free win" that requires no extra button). Slot 6 is always the **ultimate**.

Class Traits (levels 5/10/15/25) are automatic passives listed under each class.

---

### ⚔️ Blade Dancer — *fast melee, combo momentum*

| Lv | Skill | Cost / CD | Effect | Rank III rider |
|----|-------|-----------|--------|----------------|
| 1 | **Spin Slash** | 6 MP / 4 s | 140% damage to all enemies within 2 tiles | Knocks enemies back 1 tile |
| 4 | **Shadow Step** | 8 MP / 8 s | Dash 4 tiles through enemies; next attack +50% damage | Leaves an afterimage that taunts for 3 s |
| 8 | **Twin Fang** | 10 MP / 6 s | Two strikes, 90% each; 2nd auto-crits if target is below 50% HP | 3rd strike added |
| 12 | **Rhythm** *(passive)* | — | Each hit within 2 s of the last: +4% attack speed, stacks 5× | Stacks 8×, +3% damage per stack |
| 16 | **Moonlight Flourish** | 18 MP / 14 s | 3-hit combo, 120% each; heals 5% max HP per enemy hit | Final hit hits in a full circle |
| 20 | **⭐ Blade Storm** | 30 MP / 45 s | Spin for 3 s, 60% damage every 0.4 s, immune to knockback | Duration 4.5 s, move at full speed while spinning |

**Traits:** L5 *Light Feet* — dodge cooldown −20% · L10 *Momentum* — +8% damage after a dodge for 3 s · L15 *Duellist* — +10% crit vs single targets · L25 *Dance of Blades* — every 5th basic attack is free (no MP, ignores 20% DEF).

---

### 🔮 Mage — *ranged burst, resource management*

| Lv | Skill | Cost / CD | Effect | Rank III rider |
|----|-------|-----------|--------|----------------|
| 1 | **Fireball** | 8 MP / 2 s | 150% magic damage, burns for 10% over 3 s | Explodes for 60% in a 1-tile radius |
| 4 | **Frost Nova** | 12 MP / 10 s | 110% damage in 3 tiles, slows 40% for 4 s | Freezes solid for 1 s |
| 8 | **Arcane Shield** | 14 MP / 18 s | Absorbs damage equal to 3 × INT for 8 s | Reflects 20% of absorbed damage on break |
| 12 | **Mana Well** *(passive)* | — | +30% MP regen; every 5th spell costs 0 MP | Every 4th spell is free |
| 16 | **Chain Lightning** | 20 MP / 12 s | 130% damage, jumps to 3 targets, −20% per jump | 5 targets, no damage falloff |
| 20 | **⭐ Arcane Surge** | 35 MP / 50 s | For 10 s: +50% spell power, −50% cooldowns | +14 s duration and MP costs halved |

**Traits:** L5 *Focus* — casting no longer slows movement · L10 *Elemental Weave* — alternating fire/frost spells deal +12% · L15 *Ward* — taking a killing blow leaves you at 1 HP once per floor · L25 *Overflow* — spending MP above 80% grants +15% spell power.

---

### 🛡️ Guardian — *tank, control, protection*

| Lv | Skill | Cost / CD | Effect | Rank III rider |
|----|-------|-----------|--------|----------------|
| 1 | **Shield Bash** | 6 MP / 5 s | 100% damage + 1 s stun | Stun 1.8 s, +50% damage to stunned targets |
| 4 | **Provoke** | 5 MP / 12 s | Forces enemies within 4 tiles to attack you for 6 s; +20% DEF while active | Provoked enemies deal 15% less damage |
| 8 | **Iron Wall** | 12 MP / 20 s | −60% damage taken for 5 s (move speed −30%) | Immune to stun/knockback during |
| 12 | **Bulwark** *(passive)* | — | DEF also grants 0.5 × its value as max HP; reflect 15% of mitigated damage | Reflect 30%, +5% DEF |
| 16 | **Guardian's Oath** | 16 MP / 25 s | Barrier absorbing 4 × DEF; heals 10% max HP when it expires | Barrier explodes for 150% DEF damage on break |
| 20 | **⭐ Earthshaker** | 28 MP / 45 s | Slam for 200% STR + 100% DEF in 4 tiles, knocks down 2 s | 6-tile radius, leaves a damaging fissure for 5 s |

**Traits:** L5 *Sturdy* — 15% less damage from enemies above you in level · L10 *Second Wind* — below 30% HP, regen 2% max HP/s · L15 *Unmovable* — immune to slow effects · L25 *Aegis* — the first hit of every fight is fully blocked.

---

### 🗡️ Rogue — *stealth, poison, crit burst*

| Lv | Skill | Cost / CD | Effect | Rank III rider |
|----|-------|-----------|--------|----------------|
| 1 | **Backstab** | 6 MP / 6 s | 130% damage; **260%** if striking from behind | Behind-hits reset the cooldown |
| 4 | **Smoke Bomb** | 10 MP / 16 s | Stealth 4 s, +40% dodge; exiting stealth guarantees a crit | Blinds enemies in 3 tiles for 3 s |
| 8 | **Poison Blade** | 8 MP / 10 s | Coats weapon 12 s: hits apply 8% AGI/s for 5 s, stacks 3× | Stacks 5×, poison spreads on target death |
| 12 | **Opportunist** *(passive)* | — | +15% crit vs enemies below 40% HP; crits restore 4 MP | +25% crit, crits restore 8 MP |
| 16 | **Fan of Knives** | 18 MP / 14 s | 5 knives in a cone, 70% each, applies Poison Blade | 8 knives, 360° spread |
| 20 | **⭐ Shadow Dance** | 30 MP / 50 s | 6 s: +60% attack speed, free dodges, attacks ignore 50% DEF | 9 s, and the first kill during it refreshes the duration |

**Traits:** L5 *Light Step* — no damage from slime puddles · L10 *Cutpurse* — +10% gold from enemies you kill with a crit · L15 *Evasion* — dodging restores 5 MP · L25 *Assassin* — +20% damage to enemies at full HP.

---

## 6. Balance Verification Checklist

Use this as the Phase 8 acceptance test.

| Check | Target | Status |
|-------|--------|:------:|
| Level at floor 25 (normal play, no repeats) | 17–18 | ☐ |
| Level at floor 25 (rush, no quests) | ≥ 15 | ☐ |
| Slime King fight duration, Lv 17, each class | 2:30–3:30 | ☐ |
| Every class survives ≥ 3 Crown Tosses at Lv 17 | true | ☐ |
| Cottage affordable by floor 10 without selling gear | true | ☐ |
| Townhouse affordable by floor 24 if saving | true | ☐ |
| Repeat-clearing floor N is slower than descending | true | ☐ |
| A player who buys zero potions can still beat floor 25 | true | ☐ |
| Total Zone 1 playtime (dungeon + city) | 5–7 hours | ☐ |
| No single stat allocation is more than 25% ahead of another | true | ☐ |

---

*Moonlight City — progression tuned so a 7-year-old can win and a 37-year-old can optimise.* 🌙
