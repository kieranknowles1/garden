---
categories:
  - "[[Beyond Skyrim Cyrodiil]]"
publish: true
urls:
  - https://www.nexusmods.com/skyrimspecialedition/mods/10917
---
Changelog of my contributions to Bruma 2.0, see also the [Full Changelog](https://www.nexusmods.com/skyrimspecialedition/articles/12625). If something here doesn't appear in the full changelog, it was likely only applicable to dev builds.
```table-of-contents
```
# Early July 2026
 - Implement spliced Screeching Songman lines for Crouches and Reln-Tei if Adius is taken.
 - Fix hitbox of Frostfire Glade Guardian
# Early August 2026
 - Ugly Love fixes
	- Fix VA/subtitle mismatch when showing Dumrag Simund's letter
	- Fix Afanna's grab journal scene not progressing if player not in cell, repeated forcegreet, and disable topic once player gets key/journal
	- Fix Afanna's forcegreet after player gets journal
	- Disallow interrupting Dumrag during scenes
	- Move Dumrag's final forcegreet to scene and add `IdleGive`
	- Don't delete Atul until out of sight in kill both/kill Simund ending. Don't delete her at all in spare ending.
	- Disable "what happened to your wife" if Simund survives
	- Fix bounty when attacking Dumrag in suicidal ending
 - Remove unnecessary persistence from various Bruma County NPCs
 - Correct persist location of various Bruma County NPCs
 - Add missing ref types to various Bruma interiors
 - Fix invalid usage of `$PrintedFont` in books
 - Fix investor dialogue
### Assets
 - Add missing brass amulet addon for Argonians
 - Add missing temper recipes for worn armour/weapons
 - Fix Ayleid combat dialogue for elven vampires
# Late August 2026
 - Correct generic dialogue topic prefixes for `CYRHit` and `CYRFlee`.
 - Remove unvoiced female thug from restful watchman scene
 - Fix misnamed Afanna lines in Ugly Love
 - Restrict Burdsson badgering Gort guards to voice types with dialogue
 - Remove unvoiced "My my, a dog" from Elrus
 - Remove unvoiced section of Amiel's Greenwood success response (response works without this section)
# Early September 2026
- Fix missing dialogue when declining to purchase Bruma house
# Early October 2026
- Remove several unvoiced lines from Bruma city.
- Fix unnecessary and unvoiced second prompt when asking Botram to repeat a story.
- Fix follower-specific combat lines never playing.
- Disable several lines for voices who are missing them.
- Fix output models for bard songs.
## Bard Fixes Research Notes
- Vanilla uses `SOMDialogue3D4000BARDS` for all responses, including intros and instrumentals, CYR songs previously used `SOMStereoRad04000_verb` which has a different attenuation curve, much higher reverb, and fixed speaker mixing (2D), only attenuates with distance).
- This was a regression from current Bruma release, at some point the bard system was refactored to put everything in the same scene and use a BSK script.
- Observation: Scene dialogue is faded when in direct conversation with an NPC so long as it's output model uses HRTF mixing.
	- Probably not a good candidate for analysers since there's legitimate use cases to putting a 2D output model on scene dialogue.
Average RMS:
- A Storm Gathers Darkly: -17.19dB
- Ragnar the Red: -19.87dB
# Early November 2026
- Alammu's Veil: Fix contraband line playing if walking away from Alammu after acquiring heirloom.
- Disable more unvoiced dialogue
- Fix collision of Zyrn Kraz (Frostfire Glade mini boss)
- Add missing `SetPlayerTeammate` for Adius and Olaf when they're following player.
- Fix second quest marker when inside Fort Cutpurse.
- Prevent Olaf and Nords following player outside Sedor.
# Bruma 2.0 Last Minute
- Fix invalid stage references in conditions.
- Update Bruma Stables objective in Horse, off course.
- Fix Ovacca's last line hanging in A Cutter in Need.
- Fix initially disabled create object aliases in Cider Delivery, Fighters Guild Advertising, Word to Bruma, Return Serulius's journal, Aera the Devout's Missing Amulet, Gort's Generic Fetch Quest, and A Stormcloak in Chains.
- Fix Aera the Devout's Missing Amulet being able to select any bandit camp in Cyrodiil.
## Stormcloak in Chains
- Fix generic Imperial guard not assisting the jailor during the fight.
- Fix jailor accepting player's yield.
- Disable archery training packages while fort is on alert.
- Soldiers run up to legate during final confrontation.
- Interrupt "here comes the jailer" scene if door is opened.
- Add missing links back during second interrogation persuade failures.
- Prevent garrison from assisting if player attacks Oculatus.
- Have Bjarni stay behind his guard while being escorted.
- Fix Oculatus teleporting away on death.
- Fix duplicate explosion effect when attacking second agent.
## Assets
- Add async `DeleteWhenAble` to `BSKUtilAdvanced`.
# Bruma 2.0 Hotfix
There's always the hotfix.
- Remove unvoiced female Orcs
- Fix buying horse from Wildeye stables only removing 900 gold instead of 3000
- Fix floating ref in Underpall Keep, near Frostcrag Spire, and in `POISnow16` (County Bruma Talos statue)
- Touch up navmesh in `POISnow16`
- Account for iron ingots already carried at start of Gort's quest
- Fix displayed iron ingot count exceeding target if picking up multiple at once
- Fix questioning Harald about legitimacy after a success angering him.
## Stormcloak in Chains
- Calm soldiers when on top of tower during jailbreak.
- Fix potential softlock when the Oculatus arrive.
- Fix Bjarni not having his hands bound when taken outside.
