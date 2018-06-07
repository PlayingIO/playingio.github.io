---
title: Rule Structure
---

## Base Structure

All rules have these common fields. Extra fields are required depending on what type of rule it is.

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the rule. |
| name        | A name for the rule. |
| description | Brief description of the rule. |
| type        | The type of rule. Can be one of **level**, **achievement** or **custom**. |


## Achievement Rule Structure

### Achievement Structure

|    Field    |    Description    |
|-------------|-------------------|
| metric      | This is the set metric a player will achieve if he statifies the requirements. |
| items       | The items of the set metric that the player will gain. Each item will contain the properties. |
| requires    | A set of conditions, based on which the achievement will be awarded to players. For more information on requires, see the [Requires Structure](requires-structure.md). |


#### Achievement Item Structure

|    Field    |    Description    |
|-------------|-------------------|
| key         | The name of the set item. |
| value       | Number of this item the player would gain. |


## Level Rule Structure

|    Field    |    Description    |
|-------------|-------------------|
| base_metric | This is the id of the point metric which you would be using to calculate the levels. |
| level_metric| This is the id of the state metric which will supply the list of states to be used as levels. |
| levels      | This is an array of items, each item having one state item (from the level_metric) and the maximum threshold (base_metric) when that state item is applied to players. |


### Level Rule Item Structure

|    Field    |    Description    |
|-------------|-------------------|
| rank        | The name of the state you would like to assign. |
| threshold   | The max value of the base_metric (upper threshold) that is required by the player to gain this level. The lower threshold for each is determined as one more than the previous level's threshold. The lower threshold for the first level is hard-coded to -Infinity. |

### Example

```json
{
  "name": "Boost Levels",
  "id": "boost",
  "description": "The levels a player can rise up to based on experience",
  "type": "level",
  "base_metric": "experience",
  "level_metric": "levels",
  "levels": [
    {
      "rank": "Level 1",
      "threshold": "15"
    },
    {
      "rank": "Level 2" // if no threshold is specified it goes till Infinity
    }
  ]
}
```

## Custom Rule Structure

### Rules

The rules that would be evaluated to give rewards to the player. This is an array consisting of objects with the fields rewards and requires.

|    Field    |    Description    |
|-------------|-------------------|
| rewards     | The set of metrics that a player gets when he finishes this action. This is a an array consisting of one or more rewards. For more information on rewards, see the [Rewards Structure](rewards-structure.md). |
| requires    | These are the conditions which are checked to see if the player is suitable to get this reward. For more information on requires, see the [Requires Structure](requires-structure.md). |
