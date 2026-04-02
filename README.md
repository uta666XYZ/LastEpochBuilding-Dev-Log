## Dev Log

## 2026-04-03

- **Maxroll-style Passive Tree UI** (complete, merged to dev):
  - Header with 4 class badges (base + 3 masteries), click to switch tree view
  - Mastery filtering: only show selected mastery's nodes (no zoom/pan needed)
  - Skill unlock progress bar: `passive-slider-bg` background, `progress-fill` gold bar, tick marks (1pt/5pt)
  - `passive-slider-bar` vertical indicator from progress bar to tree (diamond top, scales with aspect ratio)
  - `passive-lock-bar` chain+medallion at 22.5/45 midpoint on unselected mastery bars (disappears when mastery chosen)
  - Level badges (`skill-req-mastery-level`, 38x32 diamond), lock icons (32x32) on locked skills
  - `MASTERY_SKILL_UNLOCKS` data for all 5 classes x 4 trees (20 total), with per-tree skill unlock levels
  - Point caps: base class = 25, mastery = 45 (all classes)
  - 44 PNG sprites in `src/Assets/tree/`
  - CalcOffence.lua nil guard for cost calculation (intermittent crash fix)
- **Tree icon shape masks**: applied hex mask to `-hex` suffix icons, circle mask to others, flat-top hex to `-root` icons. 1300+ sprites processed via `scripts/apply_masks.py`
- **Node frames**: added silver/gold border frames per node type (circle/hex/root) based on allocation state. Frame assets from Maxroll treeIcons → `src/Assets/tree/`
- **Icon size differentiation**: circle 34px (smallest), hex 48px (medium), root 56px (largest). Changed `DrawAsset` from boolean `useFixedSize` to numeric `fixedSize`
- **Badge nodes hidden**: subclass badge icons (`badge_*`) skipped in rendering
- Moved 6 unreferenced sprites to `src/TreeData/bin/`, created `rebuke-hex2-circle.png` for shared passive/skill icon
- Hollowed `sprite_058` center for circle alloc frame transparency

## 2026-04-02

- Fixed stat display color codes in `Global.lua` to match LE game colors:
  - WARD: yellow → blue-white (`^x90C8FF`), ENDURANCE: yellow → green (`^x71E87D`), ARMOUR: white → physical (`^xFCDFC0`), EVASION: green → dex green (`^x9EFF76`)
  - Added converted attribute colors: BRUTALITY, MADNESS, GUILE, APATHY, RAMPANCY
- Added subclass/mastery badges on left side of passive tree (`PassiveTreeView.lua`) at x=-380
- Extended ModParser: attribute conversion mod names (Season 4), "for each of your totems/minions" scope modifiers, "X% chance to gain [BuffName] for N seconds" pattern
- Added `/worktree` command, fixed `scripts/mmr-worktree.ps1` Join-Path 3-argument bug

## 2026-04-01

- **MMR system**: Qwen + Claude Code collaboration. Created `QWEN.md`, `scripts/mmr-review.ps1`, updated `CLAUDE.md`
- **Defense formulas from tunklab.com**: armor mitigation, block mitigation (rating→DR%), dodge chance (evasion→dodge%), ward decay (v1.1 quadratic), corruption/monolith scaling
- Fixed block calculation (rating not percentage), dodge chance (was always 0%), ward decay formula
- Added CalcSections: Block Effectiveness rating, Block Mitigation %, Elemental Armor DR, Ward Decay Per Second
- **Ailment system**: 28 damaging ailments, 10 non-damaging debuffs, 8 resistance shreds with Chance/Duration/DPS-per-Stack
- Witchfire dual-type DPS (Fire + Necrotic), Warlock Overload (4 types), positive buff Config options, stacking buff Config inputs
- 220+ lines CalcSections display entries, 79 ModParser keywords
- Identified Season 4 attribute conversion (Str→Brutality etc.) as unimplemented

## 2026-03-31

- Fixed CalcOffence crash: removed all PoE-specific cost conversion code (Blood Magic, Petrified Blood, ES cost, Rage→Soul, etc.)
- **Idol Altar UI redesign**: moved from equipment slots to Passive tree area, auto-detected layout from baseName, renamed Omen Idol slots to "Fractured 1-6", import auto-populates Fractured slots
- **Combat mechanics audit**: fixed stun threshold formula (now includes Ward + StunAvoidance), fixed "stun avoidance" mod mapping (flat stat not % chance)
- Removed all Energy Shield dead code: 14 files, -175/+45 lines
- **Data mining**: extracted 471 uniques, 1,112 affixes, 898 equipment subtypes, 141 skill trees + 5 passive trees + 1 weaver tree from resources.assets using custom Python parsers
- Compared extracted data against LEB: bases 898/898 match, uniques 471/471 match, affixes 1112/1112 match
- **Tree UI improvements**: replaced procedural dot rendering with game-accurate sprites (dot-N-M.png), connector lines split around dots, removed Weapon Set swap UI, fixed multi-cell idol tooltip on blocked cells
- Matched zoom limits to dev defaults (skill: max 38, passive: max 24)

## 2026-03-30

- **+Skill level system**: global ("+X to All Skills") and per-skill ("+X to [SkillName]") mods now apply. SkillsTab shows teal "+N" suffix. Fixed Cursed Coin Amulet implicit. Fixed 621 broken ModCache entries
- **ModParser improvements**: added channel cost, ward regen, glancing blow, block effectiveness, stun/crit avoidance entries. Fixed "void" substring bug consuming "Stun A**void**ance". Added specialModList patterns. Added CalcSections display for Glancing Blow, Crit Avoidance, Stun Avoidance. Fixed WardRegen→WardPerSecond mapping
- **reqPoints gate system**: nodes requiring minimum parent points now enforced in path-building, visually indicated with dot indicators on connectors. Gated connectors dark brown when unmet. Fixed reqPoints array ordering (JSON reversed). Increased zoom limits
- **Save file format analysis**: decoded item byte array (formatVersion 2), affix encoding, unique/legendary parsing, blessing/lens container mapping. Extended blessing import for containerID 44-45 (empowered monolith extra slots)
- **Buff skill tree nodes fix**: stripped SkillId tags from buff skill tree nodes so mods apply globally when Enabled (affects Enchant Weapon, Holy Aura, Flame Ward, Focus, etc.)
- **Idol/rarity UI**: replaced blocked-cell rendering with game-accurate texture. Implemented LE rarity colors (NORMAL/MAGIC/RARE/UNIQUE/EXALTED/LEGENDARY/SET). Removed 25 PoE color codes. Fixed Exalted rarity detection (0-indexed). Extended affix parsing to 5 slots for sealed affixes
- **Tree data sync**: replaced all 1.4 passive/skill tree data (5 classes). Fixed ~4,200 positions, 89 connections, ~1,567 stat updates, 12 renames. Added 584 missing sprites (0 missing now)
- Fixed idol grid position mapping: game saves bottom-left anchor, UI expects top-left. Now converts with `UI_row = 6 - gameY - idolHeight`

## 2026-03-29

- **Leech parsing**: `+X% [Type] Damage Leeched as Health` now resolves to DamageLifeLeech. Fixed BASE_MORE mod type promotion bug. Replaced PoE ChaosDamage* stat names with LE types (Poison, Necrotic, Void). Removed PoE remnants from CalcOffence/CalcDefence
- Added LE rarity color codes, removed PoE legacy entries (GEM, RELIC, PROPHECY)
- Fixed affix ID decoding, RARE item generated names, altar implicits

## 2026-03-28

- Blessing data updated for Season 4
- Verified and fixed skill tree node data for all 127 skills — 160 corrections applied

## 2026-03-26

- Implemented S4 Idol Altar system with dynamic grid layouts

## 2026-03-25

- Implemented Idol Altar layout
  ![Idol Altar](./assets/ss/idolAltar.png)

## 2026-03-24

- Updated database for Season 4
- Updated Pinnacle boss data

## 2026-03-22

- Updated Idol UI from text to PNG icons
  ![Idol Icon](./assets/ss/idolIcon.png)

## 2026-03-19

- Separated passive tree and skill tree: passive → Tree tab, skill trees → Skills tab
  ![Skill Tree](./assets/ss/skillTree.png)

## 2026-03-05

- Implemented Blessings
  ![Blessings](./assets/ss/blessings.png)

## before 2026-03-04

- Implemented Affix's mod to ModParser and CalcOffense
