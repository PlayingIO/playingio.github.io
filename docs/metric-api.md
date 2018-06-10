---
title: Metrics Service
---

## List All Metrics

```
GET /metrics
```

Get a list of all the metrics in the game.

#### Response

```json
[
  {
    "id": "badges",
    "name": "badges",
    "type": "set",
    "items": [
      "expert",
      "novice",
      "seasoned"
    ]
  },
  {
    "id": "rank",
    "name": "rank",
    "type": "state",
    "states": [
      {
        "name": "Brigadier"
      },
      {
        "name": "Colonel"
      },
      {
        "name": "General"
      },
      {
        "name": "Recruit"
      },
      {
        "name": "noob"
      }
    ]
  },
  {
    "items": [],
    "id": "tags",
    "name": "tags",
    "type": "set"
  },
  {
    "id": "xp",
    "name": "experience",
    "type": "point"
  }
]
```

