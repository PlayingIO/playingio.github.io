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


## Get the Profile of a Mission Performer

```
GET /user-missions/:primary/performers/:id
```

Get the profile of a performer. In private missions, only existing mission performers can view performer profiles.

Returns an object with the player profile information.

#### Response

```json
{
  "id": "trinity",
  "alias": "Trinity!",
  "created":"2014-03-01T16:22:43.751Z",
  "enabled": true,
  "scores": [
    {
      "metric": {
        "id": "xp",
        "name": "experience",
        "type": "point"
      },
      "value": "50"
    }
  ],
  "teams": []
}
```

#### Errors

```
401 access_denied: Player not authorized. Only performers can view performer listing in PRIVATE missions.
404 performer_not_found: Invalid performer_id. Requested performer not a part of the mission.
```


## Create a Mission

```
POST /user-missions
```

Start a mission using a specified mission definition. It returns the state of the newly created mission.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| id       | string |         | ✓        | The ID of the newly created mission instance. |
| access   | string |         | ✓        | The access setting of the mission instance. It can be either public, protected or private. <br/>Defaults to strictest access setting allowed by the definition. |


## Update Mission Settings

```
PATCH /user-missions/:id
```

Update the name or access settings of the mission instance.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| name     | string |         |          | The new name of the mission. |
| access   | string |         |          | The new access setting for the mission. Possible access settings: PUBLIC, PROTECTED, PRIVATE. |

#### Request

```json
{
  "name": "My New Mission",
  "access": "PUBLIC"
}
```

#### Response

```json{
  "id": "my_mission",
  "definition": {
    "id": "work",
    "name": "work"
  },
  "state": "ACTIVE",
   "created": "2014-03-01T16:23:06.501Z",
  "name": "My New Mission",
  "access": "PUBLIC",
  "performers": [
    {
      "id": "clone",
      "alias": "Clonie",
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

#### Errors

```
400 validation_exception: Invalid request
400 invalid_access_setting: Access setting not valid
401 access_denied: Player not authorized to update the mission. Only mission owner can perform update.
```


## Delete a Mission

```
DELETE /user-missions/:id
```

Delete a mission instance.

#### Response

```json
{
  "message": "Mission 'clone/foo' has been deleted"
}
```

#### Errors

```
401 access_denied: Player not authorized to delete the mission. Only the mission owner can delete the mission.
```



## Join a Mission

```
POST /user-missions/:id/performers
```

Join a mission.

You must specify the role and lanes which the player wishes to join in.

There are 2 possible role values :

* observer - Can only view mission state
* player - Can play a mission

The lanes available to join depend on the mission definition.

The desired lanes and roles should be specified in the request body, the object keys being the lanes and the values being the desired roles.

If the mission is public, then the player's roles are returned. If the mission is protected then a generated approval request to join the mission is returned. If the mission is private, then an error is returned.

#### Request

```json
{
  "war": "player"
}
```

#### Response of joining a public mission

```json
{
  "war": "player"
}
```

#### Response of joining a protected mission

```json
{
  "id": "523215efacc4b2f216000034",
  "event": "join:request",
  "timestamp": "2014-03-02T17:47:04.260Z",
  "mission": {
    "id": "droid/kill"
  },
  "roles": {
    "war": "observer"
  },
  "state": "PENDING",
  "actor": {
    "id": "droid",
    "alias": "Droid!"
  }
}
```

#### Errors

```
401 access_denied: Attempt to join a private mission
400 validation_exception: Invalid request
409 player_exists: Player is already a part of the mission.
400 invalid_role: Requested role is invalid. It can only be either player or observer.
400 invalid_lanes: Requested lane is invalid/does not exist.
```


## Leave a Mission

```
DELETE /user-missions/:id/leave
```

Leave a mission.

Its returns a message indicating the result of the operation.


#### Response

```json
{
  "message": "Player 'clone' has successfully left the mission 'droid/kill'"
}
```

#### Errors

```
404 performer_not_found: Player is not a performer in the mission.
403 forbidden: Owner is not allowed to leave the mission.
```


## List Mission Triggers

```
GET /user-missions/:id/triggers
```

Get a list of all available triggers a player can play in a mission instance.

#### Response

```json
[
  {
    "trigger": "destroy_the_world:choose_your_path",
    "name": "Destroy the World",
    "actions": [
      {
        "metric": {
          "id": "existential_plane",
          "type": "state",
          "name": "Existential Plane"
        },
        "value": "Hell",
        "verb": "set"
      },
      {
        "metric": {
          "id": "karma",
          "type": "point",
          "name": "Karma"
        },
        "value": "-1000000000",
        "verb": "add"
      }
    ]
  },
  {
    "trigger": "let_it_burn:choose_your_path",
    "name": "Let it Burn",
    "actions": [
      {
        "metric": {
          "id": "existential_plane",
          "type": "state",
          "name": "Existential Plane"
        },
        "value": "Hell",
        "verb": "set"
      },
      {
        "metric": {
          "id": "karma",
          "type": "point",
          "name": "Karma"
        },
        "value": "1",
        "verb": "add"
      }
    ]
  },
  {
    "trigger": "try_to_save_the_world:choose_your_path",
    "name": "Try to save the World",
    "actions": [
      {
        "metric": {
          "id": "existential_plane",
          "type": "state",
          "name": "Existential Plane"
        },
        "value": "Material",
        "verb": "set"
      },
      {
        "metric": {
          "id": "karma",
          "type": "point",
          "name": "Karma"
        },
        "value": "1000000001",
        "verb": "add"
      }
    ]
  }
]
```

#### Errors

```
401 access_denied: Player not authorized. Player not a part of the mission.
```


