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

