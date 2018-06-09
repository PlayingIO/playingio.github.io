---
title: Missions Service
---

## List All Mission Definitions

```
GET /mission-definitions/
```

Get a list of all available mission definitions. Mission definitions are the blueprints used to create a new mission instance.

#### Response

```json
[
  {
    "id": "destroy_evil",
    "lanes": ["neutral", "good", "evil"],
    "name": "Destroy Evil",
    "access": [
      "PRIVATE"
    ]
  },
  {
    "id": "destroy_good",
    "lanes": ["neutral", "good", "evil"],
    "name": "Destroy Good",
    "access": [
      "PUBLIC"
    ]
  }
]
```

