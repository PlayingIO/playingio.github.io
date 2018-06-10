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


## List Teams

```
GET /teams
```

Get a list of all the teams the player can join or is a part of.

All public and protected teams created by any player in the game will be listed.

Any teams which were bootstrapped will be created and listed here when the player joins the game for the first time.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| skip     | number | 0       |          | Number of teams to skip. |
| limit    | number | 10      |          | Maximum number of teams to return. |

#### Response

```json
{
  "data": [
    {
      "id": "con_artists",
      "name": "The Con Artists",
      "definition": {
        "id": "international",
        "name": "international"
      },
      "access": "PRIVATE",
      "created": "2014-04-03T14:49:00.011Z",
      "owner": {
        "id": "droid",
        "alias": "Droid!"
      },
      "roles": [
        "member",
        "admin",
        "super_admin",
        "owner"
      ],
      "my_roles": [
        "owner"
      ],
      "can_leave": false
    },
    {
      "id": "musicians",
      "name": "The Musicians",
      "definition": {
        "id": "local",
        "name": "local"
      },
      "access": "PRIVATE",
      "created": "2014-03-01T16:29:30.088Z",
      "owner": {
        "id": "droid",
        "alias": "Droid!"
      },
      "roles": [
        "member",
        "admin",
        "super_admin",
        "owner"
      ],
      "my_roles": [
        "owner"
      ],
      "can_leave": false
    }
  ],
  "total": 2
}
```

