---
title: Team Structure
---

## Base Structure

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the team. |
| name        | A name for the team. |
| description | Brief description of the team. |
| image       | An Image which represents the team. You need to do a multi-part form post to send the image data. |
| settings    | Settings for the whole team. See below for Settings section for details |
| permissions | An structure which defines the roles present in each team, and the permissions associated with each role. <br />This is an array of permission rows. Each permission row is an array, with two elements:<ul><li>role: The name of the role.</li><li>permissions: A map of all permissions, each having a boolean value.</li></ul>The list of permissions are as follows:<ul><li>lock: player can lock the team, so no member can join or leave the team</li><li>peer: player can invite others to the same role</li><li>assign: player can invite others to lower roles</li><li>leave: player can leave the team.</li></ul>*Note: the player here means anyone having a role with that permission. |
| creator_roles |  An array of roles which will be assigned to the player who creates an instance of this.<br/>Note: Only the roles defined in the permissions key can be used. |

### Settings Structure

|    Field    |    Description    |
|-------------|-------------------|
| access      | The access settings with which the team instance can be created. It is an array which can contain any or all of these PUBLIC, PROTECTED, PRIVATE. When players create an instance of this team, they get the chance to choosing only one of the access settings you set in the array. |
| max_global_instances | The maximum number of instances of this type that can be created. |
| max_player_instances | The maximum number of instances of this type that a player can create. |
| max_players | The maximum number of players that any instances of this type can contain. |
| public      | If false, the team definition will only be available to the game admins. Only the game admin can create an instance of the definition.<br />If true, the availability of the definition will be controlled by the requires key. If the requires key is not set, then it will be available to everyone within the game. |
| requires    | The requirements for creation of an instance from this definition. Only the players who satisfy the requirements can create this type of team. For more information on requires, see the [Requires Structure](requires-structure.md). |

## Example

### Team

```json
{
  "name": "Managers",
  "id": "managers",
  "description": "This is the team for all managers",
  "permissions": [
    ["Starter", {"lock": true, "assign": true}],
    ["Admin", {"lock": true, "assign": true, "peer": true, "leave": true }]
  ],
  "creator_roles": ["Admin"],
  "settings": {
    "access": ["PUBLIC"],
    "max_global_instances": "Infinity",
    "max_player_instances": "Infinity",
    "max_players": "Infinity",
    "public": true,
    "requires": {}
  }
}
```

### Team with Creation Requirements

```json

  "name": "Moderators",
  "id": "moderators",
  "description": "This is the team for all managers",
  "permissions": [
    ["Forum Moderator", {"lock": true, assign: true}],
    ["Chat Moderator", {"lock": true, "assign": true}],
    ["Admin", {"lock": true, "assign": true, "peer": true, "leave": true }]
  ],
  "creator_roles": ["Admin"],
  "settings": {
    "access": ["PUBLIC"],
    "max_global_instances": "Infinity",
    "max_player_instances": "Infinity",
    "max_players": "Infinity",
    "public": true,
    "requires": {
      "type": "metric",
      "not": false,
      "context": {
        "id": "levels",
        "type": "state",
        "operator": "eq",
        "value": "Level 3"
      }
    }
  }
}
```
