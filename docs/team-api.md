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

#### Errors

```
401 access_denied: Only members can view state of private teams
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

#### Errors

```
403 forbidden: Player does not meet requirements to create a team of that definition.
404 team_definition_not_found: Invalid team_definition_id.
400 invalid_access_setting: Requested access settings are invalid.
409 team_instance_exists: A team with the same ID already exists.
```


## Join a Team

```
POST /teams/:primary/members
```

Join a team.

You must specify the roles with which you want to join the team. The roles available to join depend on the team definition.

If the team is public then he is added to the team and the player's roles are returned. If the team is protected then a generated approval request to join the team is returned. If the team is private an error is returned.

The requested roles should be specified as an object in the request body with the keys being the requested role.

The response returned are the player's roles. The key is the name of the role and the value is the its rank.

#### Request

```json
{
   "hacker": true
}
```

#### Response

* In case of a public team.

```json
{
   "hacker": true
}
```

* In case of a protected team.

```json
{
  "id": "520512b563c67d641e000002",
  "event": "join:request",
  "timestamp": "2014-03-02T18:03:59.848Z",
  "actor": {
    "id": "droid",
    "alias": "Droid!"
  },
  "team": {
    "id": "anonymous",
    "name": "Anonymous"
  },
  "roles": {
    "hacker": true
  },
  "state": "PENDING"
}
```

#### Errors

```
409 player_exists: Player already a part of the team.
401 team_locked: Team is locked. Cannot add new players.
400 invalid_role: Requested role is invalid.
401 access_denied: Cannot join a private team.
```


## Leave a Team

```
DELETE /teams/:id
```

Leave a team instance who's id is specified.

It returns a confirmation message of the event.

#### Response

```json
{
  "message": "The player 'hulk' has been removed from team 'avengers'"
}
```

#### Errors

```
403 forbidden: Player cannot leave the team.
404 member_not_found: Player not a member of the team.
```


## Change Own Roles in Team

```
PATCH /teams/:primray/roles/:user_id
```

Update player roles in a team.

In a public team, the roles are changed immediately and the new player roles are returned.

In case of a protected and private team, player's roles are updated if he has the required permissions and the updated player roles are returned. If the player does not have permission, it places a request for role change and returns the request.

Each member of the team must have at least 1 role in the team.

The requested roles should be in passed in the body of the request with the keys being the requested roles.

#### Request

```json
{
  "member": false,
  "admin": true
}
```

#### Response

* In a public team

```json
{
  "message": "You have successfully changed member droid roles in team con_artists",
  "roles": [
    "admin"
  ]
}
````

* In case of a protected or private team.

```json
{
  "id": "633945c0-29fd-11e4-b2d2-3d471184ea59",
  "event": "role:request",
  "timestamp": "2014-08-22T13:08:19.612Z",
  "team": {
    "id": "globe_trotters",
    "name": "The Globe Trotters"
  },
  "roles": {
    "member": false,
    "admin": true
  },
  "state": "PENDING",
  "actor": {
    "id": "ripley",
    "alias": "The Amazing Mr.Ripley!"
  }
}
```

#### Errors

```
401 team_locked: Cannot update roles when the team is locked.
400 role_required: Player must have at least 1 role in the team.
```


## List All Members of a Team

```
GET /teams/:primary/members
```

Get a list of all the players who are members of a team

In case of a private team, only the members of the team can see this list.

#### Response

```json
{
  "data": [
    {
      "alias": "Neo",
      "id": "neo"
    },
    {
      "alias": "Trinity",
      "id": "trinity"
    }
  ],
  "total": 2
}
```


## Get the Profile of a Team Member

```
GET /teams/:primary/members/:id
```

Get a team member's profile. Only fellow team members can see the profile.

#### Response

```json
{
  "id": "neo",
  "created": "2014-03-02T18:03:59.848Z",
  "enabled": true,
  "alias": "The One",
  "scores": [
    {
      "metric": {
        "id": "xp",
        "name": "experience",
        "type": "point"
      },
      "value": "100"
    }
  ],
  "teams": [
    {
      "id": "hackers",
      "definition": {
        "id": "international",
        "name": "international"
      },
      "roles": [
        "hacker"
      ],
      "name": "TheMatrix"
    }
  ]
}
```

#### Errors

```json
401 access_denied: Player not authorized. In PRIVATE teams, only team members can view the team.
404player_not_found: Requested player is not a part of that team.
```


## Kick Member From a Team

```
DELETE /teams/:primary/members/:id/kick
```

Kick a member from the team.

Team owners can not be kicked. Members who kick must have sufficient privileges to do so else an error is returned.

#### Response

```json
{
  "message": "The player 'trinity' has been removed from team 'hackers'"
}
```

#### Errors

```
401 access_denied: Player does not have sufficient permissions to perform kick.
403 forbidden: Cannot kick owner of the team.
404 member_not_found: Requested player not a part of the team.
```


## Change Roles of a Team Member

```
PATCH /teams/:primary/roles/:id
```

Update roles of a member.

To perform the role update, the player must have sufficient privileges within the team else an error is returned. All members in the team must have at least 1 role.

The required roles should be specified in the request body.

Response with the new roles and their ranks is returned.

#### Request

```json
{
  "mentor": true,
  "hacker": false
}
```

#### Response

```json
{
  "message": "You have successfully changed member trinity roles in team hacker",
  "roles": [
    "mentor"
  ]
}
```

#### Errors

```
401 access_denied: Player does not have required permissions to perform the role update.
404 member_not_found: Requested player not found in the team.
400 invalid_role: Invalid role requested.
```


## Transfer Team Ownership

```
PATCH /teams/:id/transfer
```

Transfer team ownership to another player.

The new owner can be either an existing member of the team or any other player in the game.

The roles assigned to the new owner can be either specified in the request body or by default, they will be the owner roles of the team.

The original owner will remain a part of the team with older roles in the team after the transfer.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| newOwner | string |         | ✓        | ID of the new owner. |
| roles    | object |         | ✓        | Object that contains the roles of the new owner. |

#### Request

```json
{
  "id": "ripley",
  "roles": {
    "member": true
  }
}
```

#### Response

```
{
  "id": "globe_trotters",
  "name": "The Globe Trotters",
  "definition": "local",
  "created": "1937-03-01T16:30:30.088Z",
  "access": "PROTECTED",
  "owner": "ripley",
  "locked": false,
  "member_count": {
    "member": 1,
    "admin": 0,
    "super_admin": 0,
    "owner": 1
  },
  "total_members": 2
}
```

#### Errors

```
400 invalid_role: The roles specified are either non-existent or the player has no roles
401 access_denied: Player not authorized to transfer ownership. Only the team owner can perform this action.
```


## Update Team Settings

```
PATCH /teams/:id
```

Update team settings.

Only the owner can perform this action.

Only the team name and access setting can be updated. The new values should be sent as an object in the request body.

The updated state of the team is returned.

#### Parameters

|   Name    | Type   | Default | Required |   Description   |
|-----------|--------|---------|----------|-----------------|
| groupname | string |         | ✓        | The required team name. |
| access    | object |         | ✓        | The required team access setting. Valid access settings: PUBLIC, PROTECTED, PRIVATE. |

  
#### Request

```json
{
  "access": "PUBLIC"
}
```

#### Response

```
{
  "id": "guardians",
  "name": "The Gaurdians",
  "definition": "immortals",
  "created": "2014-03-01T16:29:30.088Z",
  "access": "PUBLIC",
  "owner": "star_lord",
  "locked": false,
  "member_count": {
    "God": 2
  },
  "total_members": 2
}
```

#### Errors

```
401 access_denied: Player not authorized. Only team owner can update the team.
400 invalid_access_setting: Requested access setting is invalid.
```


## Disband a Team

```
DELETE /teams/:team_id
```

Disband a team.

All members of the team will be removed from the team.

A confirmation message for the event is returned.

#### Response

```json
{
  "message": "Team 'avengers' disbanded successfully"
}
```

#### Errors

```
401 access_denied: Player not authorized. Only team owner can disband a team.
```


## Lock a Team

```
POST /teams/:primary/lock
```

Lock a team. Locking a team prevents new players from joining/leaving the team.

The player must have the required roles granting the permission to lock/unlock the team.

#### Response

```json
{
  "id": "globe-trotters",
  "name": "The Globe Trotters",
  "definition": {
    "id": "basketball_team",
    "name": "Basketball Team"
  },
  "created": "2013-11-10T16:49:10.570Z",
  "access": "PUBLIC",
  "owner": {
    "id": "clone",
    "alias": "Clonie"
  },
  "locked": true,
  "member_count": [
    {
      "name": "member",
      "count": 10
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
  "total_members": 11,
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

#### Errors

```
401 access_denied: Player does not have the required permissions to lock the team
```


## Unlock a Team

```
DELETE /teams/:primary/locks/:id
```

Unlock a team. Unlocking a previously locked team allows new members to join the team again.

The player must have the required roles granting the permission to lock/unlock the team.

#### Response

```json
{
  "id": "globe-trotters",
  "name": "The Globe Trotters",
  "definition": {
    "id": "basketball_team",
    "name": "Basketball Team"
  },
  "created": "2013-11-10T16:49:10.570Z",
  "access": "PUBLIC",
  "owner": {
    "id": "clone",
    "alias": "Clonie"
  },
  "locked": false,
  "member_count": [
    {
      "name": "member",
      "count": 10
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
  "total_members": 11,
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

#### Errors

```
401 access_denied: Player does not have the required permissions to unlock the team
```


## Get a Team's Activity Feed

```
GET /teams/:primary/activities
```

Get the team activity feed.

By default, if start and end are not defined, the activity feed of the last 24 hours is returned.

The start and end timestamps can be specified as an ISO Timestamp.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| start    | date   |         |          | Earliest possible activity timestamp in ISO format. |
| end      | date   |         |          | Latest possible activity timestamp in ISO format. |

#### Response

```json
[
  {
    "event": "invite:accept",
    "timestamp": "2014-03-02T12:34:34.474Z",
    "actor": {
      "id": "morpheus",
      "alias": "Morpheus"
    },
    "inviter": {
      "id": "neo",
      "alias": "Neo"
    },
    "roles": {
      "Member": true
    },
    "id": "02aefca0-a207-11e3-b0b2-6be1ca7db7ba"
  }
]
```

#### Errors

```
400 invalid_date: Requested date format is invalid.
```


## Send a Team Invitation

```
POST /teams/:primary/invites
```

Invite a player to join a team.

The player must have a role with peer or assign permissions and the required rank to send the invite.

It returns the invitation request.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| player   | string |         | ✓        | The ID of the player to whom the invitation has to be sent to. |
| roles    | object |         | ✓        | The desired roles for the player. |


#### Request

```json
{
  "id": "neo",
  "roles": {
    "Member": true
  }
}
```

#### Response

```json
{
  "id": "1b669910-a257-11e3-92be-0395a5e3aad0",
  "event": "invite",
  "timestamp": "2014-03-02T22:07:55.681Z",
  "actor": {
    "id": "morpheus",
    "alias": "Morpheus"
  },
  "invitee": {
    "id": "neo",
    "alias": "Neo"
  },
  "team": {
    "id": "5313986386e830697700910c",
    "name": "The Hackers"
  },
  "roles": {
    "Member": true
  },
  "state": "PENDING"
}
```

#### Errors

```
401 access_denied: Player not authorized to send an invite.
409 member_exists: Request player already a member of the team.
401 team_locked: Cannot perform the operation when the team is locked.
400 invalid_role: Requested roles are invalid.
409 already_invited: An invitation has already been sent to the requested player.
```


## Cancel a Sent Invitation

```
DELETE /teams/:primary/invites/:invite_id
```

Cancel a pending invite sent out by the player.

It returns the canceled invitation request.

#### Response

```json
{
  "event": "invite",
  "timestamp": "2014-03-02T18:03:59.848Z"
  "team": {
    "id": "hackers",
    "name": "Hackers"
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
    "Member": true
  },
  "state": "CANCELLED",
  "id": "052de920-a174-11e3-b581-1b9a3fec215b"
  "cancelled_at": "2014-03-02T18:04:08.507Z"
}
```

#### Errors

```
404 invalid_invite: Invalid invite_id.
```


## List Pending Team Join or Role Change Requests

```
GET /teams/:id/approvals
```

Get a list of pending player requests in a team.

A pending request can be approved by a player who is already a part of the team with the required permissions.

A player who cannot approve any new player to join the team will get an error when he requests the list of approvals.

#### Response

```json
{
  "data": [
    {
      "event": "join:request",
      "timestamp": "2014-03-01T18:58:22.142Z",
      "actor": {
        "id": "morpheus",
        "alias": "Morpheus"
      },
      "roles": {
        "Member": true
      },
      "state": "PENDING",
      "id": "75d485e0-a173-11e3-b581-1b9a3fec215b"
    },
    {
      "event": "join:request",
      "timestamp": "2014-03-02T18:11:56.928Z",
      "actor": {
        "id": "trinity",
        "alias": "Trinity"
      },
      "roles": {
        "Member": true
      },
      "state": "PENDING",
      "id": "24205800-a236-11e3-9af5-753d0e60669a"
    }
  ],
  "total": 2
}
```

#### Errors

```
401 access_denied: Player does not have sufficient permissions to view team requests.
```


## Approve Team Join or Role Change Request

```
PATCH /teams/:primary/approvals/:request_id
```

Accept a player request to join a team.

The player who accepts the request must have sufficient privileges to do so.

It returns the accepted approval request.

#### Response

```json
{
  "event": "join:request",
  "timestamp": "2014-03-02T21:19:43.002Z",
  "actor": {
    "id": "trinity",
    "alias": "Trinity"
  },
  "team": {
    "id": "hackers",
    "name": "The Hackers"
  },
  "roles": {
    "God": true
  },
  "state": "ACCEPTED",
  "id": "5f3acfa0-a250-11e3-9af5-753d0e60669a",
  "accepted_by": {
    "id": "morpheus",
    "alias": "Morpheus"
  },
  "accepted_at": "2014-03-02T22:03:06.948Z"
}
```

#### Errors

```
404 invalid_approval_request: Invalid request_id. There is no such request.
401 team_locked: Cannot perform operation when team is locked.
401 access_denied: Player is not authorized to approve the request.
```


## Reject Team Join or Role Change Request

```
DELETE /teams/:primary/approvals/:request_id
```

Reject a player request to join a team.

The player who rejects the request must have sufficient privileges to do so.

It returns the rejected approval request.

#### Response

```json
{
  "event": "join:request",
  "timestamp": "2014-03-02T20:45:31.317Z",
  "actor": {
    "id": "neo",
    "alias": "Neo"
  },
  "team": {
    "id": "hackers",
    "name": "The Hackers"
  },
  "roles": {
    "God": true
  },
  "state": "REJECTED",
  "id": "98548650-a24b-11e3-9af5-753d0e60669a",
  "rejected_by": {
    "id": "morpheus",
    "alias": "Morpheus"
  },
  "rejected_at": "2014-03-02T20:45:47.679Z"
}
```

#### Errors

```
404 invalid_approval_request: Invalid request_id.
401 access_denied: Player is not authorized to reject the request.
```

