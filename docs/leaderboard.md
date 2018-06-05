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


