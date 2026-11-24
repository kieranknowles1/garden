---
cssclasses:
  - full-width
publish: true
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
