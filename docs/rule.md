---
title: Rules
---

## Rules

A rule checks players for certain conditions and performs a specific action on the players if they satisfy those conditions. The action taken by the rule depends on its type.


## Achievement Rules

An achievement rule awards a specific achievement to a player when they satisfy all the conditions.

> Achievement rules are ideal for giving away badges (using set metrics) when the player meets certain conditions.

> Achievement rules are evaluated everytime a player does some action.

It consists of 2 parts:

* **Achievement** - The achievement itself, which must be an item of a set metric. A player cannot receive an achievement if he already has it. An achievement rule does not remove an awarded achievement if a player fails any check at a future time.
* **Requirements** - A list of checks. A player must pass all checks to gain the Achievement.

You can use these conditions that while defining the achievement requirements:

|    Type    | Condition to select | Description |
|------------|---------------------|-------------|
| Metric based condition | If the player has the metric | Metric based conditions can be used to check the value of a metric against a player's scores. E.g: A player with a certain badge or having score higher than 450, etc. |
| Action count based condition | If an action has been triggered N times | Action count based conditions check against the number of times the player has performed the specified action. |
| Aggregate conditions - All | All of the following rules | Various conditions can be combined by an AND condition. |
| Aggregate conditions - Any | Any of the following rules | Various conditions can be combined by an OR condition. |

### Use Cases

Apart from awarding achievements on certain conditions, you can organize achievement rules in complex hierarchies using checks. Each tier of achievements can check if the player was already awarded any achievements on the previous tier.


