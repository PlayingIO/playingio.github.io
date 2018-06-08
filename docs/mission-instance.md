---
title: Mission Instance
---

The mission instance is created with the mission definition by the players in the game.

## Mission Instance Structure

### Containers

They represent missions or sub-missions. They must contain a field key which must be unique.

|    Field    |    Description    |
|-------------|-------------------|
| key         | Represents a path to a mission or a sub-mission. If the key is the mission instance ID then it represents the mission instance container otherwise it represents sub-missions. The key is used to trigger tasks in sub-missions without acutally specifying the full path to the task. |
| nodes       | These represent all available nodes within the mission or sub-mission. The node can be of 2 types: <ul><li>Task or Sub-Mission</li><li>Gateway</li></ul> |

### Mission Instance Task Structure

|    Field    |    Description    |
|-------------|-------------------|
| id          | The ID of the task or sub-mission. This represents the path to the mission or sub-mission. |
| state       | Can be one of three states:<ul><li>**READY** means the player can perform the task</li><li>**COMPLETED** means the player has finished the task</li><li>**ACTIVE** means a task which has been performed but not completed (looped tasks), or a sub-mission in which only some of the tasks have been completed.</li></ul> |
| loop        | The number of times the player has perfomed this task. |
| perfomers   | The players within this task who have performed this task at least once. Each performer has the following keys: <ul><li>id: The ID of the performer.</li><li>scopes: The custom leaderboard scopes the performer had performed this task with.</li></ul> |

### Mission Instance Gateway Structure
|    Field    |    Description    |
|-------------|-------------------|
| id          | The ID of the gateway. |
| routes      | It represents all the tasks or sub-mission connected to this gateway. It is an array in which each element contains:<ul><li>id: The ID of the nodes to which the gateway is connected to. Its value is true if the player has performed the task connected to the gateway</li><li>enabled: If the gateway has been cleared and there are no more tasks left in its routes then this is false otherwise its true.</li><li>performers: An array of performers (players) within this task who have been thorough this gateway</li></ul>The performers key is an array of performers, each performer being an object with only one key:<ul><li>id: the ID of the performer.</li></ul> |
