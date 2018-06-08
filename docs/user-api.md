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

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| start    | date   |         |          | Earliest possible activity time stamp in ISO format|
| end      | date   |         |          | Latest possible activity time stamp in ISO format |

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


## Get Own Notifications

```
GET /users/me/notifications
```

Gets the player notifications for a particular period of time.

If no start and end keys are provided, by default, notifications from the last 24 hours will be returned.

Timestamp values must be either ISO or UNIX time stamp.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| start    | date   |         |          | Earliest possible activity time stamp in ISO format|
| end      | date   |         |          | Latest possible activity time stamp in ISO format |

#### Response

```json
{
  "data": [
    {
      "id": "052de920-a174-11e3-b581-1b9a3fec215b",
      "event": "join",
      "timestamp": "2014-03-02T18:03:59.848Z",
      "actor": {
        "id": "neo",
        "alias": "Neo"
      },
      "process": {
        "id": "matrix",
        "name": "TheMatrix"
      },
      "roles": {
        "matrix": "player"
      },
      "seen": false
    },
    {
      "id": "931dda24-a174-da23-g323-1b9adaec2ds",
      "event": "leave",
      "timestamp": "2013-11-10T16:48:10.570Z",
      "actor": {
        "id": "keymaker",
        "alias": "The Key Maker!"
      },
      "process": {
        "id": "matrix",
        "name": "TheMatrix"
      },
      "seen": false
    }
  ],
  "unseen": 2
}
```

## Mark Notifications as Read

```
POST /user/notifications/:id
```

Mark player notifications as read.

Responds with an object containing 2 keys:

* ok: Total number of notifications that were marked read.
* unseen: Total number of notifications that are still unseen.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| more     | array  |         |          | An Array containing the IDs of notifications that are to be marked as read. |

#### Request

```json
{
  "ids": ["706db9d2-5756-11e3-a219-b1df8153ecfb"]
}
```

#### Response

```json
{
  "ok": 1,
  "unseen": 0
}
```

#### Errors

```
401 invalid_notification: Invalid notification id
```


## Get Pending Approvals

```
GET /user/me/approvals
```

Get the list of pending approvals to join teams/processes for the player.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| skip     | number | 0       |          | Number of approvals to skip. |
| limit    | number | 10      |          | Maximum number of approvals to return. |

#### Response

```json
{
  "data": [
    {
      "event": "join:request",
      "timestamp": "2014-03-01T18:58:22.142Z",
      "team": {
        "id": "53122d80188101a72a668a63",
        "name": "Titans"
      },
      "roles": {
        "God": true
      },
      "state": "PENDING",
      "id": "75d485e0-a173-11e3-b581-1b9a3fec215b"
    },
    {
      "event": "join:request",
      "timestamp": "2014-03-01T19:02:22.642Z",
      "process": {
        "id": "neo/53122e9a188101a72a668a64",
        "name": "Protected"
      },
      "roles": {
        "~": "player"
      },
      "state": "PENDING",
      "id": "052de920-a174-11e3-b581-1b9a3fec215b"
    }
  ],
  "total": 2
}
```


## Get Pending Invitations

```
GET /users/me/invites
```

Get the list of pending invitations to join teams/processes for the player.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| skip     | number | 0       |          | Number of invites to skip. |
| limit    | number | 10      |          | Maximum number of invites to return. |

#### Response

```json
{
  "data": [
    {
      "event": "invite",
      "timestamp": "2014-03-01T19:04:33.209Z",
      "actor": {
        "id": "neo",
        "alias": "Neo"
      },
      "team": {
        "id": "53120aea188101a72a668a5e",
        "name": "The Gaurdians"
      },
      "roles": {
        "God": true
      },
      "state": "PENDING",
      "id": "5300da90-a174-11e3-b581-1b9a3fec215b"
    },
    {
      "event": "invite",
      "timestamp": "2014-03-01T19:05:23.452Z",
      "actor": {
        "id": "neo",
        "alias": "Neo"
      },
      "process": {
        "id": "neo/5312096a8e1ebb4550a6a6f9",
        "name": "The Matrix"
      },
      "roles": {
        "~": "player"
      },
      "state": "PENDING",
      "id": "70f353c0-a174-11e3-b581-1b9a3fec215b"
    }
  ],
  "total": 2
}
```

## Accept an Invitation

```
POST /users/me/invites/:id
```

Accept an invitation to join a process or a team.

#### Response

```json
{
  "event": "invite",
  "process": {
    "id": "gandalf/lotr",
    "name": "lotr"
  },
  "actor": {
    "id": "gandalf",
    "alias": "Gandalf The Grey"
  },
  "invitee": {
    "id": "legolas",
    "alias": "Legolas!"
  },
  "roles": {
    "gondor": "player"
  },
  "state": "ACCEPTED",
  "timestamp": "2014-08-20T05:43:30.062Z",
  "id": "53120aea188101a72a668a5e",
  "accepted_at": "2014-03-21T09:24:34.157Z"
}
```

#### Errors

```
404 invalid_invite: Thrown when an invalid invite key is provided.
401 team_locked: Thrown when the team is locked.
```


## Reject Invitation

```
DELETE /users/me/invites/:id
```

Reject an invitation to join the team or process.

#### Response

```json
{
  "event": "invite",
  "process": {
    "id": "gandalf/lotr",
    "name": "lotr"
  },
  "actor": {
    "id": "gandalf",
    "alias": "Gandalf The Grey!"
  },
  "invitee": {
    "id": "saruman",
    "alias": "Saruman!!"
  },
  "roles": {
    "gondor": "player"
  },
  "state": "REJECTED",
  "timestamp": "2014-08-20T05:43:30.062Z",
  "id": "53120aea188101a72a668a5e",
  "accepted_at": "2014-03-21T09:24:34.157Z"
}
```

#### Errors

```
404 invalid_invite: Thrown when an invalid invite key is provided.
```