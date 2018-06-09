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


## List Missions

```
GET /user-missions/
```

Get a list of all the missions a player can play/join.

All public and protected missions created by any player in the game will be listed.

Any missions which were bootstrapped will be created and listed here when the player joins the game for the first time.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| skip     | number | 10      |          | Number of missions to skip. |
| limit    | number | 10      |          | Maximum number of missions to return. |
| state    | string | ACTIVE  |          | Comma separated list of mission states to filter by. Mission states: ACTIVE, COMPLETED. |

#### Response

```json
{
  "data": [
    {
      "id": "neo/5312096a8e1ebb4550a6a6f6",
      "name": "Real Life",
      "definition": "boring_stuff",
      "access": "PRIVATE",
      "created": "2014-03-01T16:23:06.500Z",
      "owner": {
        "id": "neo",
        "alias": "Neo"
      },
      "state": "ACTIVE",
      "performers": [
        {
          "id": "neo",
          "alias": "Neo"
        }
      ]
    },
    {
      "id": "neo/5312096a8e1ebb4550a6a6f8",
      "access": "PRIVATE",
      "created": "2014-03-01T16:23:06.501Z",
      "definition": "destroy_evil",
      "name": "The Matrix",
      "state": "ACTIVE",
      "owner": {
        "id": "neo",
        "alias": "Neo"
      },
      "performers": [
        {
          "id": "neo",
          "alias": "Neo"
        },
        {
          "id": "morpheus",
          "alias": "Morpheus"
        }
      ],
    }
  ],
  "total": 2
}
```


## Get a Mission

```
GET /user-missions/:id
```

Get details of a mission instance with the specified id.

#### Response

```json
{
  "id": "driving_test",
  "definition": {
    "id": "test",
    "name": "Simple tests"
  },
  "state": "ACTIVE",
  "created": "2014-03-01T16:34:57.402Z",
  "access": "PROTECTED",
  "performers": [
    {
      "id": "alonso",
      "alias": "Alonso",
      "lanes": [
        {
          "name": "main",
          "role": "observer"
        }
      ]
    },
    {
      "id": "massa",
      "alias": "Massa",
      "lanes": [
        {
          "name": "main",
          "role": "player"
        }
      ]
    }
  ],
  "owner": {
    "id": "clone",
    "alias": "Clonie"
  },
  "lanes": [
    "main"
  ]
}
```


## List All Performers of a Mission

```
GET /user-missions/:primary/performers
```

List the performers of the mission.

Returns an array with objects containing the player id and alias.

#### Response

```json
[
  {
    "id": "neo",
    "alias": "The One!"
  },
  {
    "id": "trinity",
    "alias": "Trinity!"
  }
]
```


