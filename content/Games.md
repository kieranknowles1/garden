---
cssclasses:
  - full-width
modified: 2026-11-19T21:49:16.000Z
---

```base
properties:
  file.name:
    displayName: Name
  note.genres:
    displayName: Genres
views:
  - type: cards
    name: Table
    filters:
      and:
        - categories.contains(link("Games"))
    order:
      - file.name
      - genres
    image: note.cover
    imageAspectRatio: 1.5
    imageFit: contain

```
