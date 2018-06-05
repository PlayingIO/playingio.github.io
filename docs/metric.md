---
id: metric
title: Metrics
sidebar_label: Metrics
---

## Metrics

Metrics are used to measure the performance of the players in your game, of teams, and even of your whole game. Normally, you'd setup your metrics to capture and quantify a specific behaviour of your players, so that you can later assess how well your players are being driven. There are four kinds of metrics Playing I/O offers you. These are:



### Point Metrics

> The simplest form of metrics, they are found in most games. They are just a numerical value which increases/decreases when players perform actions. They are used to keep track of player progress.

Points can be given to a player by performing an action, a task in a process or by executing custom rules.

Point metrics can be used to create leaderboards. You can also model level systems by combining a point metric and a state metric in a level rule.

#### Use Cases

Point metrics are ideal to capture any kind of progress or for keeping track of any activity.



### Set Metrics

> A set metric is a group of unordered items.

While designing set metrics, you need to define preset items.

### Use Cases

There are several ways you can use sets.

* You can use set items as badges to represent a player's progress in various aspects of your game.
* You can use set items as tags or inventory items on a player.
* You can combine set items with achievement rules to make player achievements.



## State Metrics

> A state metric indicates a particular state which the player is currently in.

While designing state metrics, you need to define preset states, which are essentially the various states that a player can take.

### Use Cases

A state metric can be used to associate a state with a player. This could be as simple as active, online, banned, etc or can be combined with our level rules to create a player leveling system.



## Compound Metrics

> This a special type of a metric who's value depends on other metrics.

Unlike other metrics, which can be modified directly, compound metrics can never be directly modified (i.e. you cannot add to or remove from a compound metric score). Their value is always computed from an expression.

This metric has a formula constraint, which is the expression that is used to calculate the value of the compound metric, can reference a player's scores along with constants in the formula. Only point, set and compound metric scores can be referenced in the formula.

Compound metrics can be used to create leaderboards and can also be used in rules, similar to point metrics.

### Use Cases
Compound metrics work well if you need a consolidated score that represent various factors.

For example, in a sales and marketing team, it would be unfair to judge someone based on a single metric. The performance metric should reflect several factors such as sales, leads, calls made, promotional tweets made and so on.

Compound metrics let you aggregate the effect of all of these into one, meaningful metric.