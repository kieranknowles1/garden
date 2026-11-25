---
categories:
  - "[[Skyrim]]"
  - "[[Resouces]]"
publish: true
---
```table-of-contents
```
# Animations
## Bound Prisoner
Play idle `OffsetBoundStandingStart` and equip prisoner cuffs to create a bound prisoner, useful for if they are being escorted to jail.
# BSK Quest Document Conventions
- <span style="color:orange">[orange text in square brackets]</span> - Script notes for voice actors
- <span style="color:purple">[purple text in square brackets]</span> - Implementer notes
- <span style="color:cornflowerblue">Light blue text</span> - Shared info/asset, reference external copy rather than duplicating
# Custom Item Categories
You shouldn't edit UI files yourself since it would cause conflicts with other mods. Instead, you can use I4 to assign types based on keywords or IDs. There's a few sample configs in the download and full documentation on GitHub. There's also an icon pack available, adding your own icons requires would require Flash. These can use Scaleform keys defined using [[#UI Localization]].
```json
{
  "match": {
    "formType": "Weapon",
    "keywords": {
        "anyOf": [
          "ccBGSSSE001_FishingPoleKW",
          "IconFishingRod"
      ]
    }
  },
  "assign": {
    "subTypeDisplay": "$FishingRod",
    "iconSource": "I4IconAddon/icons.swf",
    "iconLabel": "weapon_fishingrod"
  }
}
```
## Pitfalls
- Any filter that takes an array is ANDed by default unless `anyOf` is specified.
## See Also
- [Inventory Injector](https://www.nexusmods.com/skyrimspecialedition/mods/85702)
- [Inventory Injector Wiki](https://github.com/Exit-9B/InventoryInjector/wiki)
- [Icon Packs](https://www.nexusmods.com/skyrimspecialedition/mods/85702)
![[Cyrodiil Topic Priorities]]
# UI Localisation
UI strings are defined using Scaleform translations in `Data/Interface/MyMod_LANGUAGE.txt` without the `.esp` extension. These are loaded automatically so long as the plugin with a matching name is loaded.

This file takes the format $key TAB Value such as
```
$myNewType	My New Type
```
Make sure to save it using UTF16 LE with BOM and that you use tabs to separate, not spaces, be careful since some editors silently replace tabs with spaces.
# The Weird and Wonderful World of Woefully Wacky Bugs
## NPC Sleep Package Replaced With Generated Form
Instead of sleeping, the NPC will stand in place, enhanced console reports a generated form (starting with 0xFF) for their package.

Caused by the NPC being unable to reach their bed due to a missing navmesh.
