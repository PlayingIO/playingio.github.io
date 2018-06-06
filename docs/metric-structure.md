---
title: Metric Structure
---

## Base Structure

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the metric. |
| name        | A name for the metric. |
| description | Brief description of the metric.
| image       | An image which represents the metric. |
| type        | Type of the metric: one of point, set, state and compound. |
| constraints | The constraints on a metric differ depending on what type of metric it is. Refer to the linked sections for each metric type:<br/><ul><li>Point Metric Constraints</li><li>Set Metric Constraints</li><li>State Metric Constraints</li><li>Compound Metric Constraints</li></ul> |
| tags        | Array of strings which can be used when querying to get all designs based on certain tags specified |


### Point Metric Constraints

|    Field    |    Description    |
|-------------|-------------------|
| min         | The minimum value the point metric can have. |
| max         | The maximum value the point metric can have. |
| default     | The value which will be used in calculations if this metric is not already set on the player. |


### Set Metric Constraints
|    Field    |    Description    |
|-------------|-------------------|
| max_items   | The maximum number of unique items of the set that a player can have. |
| items       | An array of individual items in the set. Click here to see the properties each item can have. |

#### Set Item Properties
|    Field    |    Description    |
|-------------|-------------------|
| name        | Name of the item. |
| description | A brief description of the item. |
| image       | An image that represents the item. |
| max         | Maximum count of the item a player can get. |
| hidden      | Hidden set items will not show up in player profiles if they are not earned. Items can be marked as hidden for surprise/hidden rewards. |


### State Metric Constraints

|    Field    |    Description    |
|-------------|-------------------|
| states      | An array of individual states in the metric. Click here to see the properties each state item can have. |

#### State Item Properties

|    Field    |    Description    |
|-------------|-------------------|
| name        | Name of the state. |
| description | A brief description of the state. |
| image       | An image that represents the state. |
| max         | Maximum count of the item a player can get. |
| hidden      | Hidden set items will not show up in player profiles if they are not earned. Items can be marked as hidden for surprise/hidden rewards. |


### Compound Metric Constraints
|    Field    |    Description    |
|-------------|-------------------|
| formula     | The formula which is evaluated to get the metric's value. |


## Examples

### Point Metric

```json
{
  "name": "Experience",
  "id": "experience",
  "type": "point",
  "description": "My Players experience within the game",
  "constraints": {
    "default": "10",
    "max": "5000",
    "min": "10"
  },
  "tags": ["course_1", "game"]
}
```

### Set Metric

```json
{
  "name": "Badges",
  "id": "badges",
  "type": "set",
  "description": "My Players will get these badges",
  "constraints": {
    "items": [
      {
        "name": "Rookie",
        "description": "Just Starting"
      },
      {
        "name": "Trainee",
        "description": "Just Training"
      },
      {
        "name": "Teacher",
        "description": "Teaches Others"
      }
    ],
    "max_items": "Infinity"
  }
}
```

### State Metric

```json
{
  "name": "Levels",
  "id": "levels",
  "type": "state",
  "description": "The level my player is currently in",
  "constraints": {
    "states": [
      {
        "name": "Beginner",
        "description": ""
      },
      {
        "name": "Medium",
        "description": ""
      },
      {
        "name": "Advanced",
        "description": ""
      }
    ]
  }
}
```

### Compound Metric

```json
{
  "name": "Karma",
  "id": "karma",
  "type": "compound",
  "description": "The Karma points a player has",
  "constraints": {
    "formula": "$scores['experience'] * 10"
  }
}
```