---
title: Leaderboards
---

## Leaderboards

Leaderboards are a system which rank players based upon their scores.

Playing I/O provides realtime leaderboards which are automatically updated as and when there is any player activity. No extra calls are required to update leaderboards.

### Use Cases

At Playing I/O, we love leaderboards. It's a great way of getting a social element into the game, and driving competetive behaviour. Our detailed leaderboards can also provide insight on player activity and let players know how they can improve their performance.

Leaderboards fit well in almost all scenarios, but lets just take an example of a sales and marketing team. If you want to track the overall performance of a sales executive, the various metrics that you might be interested are the total revenue generated, sales calls made, number of new clients, etc. You can have different leaderboards tracking each of these metrics, but Playing I/O can also let you make a leaderboard track a compound metric that depends on these metrics. This gives a better overall pricture. The leaderboards can also be extended to teams, where teams compete against each other and take things to another level.


## Base Metric

Leaderboards are set to calculate rankings on a preset Base Metric. Players with a higher score on the base metric will have a higher rank. The base metric can be a point or a compound metric.


## Team Leaderboards

You can have leaderboards on teams too. To create a team leaderboard, set the This Leaderboard is for field to Teams (this corresponds to the entity_type key in the design). In team leaderboards, a team's score is updated when any of its members score points. Only the scores which a member earns after joining the team are counted.

> A team's score on the leaderboard is not the sum of all the scores of its members.


## Leaderboard Cycles

With cycles, leaderboards can be reset at regular intervals which lets you find the top performers in a cycle.


## Leaderboard Scopes

The scope of a leaderboard defines who all will be tracked on the leaderboard. If the scope type is team_definition or team_instance, then an additional scope.id field is required which tells which team definition you want the leaderboard for.

For player leaderboards, there can be 4 scopes:

* Game: Leaderboard is game-wide.
* Across All Teams of Type(team_definition): Leaderboard is across all instances of a team definition.
* Within Each Team of Type(team_instance): Leaderboard is restricted to each instance of a team definition.
* Custom: Leaderboard is free-form. You can decide what contributions you want to count on the leaderboard.

For team leaderboards, there can be only 3 scopes:

* Game: Leaderboard is game-wide.
* Across All Teams of Type(Team Definition): Leaderboard is across all instances of a team definition.
* Custom: Leaderboard is free-form. You can decide what contributions you want to count on the leaderboard.


