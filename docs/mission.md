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
