## Actions

> Actions are an easy way of capturing events on the PlayingIO platform.

Actions are basic triggers that allow you to translate player events/actions into a measurable entities on PlayingIO.

An action has 3 parts:

* **Rewards** - Rewards are a set of scores and conditions where the scores are given to a player if he satisfies the given conditions.

* **Variables** - Variables that can be passed in during runtime and can be used in conditions for rewards or in rewards themselves.

* **Visibility** Requirements - By default, all actions are visible to all players. By using visibility requirements, you can restrict access to actions to only those players who satisfy a given criteria.

### Use Cases

Any task done by a person can be easily recorded in PlayingIO by using Actions. For example, an action could be going to the gym, reading a book, liking a facebook post or answering a quiz.

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
| Time based condition | If the following timed condition is satisfied | Time based conditions can be used to check against the time the action was performed. These comparisons are doneƒ taking into account the time zone of the game. |
| Team based condition | If the player is a part of the team | Team based conditions are used to check against the player's role in a team. |
| Aggregate conditions - All | All of the following rules | Various conditions can be combined by an AND condition. |
| Aggregate conditions - Any | Any of the following rules | Various conditions can be combined by an OR condition.

## Scoring Probability

A scoring probability (a.k.a chance) can be used to bring in an element of randomness or chance into the system. Scoring probability in actions can be set at 2 levels:

* Overall action level
* Per-reward level

A scoring probability at the action level would affect all the rewards of the action. It would be useful when you want to give all rewards together. The reward level probability would affect only a particular reward, hence you can control probabilities of each reward individually. Both probabilities can also be used simultaneously.

## Rate Limiting

The default rate limit of an action is Infinity (i.e no rate limits), but if you want to rate limit an action you can specify the frequency limit. For example, an action can be set to execute at a maximum of 10 times per day.

There are 3 different types of rate limits that can be applied, they are:

* **Rolling Window** - A moving window type rate limit essentially means that at any given point in time, the activity can only be done a specified number of times in the last interval. Let us say your rate limit is 5 times per day, this would translate to a limit of 5 times in the last 24 hours (more precisely, in the last 24x60x60x1000 milliseconds) at any given point in time. Since we need to store timestamps for each event, there is a limit of 50 events in an interval. This is the default algorithm.

* **Fixed Window** - In a fixed window rate limit, the windows are fixed at particular time intervals - minutes, hours, days, weeks, months and years. What this means is that if your rate limit is 5 per day, the activity can be performed 5 times in a calendar day. In this alogirthm, the game's timezone is used to figure out calendar limits. The interval unit has been restricted only to 1 (i.e. 5 times in 1 day, in 1 minute, etc) as some other values can lead to unexpected results.

* **Leaky Bucket** - The leaky bucket algorithm, though generally used in the world of networking, might also find use in games. You can read more about it [here](http://en.wikipedia.org/wiki/Leaky_bucket).


