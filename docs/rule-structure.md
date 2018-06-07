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
| metric      | This is the set metric a player will achieve if he statifies the requirements. Click here to see the metric structure. |
| items       | The items of the set metric that the player will gain. Each item will contain the properties Click here to see the item structure. |
| requires    | A set of conditions, based on which the achievement will be awarded to players. Click here to see the condition structure. |


