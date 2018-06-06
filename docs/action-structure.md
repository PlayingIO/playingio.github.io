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

### Requires Structure

This allows certain checks to be performed to make sure a player can actually see/perform/join the task, action, team.

|    Field    |    Description    |
|-------------|-------------------|
| type        | The type of condition. It can be one of **metric**, **action**, **team**, **and**, **or**. |
| not         | If true, inverts the condition. So any check which would usually evaluate to true will now evaluate to false. |
| expression  | An array of contexts joined with an AND or OR operator. This field is only applicable if the condition type is and or or, and replaces the context field. |
| context     | The actual comparison expression. The structure changes depending on the type of condition selected. The structure for each condition is defined below: <ul><li>**metric** - Metric Based Condition</li><li>**action** - Action Based Condition</li><li>**team** - Team Based Condition</li><li>**time** - Timed Condition</li><li>**var** - Formula Based Condition</li></ul>The **and** and **or** type of conditions do not have this field. Instead, they have the expressions field, which is an array of these contexts. |

#### Metric Based Condition

Condition which depends on a player's score for a particular metric

|    Field    |    Description    |
|-------------|-------------------|
| id          | ID of the metric. |
| type        | Type of the metric being used for comparison. <br/>For set metrics, there is an additional item field which denotes the set item |
| item        | The set item whose value will be compared. <br/>This field is only available if the metric type is set. |
| operator    | Can be one of the relational operators: <ul><li>**eq** (equal to),</li><li>**ne** (not equal to),</li><li>**gt** (greater than),</li><li>**ge** (greater than or equal to),</li><li>**lt** (less than),</li><li>**le** (less than or equal to)</li></ul> |
| value       | The value of the metric that the player should have on his scores. |

#### Action Based Condition

Condition which depends on how many times an action has been performed.

|    Field    |    Description    |
|-------------|-------------------|
| id          | ID of the action. |
| operator    | Can be one of the relational operators: **eq**, **ne**, **gt**, **ge**, **lt**, **le**. |
| value       | The number of times the action should be executed by the player. |

