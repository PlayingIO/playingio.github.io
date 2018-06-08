---
title: Users Service
---

## Get Own Profile
```
GET /users/me
```

Returns the profile information for the current player. The first_name and last_name is present only on player profiles which are linked to a Playlyfe User account.

* Response

```json
{
  "first_name": "Johny",
  "last_name": "Jose",
  "id": "neo",
  "alias": "Neo",
  "created": "2014-03-01T16:22:43.751Z",
  "scores": [
    {
      "metric": {
        "id": "existential_plane",
        "name": "Existential Plane",
        "type": "state"
      },
      "value": {
        "name": "Hell",
        "description": "The plane of demons"
      }
    },
    {
      "metric": {
        "id": "karma",
        "name": "Karma",
        "type": "point"
      },
      "value": "-1000000000"
    },
    {
      "metric": {
        "id": "weapons",
        "name": "Weapons",
        "type": "set"
      },
      "value": {
        "Bhramastra": {
          "description": "The Ultimate Weapon of mass destruction",
          "count": "1"
        }
      }
    }
  ],
  "enabled": true,
  "teams": [
    {
      "id": "53120aea188101a72a668a5e",
      "definition": {
        "id": "immortals",
        "name": "Immortals"
      },
      "roles": [
        "God"
      ],
      "name": "The Gaurdians"
    }
  ]
}
```