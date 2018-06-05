---
title: Missions
---

## Missions

At its core a mission is a way of structuring your players interactions with the game. You can create a simple linear sequence of activities for a player to do or complex choice driven quests.

Missions are the most powerful component available to you in a game. They make it possible to create complex and flexible player journeys. The way you use missions depends on the type of gamified system you wish to make.

There are several benefits you can reap from well defined missions within a game:

* Sketching out the player journey as a mission forces us to think through the actual player experience. We can put ourselves in the player's shoes, and that helps us design better experiences.
* Missions enable you to change the player experience and see the impact immediately in the game.
* Breaking down an activity into many short steps enables the player to receive feedback on each step. This increases the probability of a player making it through the entire mission especially if its long.
* Missions make it easy for us to find out which step of a activity causes people to get stuck or give up. We can then use that insight to improve the player experience.
* Finally missions enable you to create external games much faster. Playing I/O takes care of all the complex state transitions and player progress tracking.

### Use Cases

Missions are great when you need to create things like skill trees. In the context of an online education platform, good skill trees encourage players to learn and allow them to build their own skill set as per their interests. The mission of guidance can be very easily tweaked and tested with Playing I/O.

With features such as Lanes and Escalations, missions work well for role based activities too. In an organization, missions are generally well defined and often involve people with different roles, which makes missions in playlyfe a natrual fit.

## Building a Process

A mission consists of a set of nodes connected together with a set of flows. There are 4 types of nodes that you can create in a mission:

#### Task

These are the bread and butter of a mission. A task can be any activity that you wish your players to perform. All the other nodes are just helpers to control the order in which tasks can be performed. A task can have only 1 incoming flow (in-flow) and 1 outgoing flow (out-flow).

| Property | Description |
|----------|-------------|
| ID       | A unique ID for the task within the mission/sub-mission |
| Name     | A name for the task |
| Loop Count | The number of times a player can loop over the task. [default: 0] |
| Lane     | The lane to which this task belongs |

#### Sub-Process

A sub-mission is almost the same as a mission. You can use sub-missiones to model complex missiones which are big and difficult to visualize. You can also use it to break down the player journey into different modules.

| Property | Description |
|----------|-------------|
| ID       | A unique ID for the sub-mission within the mission/sub-mission |
| Name     | A name for the sub-mission |
| Instance Key | A unique key which identifies this sub-mission from all other sub-missiones within the mission. You can use this to access a particular sub-mission within a complex mission from the API. |
| Loop Count | The number of times a player can loop over the sub-mission. [default: 0] |
| Lane     | The lane to which this sub-mission belongs |

#### Parallel Gateway

Diverging parallel gateways allow a player to perform many different task in any order they wish (i.e., in parallel).

> A diverging parallel gateway has more out-flows than in-flows.

Converging parallel gateways allow a player to proceed along the out-going flow only after they complete all in-flows. It provides a way of synchronizing different paths of progress before moving forward.

> A converging parallel gateway has less out-flows than in-flows.

| Property | Description |
|----------|-------------|
| ID       | A unique ID for the gateway within the mission/sub-mission |
| Name     | A name for the gateway |
| Lane     | The lane to which this sub-mission belongs |

#### Exclusive Gateway

Diverging exclusive gateways allow a player to perform exactly one task from among the many available alternatives.

A diverging exclusive gateway has more out-flows than in-flows.

Converging exclusive gateways allow you to merge progress along different paths together.

A converging exclusive gateway has less out-flows than in-flows.

Unlike converging Parallel gateways, converging Exclusive gateways allow players to proceed along it's out-going flows before they complete all in-flows.

| Property | Description |
|----------|-------------|
| ID       | A unique ID for the gateway within the mission/sub-mission |
| Name     | A name for the gateway |
| Lane     | The lane to which this sub-mission belongs |


### Connection Restrictions

Building missiones using nodes and flows is fairly straight forward, however, there are some restrictions:

* Tasks/Sub-Missions can only have 1 out-flow and 1 in-flow
* Gateways can be either converging or diverging.
  * Diverging gateways have atmost 1 in-flows and atleast 1 out-flows
  * Converging gateways have atmost 1 out-flows and atleast 1 in-flows


### Start Nodes

All nodes without any in-flows in a mission are called Start Nodes. When a player starts a mission, the very first tasks available to him are the Start Nodes in a mission.


### Common Flow Patterns

There are a lot of things you can achieve with missiones, from designing approval systems, to parallel quests and more! We illustrate some of the most common flow patterns below:

#### Linear Flow

You can use this flow to model a linear sequence of activities which the player must perform.

#### Approval Flow

You can use this flow to model a simple approval mechanism. In the diagram, only candidates of the admin lane can perform the approve task. When a player finishes the approve task it resolves the scoring actions on the work task.

#### Parallel Split Flow

You can use this flow to model many choices available to a player which the player can perform in any order.

#### Synchronization Flow

You can use this flow to synchronize several paths that a player could take at a common point before moving forward.


### Sub-Missions

Sub-Missions are an effective way to manage arbitrary complexity within your missiones. Sub-Missions are like small missiones within a larger one. There are a few things you need to know to work with sub-missiones.

#### Task IDs in Sub-Missions

Sub-Missions add a new context for your tasks. This means that two different sub-missiones can have tasks with the same IDs within them. This can be useful for repeated tasks across many sub-missiones.


### Lanes

Lanes are a restricting mechanism used to divide the tasks within a mission among different types of players. This allows you to co-ordinate complex activities between many different teams of players.

Candidates of a lane are eligible to perform tasks/sub-missiones/gateways belonging to that lane. Players can be a part of multiple lanes and cannot perform tasks in lanes they are not a part of.

> You can combine lanes with resolution to create an approval mechanism.



## Rate Limiting

Playing I/O allows you to control the rate at which any task or sub-mission within a mission can performed. You can use this to create an effective an anti-gaming mechanic which prevents players from abusing your system. You can also use it to effect scheduled daily, weekly or monthly tasks.

There are 3 different types of rate limits that can be applied, they are:

Rolling Window - A moving window type rate limit essentially means that at any given point in time, the activity can only be done a specified number of times in the last interval. Let us say your rate limit is 5 times per day, this would translate to a limit of 5 times in the last 24 hours (more precisely, in the last 24x60x60x1000 milliseconds) at any given point in time. Since we need to store timestamps for each event, there is a limit of 50 events in an interval. This is the default algorithm.

Fixed Window - In a fixed window rate limit, the windows are fixed at particular time intervals - minutes, hours, days, weeks, months and years. What this means is that if your rate limit is 5 per day, the activity can be performed 5 times in a calendar day. In this alogirthm, the game's timezone is used to figure out calendar limits. The interval unit has been restricted only to 1 (i.e. 5 times in 1 day, in 1 minute, etc) as some other values can lead to unexpected results.

Leaky Bucket - The leaky bucket algorithm, though generally used in the world of networking, might also find use in games. You can read more about it here.



## Trigger Notifications

Triggers notifications enable players to interact with a mission through their feed steam.



## Scoring

A task/sub-mission can have multiple rewards which take place when they are completed. Rewards change the scores of the player who performed the task/sub-mission.

### Advanced Scoring Mechanics
Besides the normal simple scoring mechanism, Playing I/O provides you a number of advanced scoring mechanics which can be used to make complex gamified experiences.

#### Chance

When a player completes a task the scoring actions associated with it are always applied to the player. Using the chance mechanic you can specify the probability of Playing I/O applying a reward.

There are 2 types of chance mechanics currently:

* Activity Reward Chance - This is the probability of Playing I/O trying to apply the rewards of an activity. If the activity has no rewards this does not have any effect. You can do this if you have many rewards on an activity and wish to have Playing I/O always apply them together.

* Individual Reward Chance - This is the probability of Playing I/O applying an individual rewards to a player. You can do this if you want to set a different chance for applying each reward on the completion of an activity.

> You can combine both Activity Reward Chance and Individual Reward Chance together on a single activity.

#### Resolution

You can use scoring resolutions to delay the application of rewards on a player. Instead of applying the rewards on completion of a task you can specify a second task as the resolution task for the reward.

#### Recurrence

Recurrence is a mechanic which only works with looped tasks. Usually, Playing I/O applies the reward on a looped task only once the task has been completed. A looped task is considered to be complete only after all it's loops are complete. Sometimes this is not the desired behavior. You may wish to apply the reward on every loop of the task. This can be done by marking the reward as a recurring reward.



## Scopes

Scopes let you create leaderboards for an arbitrary group of players. These are particularly useful when you want to create groups based on the source of an event.

Consider a class in which students are being offered various optional courses. Each course has quizzes which on completion grant rewards to the students. To maintain a leaderboard for any given course, instead of creating teams for each course, you can simply pass the course's name or any other unique identifier as the scope and a leaderboard for that course will be created on the fly. This leaderboard will only track actions/rules/missiones that have been performed with the given scope.

Scopes are currently available only via API and are not available in the simulator and would need a custom leaderboard in the design of the game. When executing an action, rule or task, you need to pass the scopes an array of objects with a keys id and entity_id. The id is the actual scope ID and the entity_id is the ID of the player who is being scored.



