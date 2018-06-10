---
title: Teams Service
---

## List All Team Definitions

```
GET /team-definitions
```

Get a list of all available team definitions. Team definitions are the blueprints used to create a new team instance.

Only the teams which can be created by the player will be listed.

#### Response

```json
[
  {
    "id": "immortals",
    "name": "Immortals",
    "roles": [
      "God",
      "Angels"
    ],
    "access": [
      "PRIVATE"
    ]
  },
  {
    "id": "demons",
    "name": "Demons",
    "roles": [
      "Devil",
      "Angels",
      "Monsters"
    ],
    "access": [
      "PRIVATE"
    ]
  }
]
```

