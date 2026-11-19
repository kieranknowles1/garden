---
categories:
  - "[[Games]]"
cover: "[[Skyrim.jpg]]"
dg-publish: true
genres:
  - "[[Genres#RPG]]"
  - "[[Genres#Fantasy]]"
---
> Just one more mod
![[Category Listing.base]]
# Characters & Hijinks
## Lydia
Once, after being dismissed at the College of Winterhold, got stuck behind an open gate for weeks.
## Takes-Your-Stuff
The name says it all, pure chaotic lawful. First playthrough with [Conquest of Skyrim](https://www.nexusmods.com/skyrimspecialedition/mods/65628). Once fought two battles at the same time by constantly teleporting between them. Over two days, the rule of Whiterun went:

1. Empire, under Jarl Balgruuf.
2. Undead Army after Jarl Balgruuf was overthrown and replaced by Jarl Balgruuf.
3. Empire, after they retook the city and appointed Jarl Balgruuf when Takes-Your-Stuff forgot to assign a garrison.
4. Undead Army retook the city and assigned Balgruuf as Jarl, this time with a garrison.

Once got caught stealing and had everything confiscated, then ran stark naked through Windhelm to steal the evidence chest along with all her stolen possessions before

# Console Command Cheatsheet
## Clear Bounty
Run with guard selected in console.
```
paycrimegold 0 0
player.additem f <fine_amount>
```
## Show Limited Race Menu
```
player.equipitem Dirty_NewScissors
```

# Cursed Knowledge
- `IsCurrentFurnitureRef` crashes if used on a disabled actor.
- Most of what [Antigen](https://github.com/mutagen-Modding/antigen) checks for.
# Troubleshooting
If you're not tinkering with the load order, you're doing this, especially on Linux.
See also: [[Skyrim Modder's Book#The Wierd and Wonderful World of Woefully Wacky Bugs]].
## 🏳️‍🌈 Everything's a Rainbow 🏳️‍🌈
Symptom: Most materials appear rainbow coloured, e.g., grass. 🏳️‍🌈

Solution: Disable community shaders (`End` for menu), clear disk and shader cache, then reenable shaders. 🏳️‍🌈

## Alchemy Borked With Apothecary
Symptom: With Apothecary installed, all player-made potions are extremely powerful, easily 5x what they should be. No idea which mod causes it can can't be bothered to do a bisect.

## CK Platform Extended Not Loading
Symptom: [Creation Kit Platform Extended](https://www.nexusmods.com/skyrimspecialedition/mods/71371) silently fails to load, causing slow load times and the inability to save with ESPs as masters.

Solution: Add `WINEDLLOVERRIDES="winhttp.dll=n,b;d3d11.dll=b,n" %command%` to Steam's launch options. If all text is black, then set `bUIDarkTheme` to false in the INI.

## Creatures Chase Indefinitely
Symptom: Creatures will sometimes act extremely aggressively., continuing to chase even after their cell is unloaded.

Solution: Run `player.SCAOnActor` to reset all attackers. This doesn't stop them from attacking again when close.

## Interiors Black
Symptom: Certain interior cells render as solid black, even when holding a torch.

Solution: In xEdit, ensure the cell's fog near/far is not set to zero by enabling `Lighting → Inherits → Fog Power, Fog Max`

## No Lighting
Symptom: No interior or held lights have any effect.

Solution: Delete the `ShaderCache` directory. Note that this will be placed inside mod config and not overwrite.

Should probably do this after every update. Seems closely linked to [[#🏳️‍🌈 Everything's a Rainbow 🏳️‍🌈]]

## NPCs Swim When Running
Symptom: NPCs (observed with Xelzaz and Sa'chil) will appear to swim when running.

Solution: Set the NPC's race to `WerewolfBeastRace` and back again to reset its animations.

## Random Townspeople Hostile
Symptom: Random townspeople turn hostile for no reason, due to their crime faction being an enemy of the player.

Solution: `SetPCEnemyOfFaction CrimeFaction<faction> 0`

## Shaders Not Compiling
Symptom: Community shaders, or other mods that require shader compilation, will report that all of its shaders fail to compile.

Solution: Copy `d3dcompiler_47.dll` from `modorganizer2/dlls` to the same directory as `SkyrimSE.exe`

## Some SKSE Plugins Fail to Load
Symptom: Specific SKSE plugins are not loading, specifically including `DynamicActivationKey.dll`, `FormListManipulator.dll`, and `OpeanAnimationReplacer.dll`

Latest private profiler redirector does not load, which makes it incompatible with community shaders, load time is bearable but want to fix this if possible.

Solution: Reinstall [MSVC Redist](https://aka.ms/vs/17/release/vc_redist.x64.exe), run the installer through the SkyrimSE Wine prefix.

## Story Manager Quests Not Triggering
Sometimes story manager quests, such as brawls, won't trigger. This is very inconsistent, but seems to fix itself when changing location. It may also have something to do with the current cell.

Don't have a reliable fix, so just documenting when and where it happens to look for a pattern.

| Action                | Cell                | Event Type | Works?                           |
| --------------------- | ------------------- | ---------- | -------------------------------- |
| Carlotta-Mikael favor | Whiterun worldspace | Script     | Fight started after cell switch. |

## Synthesis Crashes on Startup
Symptom: Synthesis crashes on startup, with the log indicating a missing dotnet executable.

Solution: Much like [[#Some SKSE Plugins Fail to Load]], install the .NET SDK to Skyrim's wine prefix.

## Tree LOD Shows Entire Atlas
Symptom: Tree billboards will show their entire [atlas](https://dyndolod.info/Help/Tree-LOD)

Solution: Ensure that no mod is overriding `Textures/Terrain/Tamriel/Trees/TamrielTreeLOD.dds`, BSA parsing should be enabled first.

## Whiterun Gate Guard Missing
Symptom: The guard at Whiterun's gate will be missing, making the city inaccessible.

Solution: Manually move the guard's reference, `GuardWhiterunImperialGate1REF` to the player.
