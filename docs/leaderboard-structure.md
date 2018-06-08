---
title: Leaderboard Structure
---

## Base Structure

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the leaderboard. |
| name        | A name for the leaderboard. |
| description | Brief description of the leaderboard. |
| metric      | The metric on which this leaderboard is based upon. This is an object with two fields:<ul><li>type: the type of the metric (either point or compound)</li><li>id: The ID of the metric</li></ul> |
| scope       | The scope on which this leaderboard operates. <br/>The scope is decided by the type property in this object. The four supported scope types are:<ul><li>game: Leaderboard is game-wide.</li><li>team_definition: Leaderboard is across all instances of a team definition.</li><li>team_instance: Leaderboard is restricted to each instance of a team definition.</li><li>custom: Leaderboard is free-form. You can decide what contributions you want to count on the leaderboard.</li></ul>If the scope type is one of team_definition or team_instance, then an additional id field is required within the scope object.<ul><li>For type: team_definition: The id field will be the team definition this leaderboard will apply to.</li><li>For type: team_instance: The id field will be the team definition whose instances this leaderboard will be restricted to.</li></ul> |
| type        | The ranking type of the leaderboard. Only regular for now. |
| entity_type | The type of leaderboard whether its for players or teams.<br/>Player leaderboards can be on all four scopes, where as the Team leaderboards are possible only with game and team_definition scopes.|
| cycles      | Leaderboard cycles are the interval in which the leaderboard is reset. Your leaderboard design can have more than one cycles. There are 5 possible cycles:<ul><li>alltime: Leaderboards are never reset.</li><li>yearly: The current leaderboard is reset at the start of every year.</li><li>monthly: The current leaderboard is reset at the start of every month.</li><li>weekly: The current leaderboard is reset on every Sunday.</li><li>daily: The current leaderboard is reset at midnight GMT.</li></ul>When you are finally reading the leaderboard (in your game), you'd pass only one of the selected cycles in your request. |
| requires    | The visibility requirements for the leaderboard. Only the players who satisfy the requirements can view the leaderboard. For more information on requires, see the [Requires Structure]. |
| tags        | Array of strings which can be used when querying to get all designs based on certain tags specified |
