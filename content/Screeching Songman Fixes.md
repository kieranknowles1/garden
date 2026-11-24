---
categories:
  - "[[Beyond Skyrim Cyrodiil]]"
publish: true
status: Complete
tags:
  - project/beyond-skyrim
---

![](https://photos.selwonk.uk/api/assets/d299e734-e24a-477c-ac2b-630502acf414/thumbnail?size=preview#.jpg)
## Issues

- [ ] \[Self\] Line "Who are you complaining about" is available for all quest starter NPCs, but only one will have complained.

## Could Not Reproduce

- [ ] \[Renod Watchman Scene\] There is quite a long pause between Bentior's "_I have to say, this is one of the most entertaining brawls we've had for a while._" line and Adius arriving and saying "_Bentior, I'm here to- now, now... what's this?_" In the meantime everyone just stands around.
    - [ ] Happens occasionally, but not enough to properly test a fix.
- [x] \[Renod Watchman Scene\] Is very often overridden by the various Serulius and Bentior scenes.
      Not applicable since Serulius scenes were disabled.

## TODOs

- [x] \[Renod Watchman Scene\] Adius says "_Break it up, you four_" even if there are fewer people in the inn than that. Seems to be relic of previous set up where there would be multiple "thugs". Could just cut line to "_Break it up"_?
    - [x] Thug3 is voiced but not Thug2
    - [x] Aliases and actors were in place, but never enabled due to a missing property.
    - [x] Could reuse existing lines for Thug2

## Done
- [x] \[Self\] Renod does not use his lute during scenes, idle stops after first INFO
- [x] \[20\] Crouches's persuasion fail line "_I'm sure. No, you're probably just one of Adius' little hound dogs._" appears to be missing a voice file.
    - [x] Only applies to male version. Could trim female version and/or splice.
    - [x] It was always there just named wrong.
- [x] \[30\] If you "buy" the lute for 3000 gold, no gold is actually taken from you.
- [x] \[Self\] Renod has two lute meshes. Singing uses CYR mesh while battle lute and item use vanilla mesh.
- [x] \[150\] If the player exits out of dialogue when talking to Reln-Tei after returning Renod's lute and before picking an option, they will get the dialogue choice "…" to get back to the quest ending instead of a normal line. His other lines ("have a coin" etc) will also appear. We should lock the latter or offer the quest-related options instead.
- [x] \[SCK\] a weird bug I'm getting... In the last merge I edited the quest fragment on stage 40 of CYRMS04 to replace "Stop()" (which was unsurprisingly bugging out the quest) with lines to get objectives 30 & 40 to work properly. The weird thing is that the changes appear fine in the CK but in-game I still get the quest stopping at stage 40...    just deleted stop() and replaced it with:     `setObjectiveCompleted(30)`    `setObjectiveDisplayed(40)`
    - [x] This change was already present in the fragment script, but included in a different commit for whatever reason.
- [ ] \[Self, Watchman Scene\] Awkward pause if thugs spawn far away from Renod. Could move them closer or increase walking pace.
- [x] \[10\] The quest log will indicate that Reln-Tei doesn't want Renod to be killed even if the player doesn't offer to kill him and you thus never hear Reln's line about this. To avoid multiple quest stages, perhaps we could restructure the dialogue tree so that a slightly amended "~~What if~~ **Are you saying I** should kill~~ed~~ him?" (CYRMS04MiscRelnTeiKillQuestion) is always the first option. The textual change would accommodate for role-players whose first thought wouldn't be to actively suggest murder. This line would then unlock "**I'll talk to him.**" (CYRMS04MiscRelnTeiAgree) and a slightly amended "I will **help,** if there's pay involved." (CYRMS04MiscRelnTeiPayNeeded).
    - [x] Line references Adius, would need splicing.
- [ ] \[Renod Watchman Scene\] It can also still be heard after you've already spoken to Crouches about them kicking Renod out. Would either condition out or make quest start conditional on you hearing both scenes.
    - [ ] Can't condition quest start as Adius being taken disables the Watchman scene.
- [x] \[Self\] Crouches persuasion fails should link back to topics rather than be goodbyes
- [x] \[Self, Watchman\] Adius can block Renod on the way out of the inn.

## For Vikunja

- [x] Watchman scene is disabled if Adius is taken. Could consider if we'll have a recording for other captains.

## Nice to Have

- [x] Renod's dialogue mentions him being in his home regardless of his current location
    - [x] Cut out "In my house" from first line
    - [x] Cut out "barge into my home" from "not as good as you think"
- [x] If Adius has been taken, Crouches is missing a line for failing persuasion as generic quest INFOs were never recorded.
    - [x] Cut out Adius part and replace with guards
- [x] Splice lines for Thug2, could use any voice type, even with a different race/gender, as she only appears in this one scene.

## For Bruma Changelog

- [x] Fix gold not being removed if buying from bandits for 3000 gold.
- [x] Fix "…" if exiting Reln-Tei's end of quest dialogue.
- [x] Fix Renod missing hellos/goodbyes due to his voice type being missing from the unique voice form list.
    - [x] Need to confirm if this is an issue in the current Bruma release.
    - [x] Needed to change this on `DialogueBruma` as his lines for `CYRGenericDialogueUniqueVoices` were never recorded.
