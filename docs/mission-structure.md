---
title: Mission Structure
---

There are two parts to a mission:

* the design, which we call the definition of the mission, and
* the instance, which is created by the players when they are in the game

Both the definition and the instance have very separate structures. The definition structure is as follows:


## Mission Definition Structure

The design structure is what is used when you are making the mission.

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the mission. |
| name        | A name for the mission. |
| description | A brief description of the mission. |
| image       | An image which represents the mission. |
| settings    | Settings for the whole mission. See below Settings section for details. |
| lanes       | Lanes can be used to retrict players to a particular task/tasks. Each lane has a structure described in the Lane Structure section below |
| id          | A unique ID for the mission. |


### Settings Structure

|    Field    |    Description    |
|-------------|-------------------|
| access      | The access settings with which the mission instance can be created. It is an array which can contain any or all of these PUBLIC, PROTECTED, PRIVATE. When players create an instance of this mission, they get the chance to choosing only one of the access settings you set in the array. |
| max_global_instances | The maximun number of mission instances that can be created from this mission. |
| max_active_global_instances | The maximun number of active mission instances that can be created from this mission. |
| max_player_instances | The maximum number of mission instances that a player can create from this mission. |
| max_active_player_instances | The maximum number of active mission instances that a player can create from this mission. |
| requires. | A set of conditions, which will determine if the player can see this definition in the game or not. For more information on requires, see the [Requires Structure](requires-structure.md). |


### Lanes Structure

|    Field    |    Description    |
|-------------|-------------------|
| name        | The name of the lane. |
| default     | If true, all players who join an instance of this mission will automatically join this lane. |


### Activity Structure

Activities are the tasks or sub-missions within a mission. They can have rewards attached to them.

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the activity. |
| name        | Name of the activity. |
| type        | Whether the activity is a task or a sub-mission. |
| lane        | The lane in which the activity belongs to. |
| loop        | Number of times a player can perform this task. <br />If loop is not set, the player can perform the task once.<br /> If loop is set to 10, the player can perform the task 11 times. |
| rewards     | The rewards which the player can earn upon completing this task. |
| requires    | The requirements for performing the task. |
| probability | Chance that the player will get any of the rewards on completing the task. |


### Gateway Structure

Gateways are used to restrict access to certain tasks and sub-missions based on the mission state. They cannot have rewards on them.

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the gateway. |
| name        | Name of the gateway. |
| type        | The gateway can be either parallel or exclusive. |
| lane        | The lane in which the activity belongs to. |


### Sequence Flow Structure

Sequence flows are lightweight objects which connect other nodes (activities, gateways) to each other. Each sequence flow has a source node and a target node.

|    Field    |    Description    |
|-------------|-------------------|
| src         | The node from which the sequence flow originates. |
| target      | The node to which the sequence flow ends. |
| retry       | If set to true, the player can retry a task if he fails. Default is false. |
| lane        | The lane in which the activity belongs to. |



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
