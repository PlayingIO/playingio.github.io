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


