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



## Level Rules

A level rule is used to determine the level of a player. The level rule basically changes the state of a player based on various thresholds on a given point metric.

A level rule has 3 parts:

* Base Metric - A point metric which is checked to determine the level to be assigned. The 'minimum' and 'maximum' constraints of the base metric determines the threshold value range.
* Level Metric - A state metric whose preset states represent the different levels.
* Levels - An ordered list of levels. Each level consists of a state and a threshold value. A player achieves a level when they cross the threshold for the previous level in the list.

### Use Cases

You can create an experience based level system using level rules. You could also use them to show different reward tiers within a game.

Caution: Level rules only work if a player has a metric, so in case you want a player to have a certain level after creation, you might want to consider bootstrapping the base metric.



## Custom Rules

Unlike the other two rules, where the rewards were fixed at either a set or a state metric, there are no such restrictions in a custom rule.

You can also do a lot more as they have variables that can be passed during runtime, which can be used to modify your conditions or rewards on the fly.

Lastly, they need to be triggered independently via an API call and can be applied on multiple players. You can have a look at our API documentation on custom rules for the details. To know more about how rewards are applied in custom rules, click here

A custom rule has 2 parts:

* Variables - Variables that can be passed in during runtime and can be used in conditions for rewards or in rewards themselves. Variables can be of 2 types - either strings or integers.
* Rewards - Rewards are a set of scores and conditions where the scores are given to a player if he satisfies the given conditions.

### Use Cases

There might be scenarios where using an action might not be the right, specially for some ad-hoc tasks or accounting for some unexpected behaviour.

In a classroom model, you might be doing quizes and giving homework, both are a great fit for actions, but what if you want to give special rewards to those who have been consistent? You can create a custom rule that can be triggered every week to do this check.

Similarly, in sales teams, recording sales, leads, calls, etc are easy to do via actions. Now if you want to check if people have reached their targets or give out bonuses to those who exceeded them, custom rules can easily do the job.

### Rewards in Custom Rules

Like mentioned earlier, there are no restrictions on the scores that can be given to players via custom rules. These scores will be given everytime a player satisfies the reward conditions and there is no concept of chance/probability in custom rules.

Conditions in custom rules are very similar to that of actions, here is the break down of the different kinds of conditions that can be put into place:

|    Type    | Condition to select | Description |
| Metric based condition | If the player has the metric | Metric based conditions can be used to check the value of a metric against a player's scores. e.g., A player with a certain badge or having score higher than 450, etc. |
| Action count based condition | If an action has been triggered N times | Action count based conditions check against the number of times the player has performed the specified action. |
| Time based condition | If the following timed condition is satisfied | Time based conditions can be used to check against the time the action was performed. These comparisons are done taking into account the time zone of the game. |
| Formula Based | If the following equation is satisfied | We can use variables and player's scores in conditions. In this kind of condition, we can specify both the LHS and RHS of an equation. |
| Team based condition | If the player is a part of the team | Team based conditions are used to check against the player's role in a team. |
| Aggregate conditions - All | All of the following rules | Various conditions can be combined by an AND condition. |
| Aggregate conditions - Any | Any of the following rules | Various conditions can be combined by an OR condition. |

