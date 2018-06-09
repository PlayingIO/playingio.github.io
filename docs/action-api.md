---
title: Actions Service
---

## List All Actions

```
GET /actions
```

Get a list of all available actions.

#### Response

```json
[
  {
    "id": "collect_coin",
    "name": "collect a coin",
    "description": "Hit the brick and collect a coin",
    "actions": [
      {
        "metric": {
          "id": "coins",
          "type": "point",
          "name": "coins"
        },
        "value": "1",
        "verb": "add",
        "probability": 1
      }
    ]
  },
  {
    "id": "rescue",
    "name": "rescue_princess",
    "description": "Rescue the princess",
    "actions": [
      {
        "metric": {
          "id": "coins",
          "type": "point",
          "name": "coins"
        },
        "value": "300",
        "verb": "add",
        "probability": 1
      }
    ]
  }
]
```


