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


## Get a Team

```
GET /teams/:id
```

Get information about a particular team instance.

#### Response

```json
{
  "id": "globe_trotters",
  "name": "The Globe Trotters",
  "definition": {
    "id": "local",
    "name": "local"
  },
  "created": "2014-03-01T16:29:30.088Z",
  "access": "PROTECTED",
  "owner": {
    "id": "clone",
    "alias": "Clonie"
  },
  "locked": false,
  "member_count": [
    {
      "name": "member",
      "count": 0
    },
    {
      "name": "admin",
      "count": 0
    },
    {
      "name": "super_admin",
      "count": 0
    },
    {
      "name": "owner",
      "count": 1
    }
  ],
  "total_members": 1,
  "roles": [
    "admin",
    "member",
    "owner",
    "super_admin"
  ],
  "my_roles": [
    "owner"
  ],
  "can_leave": false
}
```


## Create a Team

```
POST /teams
```

Create a team instance by specifying a team definition.

It returns the state of the newly created team.

|   Name    | Type   | Default | Required |   Description   |
|-----------|--------|---------|----------|-----------------|
| groupname | number | 0       | ✓        | The name of the newly created team instance. |
| access    | string |         | ✓        | The access setting of the team instance. It can be either public, protected or private. <br/>Defaults to strictest access setting allowed by the definition. |
| definition| string |         | ✓        | ID of the team definition which is to be used. |

#### Reqeust

```json
{
  "name": "globe-trotters",
  "access": "PRIVATE",
  "definition": "national"
}
```

#### Response

```json
{
  "id": "globe-trotters",
  "name": "globe-trotters",
  "definition": "national",
  "created": "2014-03-02T15:11:48.079Z",
  "access": "PRIVATE",
  "owner": "morpheus",
  "locked": false,
  "member_count": {
    "God": 1
  },
  "total_members": 1
}
```

