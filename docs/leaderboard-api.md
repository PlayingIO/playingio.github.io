---
title: Leaderboards Service
---

## List All Leaderboards

```
GET /leaderboards
```

Get a list of all available leaderboards.

#### Response

```json
[
  {
    "id": "game_leaderboard",
    "name": "GameLeaderboard",
    "entity_type": "players",
    "metric": {
      "id": "xp",
      "type": "point",
      "name": "experience"
    },
    "scope": {
      "type": "game"
    },
    "cycles": [
      "weekly",
      "daily"
    ]
  },
  {
    "id": "team_definition_leaderboard",
    "name": "TeamDefinitionLeaderboard",
    "entity_type": "players",
    "metric": {
      "id": "xp",
      "type": "point",
      "name": "experience"
    },
    "scope": {
      "id": "international",
      "type": "team_definition"
    },
    "cycles": [
      "weekly",
      "daily"
    ]
  },
  {
    "id": "team_leaderboard",
    "name": "TeamLeaderboard",
    "entity_type": "players",
    "metric": {
      "id": "xp",
      "type": "point",
      "name": "experience"
    },
    "scope": {
      "id": "international",
      "type": "team_instance"
    },
    "cycles": [
      "weekly",
      "daily"
    ]
  }
]
```

