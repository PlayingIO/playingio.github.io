## Actions

> Actions are an easy way of capturing events on the playlyfe platform.

Actions are basic triggers that allow you to translate player events/actions into a measurable entities on playlyfe.

An action has 3 parts:

* **Rewards** - Rewards are a set of scores and conditions where the scores are given to a player if he satisfies the given conditions.
* **Variables** - Variables that can be passed in during runtime and can be used in conditions for rewards or in rewards themselves.
* **Visibility** Requirements - By default, all actions are visible to all players. By using visibility requirements, you can restrict access to actions to only those players who satisfy a given criteria.

### Use Cases

Any task done by a person can be easily recorded in playlyfe by using Actions. For example, an action could be going to the gym, reading a book, liking a facebook post or answering a quiz.

An action can be used pretty much anywhere. In case of an online education platform, action could be watching a tutorial video, which would fetch a player some experience points that can be used to show the person their progress within the course. Similarly, answering questions or posting on forums can also be actions that contribute to their progress or rating.

## Rewards

Rewards are a set of metrics (points, badges, etc.) that a player gets when an action is performed. These rewards are metrics and can be either added, removed or set on a player.

> Note: Adding negative values will reduce the player's score, similarly removing negative value increases the player's score.

Each reward can have its own conditions which are to be satisfied for the reward to be executed. More advanced scoring conditions such as probability and rate limits can also be set.

The various kinds of conditions available to you are:

|   Type  | Condition to select | Description |
|---------|---------------------|-------------|
| Metric based condition | If the player has the metric | Metric based conditions can be used to check the value of a metric against a player's scores. e.g., A player with a certain badge or having score higher than 450, etc. |
| Action count based condition | If an action has been triggered N times | Action count based conditions check against the number of times the player has performed the specified action. |
| Time based condition | If the following timed condition is satisfied | Time based conditions can be used to check against the time the action was performed. These comparisons are done taking into account the time zone of the game. |
| Team based condition | If the player is a part of the team | Team based conditions are used to check against the player's role in a team. |
| Aggregate conditions - All | All of the following rules | Various conditions can be combined by an AND condition. |
| Aggregate conditions - Any | Any of the following rules | Various conditions can be combined by an OR condition.

## Scoring Probability

A scoring probability (a.k.a chance) can be used to bring in an element of randomness or chance into the system. Scoring probability in actions can be set at 2 levels:

* Overall action level
* Per-reward level

A scoring probability at the action level would affect all the rewards of the action. It would be useful when you want to give all rewards together. The reward level probability would affect only a particular reward, hence you can control probabilities of each reward individually. Both probabilities can also be used simultaneously.