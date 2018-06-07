---
title: Requires Structure
---

## Requires Structure

This allows certain checks to be performed to make sure a player can actually see/perform/join the task, action, team.

|    Field    |    Description    |
|-------------|-------------------|
| type        | The type of condition. It can be one of **metric**, **action**, **team**, **and**, **or**. |
| not         | If true, inverts the condition. So any check which would usually evaluate to true will now evaluate to false. |
| expression  | An array of contexts joined with an AND or OR operator. This field is only applicable if the condition type is and or or, and replaces the context field. |
| context     | The actual comparison expression. The structure changes depending on the type of condition selected. The structure for each condition is defined below: <ul><li>**metric** - Metric Based Condition</li><li>**action** - Action Based Condition</li><li>**team** - Team Based Condition</li><li>**time** - Timed Condition</li><li>**var** - Formula Based Condition</li></ul>The **and** and **or** type of conditions do not have this field. Instead, they have the expressions field, which is an array of these contexts. |

## Metric Based Condition

Condition which depends on a player's score for a particular metric

|    Field    |    Description    |
|-------------|-------------------|
| id          | ID of the metric. |
| type        | Type of the metric being used for comparison. <br/>For **set** metrics, there is an additional item field which denotes the set item |
| item        | The set item whose value will be compared. <br/>This field is only available if the metric type is set. |
| operator    | Can be one of the relational operators: <ul><li>**eq** (equal to),</li><li>**ne** (not equal to),</li><li>**gt** (greater than),</li><li>**ge** (greater than or equal to),</li><li>**lt** (less than),</li><li>**le** (less than or equal to)</li></ul> |
| value       | The value of the metric that the player should have on his scores. |

Example:

```json
{
  "requires": {
    "type": "metric",
    "not": false,
    "context": {
      "id": "exp",
      "type": "point",
      "operator": "eq",
      "value": "55"
    }
  }
}
```

## Action Based Condition

Condition which depends on how many times an action has been performed.

|    Field    |    Description    |
|-------------|-------------------|
| id          | ID of the action. |
| operator    | Can be one of the relational operators: **eq**, **ne**, **gt**, **ge**, **lt**, **le**. |
| value       | The number of times the action should be executed by the player. |

Example:

```json
{
  "requires": {
    "type": "action",
    "context": {
      "id": "login",
      "operator": "eq",
      "value": "15"
    }
  }
}
```

## Team Based Condition

Condition which depends on how many times an action has been performed.

|    Field    |    Description    |
|-------------|-------------------|
| definition  | Definition ID of the team which the player should be a part of. |
| role        | The role the player should have within the team of this type. If you don't care about the role, then you can ignore this field. |

Example:

```json
{
  "requires": {
    "type": "team",
    "not": false,
    "context": {
      "definition_id": "disciples",
      "role": "student" // optional if the player can have any role
    }
  }
}
```

## Timed Condition

You can also define conditions which become true only when triggered on a particular time.

|    Field    |    Description    |
|-------------|-------------------|
| func        | The time unit to be counted, against a fixed duration. The available choices are: <ul><li>hour_of_day</li><li>day_of_week</li><li>day_of_month</li><li>day_of_year</li><li>week_of_year</li><li>month_of_year</li></ul> |
| operator    | Can be one of the relational operators: **eq**, **ne**, **gt**, **ge**, **lt**, **le**. |
| value       | The count of the unit to be counted chosen in the func field. For example, for day of week, the value will represent day, and sensible values will be an integer between 1 to 7, both included. |

## Formula Based Condition

You can also setup equations with two sides, a left-hand expression (LHS), a right-hand expression (RHS) and an operator. Whenever the equation is satisfied, the condition becomes truthy.

|    Field    |    Description    |
|-------------|-------------------|
| lhs         | An arithmetic expression, which can use other metrics or variables defined within the custom rules as components. An example:  $scores.health + $vars.run_length |
| operator    | Can be one of the relational operators: **eq**, **ne**, **gt**, **ge**, **lt**, **le**. |
| rhs         | Another arithmetic expression, same as the lhs. |


## All Conditions: and

An and condition, which is shown as All of the following rules can be used to join two or more conditions such that the whole condition will be true only when all of the child conditions are true.

Example:

```json
{
  "requires": {
    "type": "and",
    "expression": [
      // two or more conditions
    ]
  }
}
```

## Any Condition: or

An or condition, which is shown as Any of the following rules can be used to join two or more conditions such that the whole condition will be true even if only one of the child conditions become true.

Example:

```json
{
  "requires": {
    "type": "or",
    "expression": [
      // two or more conditions
    ]
  }
}
```
