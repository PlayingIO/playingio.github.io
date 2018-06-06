---
title: Action Structure
---

## Base Structure

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the action. |
| name        | A name for the action. |
| description | Brief description of the action. |
| image       | An image which represents the action. |
| probability | The probability that the player gets the rewards on completing the action. |
| rate        | Rate Limiting an action so that users do not abuse the system.<br/>This is an array of three items, which are:</br><ul><li>Count - The number of times the player can perform this action within the window.</li><li>Timeframe - Counted in milliseconds, this is the window or timeframe within which the player can perform this action Count number of times.</li><li>Type - The type of rate limiting being used, which can be one of **ROLLING**, **FIXED**, **LEAKY**.</li></ul> |

## Rules

The rules that would be evaluated to give rewards to the player. This is an array consisting of objects with the fields rewards and requires.

|    Field    |    Description    |
|-------------|-------------------|
| rewards     | The set of metrics that a player gets when he finishes this action. This is a an array consisting of one or more Rewards. |
| requires    | These are the conditions which are checked to see if the player is suitable to get this reward. For more information on requires, see the Requires Structure below. |

### Reward Structure
|    Field    |    Description    |
|-------------|-------------------|
| metric      | The metric which will be used for the reward. This is an object with two fields:</br><ul><li>type: the type of the metric</li><li>id: The ID of the metric</li><ul> |
| verb        | Defines which operation is performed for this reward. Can be one of add, remove, set.
| value       | The value by which the player's score changes. |
| probabilty  | The chance that this reward in an action or process task can be given must be within [0, 1] range. |

