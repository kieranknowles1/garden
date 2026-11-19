---
dg-home: true
dg-publish: true
---
Welcome to the root of my 100% human-made [Digital Garden](https://forestry.md/).
![[AI-0]]
# What's Here?
Whatever I feel like. If a table shows up as empty, it'll either be a limitation of Forestry's renderer or I haven't published anything that it would link to.
```base
views:
  - type: table
    name: View
    filters:
      and:
        - categories.isEmpty()
        - note["dg-publish"] == true
        - note["dg-home"] != true

```
