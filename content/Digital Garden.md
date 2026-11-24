---
permalink: index
publish: true
---
Welcome to the root of my 100% human-made [Digital Garden](https://forestry.md/).
![[AI-0]]
# What's Here?
Whatever I feel like. If a table shows up as empty, assume I haven't published anything that it would link to.
```base
views:
  - type: table
    name: View
    filters:
      and:
        - categories.isEmpty()
        - publish == true
        - link(file) != this

```
