---
title: Actions Service
---

## List All Actions

```
GET /actions
```

Get a list of all available actions.

#### Response

```json
[
  {
    "id": "collect_coin",
    "name": "collect a coin",
    "description": "Hit the brick and collect a coin",
    "actions": [
      {
        "metric": {
          "id": "coins",
          "type": "point",
          "name": "coins"
        },
        "value": "1",
        "verb": "add",
        "probability": 1
      }
    ]
  },
  {
    "id": "rescue",
    "name": "rescue_princess",
    "description": "Rescue the princess",
    "actions": [
      {
        "metric": {
          "id": "coins",
          "type": "point",
          "name": "coins"
        },
        "value": "300",
        "verb": "add",
        "probability": 1
      }
    ]
  }
]
```


## Play an Action

```
POST /user-actions/:action_id/play
```

Execute an action with the given id. The response contains the changes that resulted from performing the action along with the currently available actions.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| count    | number | 1       |          | Number of times the action has to be executed. |
| variables| object | {}      |          | Additional variables required by the action. This is a JSON object where each key is the variable name and the value is the variable value. |
| scopes   | string | {}      |          | The custom scopes in your Custom Leaderboards in which the scores need to be updated. This is an array of JSON objects where each object contains an id key which is the scope id and an entity_id key which is the player or team instance id. |

#### Response

```json
{
  "actions": [
    {
      "id": "collect_coin",
      "name": "collect a coin",
      "description": "Hit the brick and collect a coin",
      "rewards": [
        {
          "metric": {
            "id": "coins",
            "type": "point",
            "name": "coins"
          },
          "value": "1",
          "verb": "add",
          "probability": 1
        }
      ]
    },
    {
      "id": "rescue",
      "name": "rescue_princess",
      "description": "Rescue the princess",
      "rewards": [
        {
          "metric": {
            "id": "coins",
            "type": "point",
            "name": "coins"
          },
          "value": "300",
          "verb": "add",
          "probability": 1
        }
      ]
    }
  ],
  "events": {
    "local": [
      {
        "event": "action",
        "actor": {
          "id": "Mario",
          "alias": "SuperM"
        },
        "changes": [
          {
            "metric": {
              "id": "coins",
              "name": "coins",
              "type": "point"
            },
            "delta": {
              "old": "0",
              "new": "300"
            }
          }
        ],
        "timestamp": "2014-03-01T16:29:30.088Z",
        "id": ""
      }
    ],
    "global": [
      {
        "event": "level",
        "actor": {
          "id": "Mario",
          "alias": "SuperM"
        },
        "changes": [
          {
            "metric": {
              "id": "rescues",
              "name": "badges",
              "type": "set"
            },
            "delta": {
              "novice": {
                "old": null,
                "new": "1"
              }
            }
          }
        ],
        "timestamp": "2014-03-01T16:29:30.088Z",
        "id": ""
      }
    ]
  }
}
```

#### Errors

```
401 access_denied: Player does not have sufficient permissions to play/trigger the action
```