---
categories:
  - "[[Skyrim Modder's Book]]"
publish: true
---
Excerpt from the Cyrodiil implementation best practices document.

| Priority | Description                                                                                                                                                                 |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 255      | Very Special Dialogue (For mechanics/scripted radiant scenarios, blocking dialogue to prevent other dialogue and stuff like that)                                           |
| 86-100   | Questline / City Quest Dialogue                                                                                                                                             |
| 71-85    | Side Quest Dialogue                                                                                                                                                         |
| 61-70    | Misc Quest Dialogue                                                                                                                                                         |
| 53-60    | Service Dialogue                                                                                                                                                            |
| 52       | Accept any quest later (I'll find the amulet you're missing.)                                                                                                               |
| 51       | Obvious quest start (Do you need any help?)                                                                                                                                 |
| 0-50     | General Dialogue (Can lead to quests too, when it is disguised as standard dialogue).<br><br>These should be setup with intervals of 5 starting from 50:<br><br>50, 45, 40… |
| 0        | Rumor dialogue - order based on quest priority                                                                                                                              |
