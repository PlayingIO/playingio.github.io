---
title: Users Service
---

## Get Own Profile
```
GET /users/me
```

Returns the profile information for the current player. The first_name and last_name is present only on player profiles which are linked to a Playlyfe User account.

#### Response

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

## Update Own Profile

```
PATCH /users/me
```

Update the profile information for the current player. The alias and email can only be updated.

#### Request

```json
{
 "alias": "Johny"
}
```

#### Response

```json
{
 "id": "neo",
  "alias": "Johny",
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

## Get Own Activity Feed

```
GET /users/me/activities
```

Returns the player's activity feed.

By default, last 24 hour activity is returned. A period can be specified by passing start and end time stamps as query parameters.

#### Parameters

|   Name    | Description |
|-----------|-------------|
| start     | Earliest possible activity time stamp. Unit: UNIX time stamp or ISO time stamp |
| end       | Latest possible activity time stamp. Unit: UNIX time stamp or ISO time stamp |

#### Response

```json
[
  {
    "event": "create",
    "timestamp": "2014-03-01T16:23:06.500Z",
    "process": {
      "id": "neo/5312096a8e1ebb4550a6a6f6",
      "name": "The Matrix"
    },
    "id": "c5465c41-a15d-11e3-84ab-ebaad0c03951"
  },
  {
    "process": {
      "id": "neo/5312096b8e1ebb4550a6a6fc",
      "name": "The Matrix"
    },
    "event": "progress",
    "activity": {
      "id": "destroy_the_world",
      "name": "Destroy the World"
    },
    "changes": [
      {
        "metric": {
          "id": "existential_plane",
          "name": "Existential Plane",
          "type": "state"
        },
        "delta": {
          "old": null,
          "new": "Hell"
        }
      },
      {
        "metric": {
          "id": "karma",
          "name": "Karma",
          "type": "point"
        },
        "delta": {
          "old": "0",
          "new": "-1000000000"
        }
      }
    ],
    "timestamp": "2014-03-01T16:23:29.367Z",
    "id": "d2e79670-a15d-11e3-84ae-ebaad0c03951"
  },
  {
    "process": {
      "id": "neo/5312096b8e1ebb4550a6a6fc",
      "name": "The Matrix"
    },
    "event": "progress",
    "activity": {
      "id": "brahmastra",
      "name": "Brahmastra"
    },
    "changes": [
      {
        "metric": {
          "id": "weapons",
          "name": "Weapons",
          "type": "set"
        },
        "delta": {
          "Bhramastra": {
            "old": "0",
            "new": "1"
          }
        }
      }
    ],
    "timestamp": "2014-03-01T16:23:42.131Z",
    "id": "da833830-a15d-11e3-84ae-ebaad0c03951"
  },
  {
    "event": "create",
    "timestamp": "2014-03-01T16:29:30.088Z",
    "team": {
      "id": "53120aea188101a72a668a5e",
      "name": "The Gaurdians"
    },
    "id": "a9e93e80-a15e-11e3-b581-1b9a3fec215b"
  }
]
```
