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


## Play a Mission Trigger

```
POST /user-missiions/:id/tasks
```

Play a mission. Playing a mission causes its state to change.

It returns an array containing:
* The new triggers that become available
* The updated scores of the player
* Any events that occured

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| trigger  | string |         | ✓        | The ID of the trigger to be played. |
| scopes   | string |         |          | Scope in which the scores will be counted. |

#### Request

```json
{
  "trigger": "try_to_save_the_world:choose_your_path"
}
```

#### Response

```json
{
  "triggers": [
    {
      "trigger": "brahmastra:choose_your_weapon",
      "name": "Brahmastra",
      "lane": "matrix",
      "locked": "false",
      "rewards": [
        {
          "metric": {
            "id": "weapons",
            "type": "set",
            "name": "Weapons"
          },
          "value": {
            "Bhramastra": "1"
          },
          "verb": "add"
        }
      ]
    },
    {
      "trigger": "excalibur:choose_your_weapon",
      "name": "Excalibur",
      "lane": "matrix",
      "locked": "false",
      "rewards": [
        {
          "metric": {
            "id": "weapons",
            "type": "set",
            "name": "Weapons"
          },
          "value": {
            "Excalibur": "1"
          },
          "verb": "add"
        }
      ]
    },
    {
      "trigger": "mjolinir:choose_your_weapon",
      "name": "Mjolinir",
      "lane": "matrix",
      "locked": "false",
      "rewards": [
        {
          "metric": {
            "id": "weapons",
            "type": "set",
            "name": "Weapons"
          },
          "value": {
            "Mjolinir": "1"
          },
          "verb": "add"
        }
      ]
    }
  ],
  "events": {
    "local": [
      {
        "event": "progress",
        "activity": {
          "id": "try_to_save_the_world",
          "name": "Try to save the World"
        },
        "actor": {
          "id": "neo",
          "alias": "Neo"
        },
        "changes": [
          {
            "metric": {
              "id": "karma",
              "name": "Karma",
              "type": "point"
            },
            "delta": {
              "old": "1000000001",
              "new": "2000000002"
            }
          }
        ],
        "timestamp": "2014-03-02T17:47:04.260Z",
        "id": "aa6d4840-a232-11e3-9af5-753d0e60669a"
      }
    ],
    "global": []
  }
}
```

#### Errors

```
401 access_denied: Player not authorized. He is not a performer in the mission.
```


## Kick Performer from a Mission

```
DELETE /user-missions/:primary/performers/:id/kick
```

Kick out a performer from the mission.

This can only be done by the owner of the mission and the owner himself cannot be kicked out.

Returns a confirmation message of the event.

#### Response

```json
{
  "message": "The player 'morpheus' has been removed from mission '/neo/therealmatrix'"
}
```

#### Errors

```
401 access_denied: Player not authorized. Only mission owners can kick performers.
404 performer_not_found: Invalid performer_id. Requested performer is not a part of the mission.
403 forbidden: Mission owner cannot be kicked.
```

## Transfer Mission Ownership

```
PATCH /user-missions/:primary/transfer
```

Transfer the ownership of the mission to another player.

The new owner can be either an existing performer in the mission or any other player in the game.

The lanes and roles assigned to the new owner can be specified in the body of the request. If no lanes are specified, the mission default lanes are given to the owner.

The owner transferring ownership will retain his lanes in the mission after the transfer.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| newOwner | string |         | ✓        | ID of the new owner. |
| roles    | object |         | ✓        | Object that contains the lanes/roles of the new owner. |

##### Request

```json
{
  "newOwner": "rogers",
  "roles": {
    "main": "player"
  }
}
```

#### Response

```json
{
    "id": "fury/avengers",
    "definition": "work",
    "state": "ACTIVE",
    "created": "april11,2012",
    "access": "PROTECTED",
    "performers": {
        "fury": {
            "main": "player"
        },
        "loki": {
            "main": "observer"
        },
        "stark": {
            "main": "player"
        }
    },
    "keys": {
        "fury/avengers": "work"
    },
    "containers": {
        "work": {
            "key": "fury/avengers",
            "tokens": 1,
            "nodes": {
                "aaa": {
                    "state": "READY",
                    "tokens": 1
                }
            }
        }
    },
    "owner": "rogers"
}
```

#### Errors

```
401 access_denied: Player not authorized. Only the instance owner can transfer ownership.
400 invalid_lane: Invalid lane specified.
400 invalid_role: Roles provided are invalid. Player must have at least one role.
```


## Change Own Roles in Mission

```
POST /user-missions/:primary/roles/:user_id
```

If the mission is public then the player's roles are updated and the new mission state is returned. If the mission is protected or private then a generated approval request for role change is returned.

The new roles should be passed as an object in the request body. Keys in the object represent the lane id and the values should be the desired role.

There are 3 possible role values :

* **observer** - Can only view mission state
* **player** - Can play a mission
* ***false*** - Removes the player from the lane

The lanes available to join depend on the mission definition.

#### Request

```json
{
  "main": "player"
}
```

#### Response

In case of a protected or private mission.

```json
{
  "id": "523215efacc4b2f216000034",
  "event": "role:request",
  "timestamp": "2014-03-02T17:47:04.260Z",
  "mission": {
    "id": "droid/kill"
  },
  "roles": {
    "main": "player"
  },
  "state": "PENDING",
  "actor": {
    "id": "droid",
    "alias": "Droid!"
  }
}
```


## Get a Mission's Activity Feed

```
GET /user-missions/:primary/activities
```

Get mission activity feed.

By default, if no start and end parameters are provided, the activity feed for the last 24 hours is provided.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| start    | date   |         |          | Earliest possible activity timestamp ISO format. |
| end      | date   |         |          | Latest possible activity timestamp ISO format. |

#### Errors

```
400 invalid_date: Requested date format is invalid.
```


## List Invitations Sent out for a Mission

```
GET /user-missions/:primary/invites
```

Get a list of invites the player has sent out to join a missions.

Only invitations sent out by the player will be listed.

#### Response

```json
{
  "data": [
    {
      "event": "invite",
      "timestamp": "2014-03-02T17:37:29.371Z",
      "invitee": {
        "id": "trinity",
        "alias": "Trinity"
      }
      "roles": {
        "matrix": "player"
      },
      "state": "PENDING",
      "id": "53c42eb0-a231-11e3-b632-ede196eae44e"
    }
  ],
  "total": 1
}
```


## Send a Mission Invitation

```
POST /user-missions/:primary/invites
```

Invite a player to join a mission.

Only the owner of the mission can invite other players.

The roles need to be specified as an object with the key being the lane name and the value is the role in the lane.

It returns the invitation request.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| player   | string |         | ✓        | The ID of the player to whom the invitation has to be sent to. |
| roles    | object |         | ✓        | The roles of the player in the the mission. |

#### Request

```json
{
  "player": "trinity",
  "roles": {
    "matrix": "player",
    "training": "observer"
  }
}
```

#### Response

```json
{
  "id": "aef37730-a233-11e3-9af5-753d0e60669a",
  "event": "invite",
  "timestamp": "2014-03-02T17:54:21.347Z",
  "actor": {
    "id": "neo",
    "alias": "Neo"
  },
  "invitee": {
    "id": "trinity",
    "alias": "Trinity"
  },
  "mission": {
    "id": "neo/therealmatrix",
    "name": "Protected"
  },
  "roles": {
    "matrix": "player",
    "training": "observer"
  },
  "state": "PENDING"
}
```


## Cancel a Sent Invitation

```
DELETE /user-missions/:primary/invites/:id
```

Cancel a pending invite sent out by the player.

It returns the canceled invitation request.


#### Response

```json
{
  "event": "invite",
  "timestamp": "2014-03-02T18:03:59.848Z"
  "mission": {
    "id": "neo/therealmatrix",
    "name": "The Real Matrix"
  },
  "actor": {
    "id": "neo",
    "name": "Neo"
  },
  "invitee": {
    "id": "trinity",
    "name": "Trinity"
  },
  "roles": {
    "good": "player"
  },
  "state": "CANCELLED",
  "id": "052de920-a174-11e3-b581-1b9a3fec215b"
  "cancelledAt": "2014-03-02T18:04:08.507Z"
}
```


## List Pending Mission Join or Role Change Requests

```
GET /user-missions/:primary/approvals
```

Get a list of pending player requests in a mission.

A pending request can be approved by the owner of the mission. Others who try to request the list of approvals will get an error.

#### Response

```json
{
  "data": [
    {
      "event": "join:request",
      "timestamp": "2014-03-01T19:02:22.642Z",
      "actor": {
        "id": "morpheus",
        "alias": "Morpheus"
      },
      "roles": {
        "matrix": "player"
      },
      "state": "PENDING",
      "id": "052de920-a174-11e3-b581-1b9a3fec215b"
    }
  ],
  "total": 1
}
```

#### Errors

```
401 access_denied: When a non owner tries to view approval requests
```


## Approve Mission Join or Role Change Request

```
PATCH /user-missions/:primary/approvals/:request_id
```

Accept a player request to join a mission or role change.

Only the owner of the mission has the right to approve requests.

It returns the accepted approval request.

#### Response

```json
{
  "event": "join:request",
  "timestamp": "2014-03-01T19:02:22.642Z",
  "actor": {
    "id": "morpheus",
    "alias": "Morpheus"
  },
  "mission": {
    "id": "neo/therealmatrix",
    "name": "Protected"
  },
  "roles": {
    "matrix": "player"
  },
  "state": "ACCEPTED",
  "id": "052de920-a174-11e3-b581-1b9a3fec215b",
  "accepted_by": {
    "id": "neo",
    "alias": "Neo"
  },
  "accepted_at": "2014-03-02T17:51:42.157Z"
}
```

#### Errors

```
404 invalid_approval_request: Invalid request_id
401 access_denied: Player not authorized to approve request.
```


## Reject Mission Join or Role Change Request

```
DELETE /user-missions/:primary/approvals/:request_id
```

Reject a player request to join or change roles in a mission.

Only the owner of the mission has the rights to reject a request.

It returns the rejected approval request.

#### Response

```json

  "event": "join:request",
  "timestamp": "2014-03-02T18:05:29.321Z",
  "actor": {
    "id": "trinity",
    "alias": "Trinity"
  },
  "mission": {
    "id": "neo/thefakematrix",
    "name": "The Fake Matrix"
  },
  "roles": {
    "matrix": "player"
  },
  "state": "REJECTED",
  "id": "3d183590-a235-11e3-9af5-753d0e60669a",
  "rejected_by": {
    "id": "neo",
    "alias": "Neo"
  },
  "rejectedAt": "2014-03-02T18:05:43.984Z"
}
```

#### Errors

```
404 invalid_approval_request: Invalid request_id
401 access_denied: Player not authorized to reject a request. Only mission owners can accept/reject requests.
```

