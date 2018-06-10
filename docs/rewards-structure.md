---
title: Rewards Structure
---

### Reward Structure

|    Field    |    Description    |
|-------------|-------------------|
| metric      | The metric which will be used for the reward. This is an object with two fields:</br><ul><li>type: the type of the metric</li><li>id: The ID of the metric</li><ul> |
| verb        | Defines which operation is performed for this reward. Can be one of **add**, **remove**, **set**.
| value       | The value by which the player's score changes. |
| probabilty  | The chance that this reward in an action or mission task can be given must be within [0, 1] range. |
