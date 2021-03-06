---
title: Leaderboards Service
---

## List All Leaderboards

```
GET /leaderboards
```

Get a list of all available leaderboards.

#### Response

```json
[
  {
    "id": "game_leaderboard",
    "name": "GameLeaderboard",
    "entity_type": "players",
    "metric": {
      "id": "xp",
      "type": "point",
      "name": "experience"
    },
    "scope": {
      "type": "game"
    },
    "cycles": [
      "weekly",
      "daily"
    ]
  },
  {
    "id": "team_definition_leaderboard",
    "name": "TeamDefinitionLeaderboard",
    "entity_type": "players",
    "metric": {
      "id": "xp",
      "type": "point",
      "name": "experience"
    },
    "scope": {
      "id": "international",
      "type": "team_definition"
    },
    "cycles": [
      "weekly",
      "daily"
    ]
  },
  {
    "id": "team_leaderboard",
    "name": "TeamLeaderboard",
    "entity_type": "players",
    "metric": {
      "id": "xp",
      "type": "point",
      "name": "experience"
    },
    "scope": {
      "id": "international",
      "type": "team_instance"
    },
    "cycles": [
      "weekly",
      "daily"
    ]
  }
]
```


## Get a Leaderboard

```
GET /leaderboards/:id
```

Get the leaderboard for the given leaderboard ID.

#### Parameters

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| cycle    | string |         | ✓        | Time range of the given leaderboard. Can be one of daily, weekly, monthly, yearly, alltime. |
| sort     | string | desc    |          | Sorting of the leaderboard. Can be asc or desc. |
| skip     | number | 0       |          | Number of players to skip. |
| limit    | number | 10      |          | Limit the number of players returned in the response. -1 denotes no limit, all players are returned. |
| entity   | string |         |          | Player or Team relative to whom the leaderboard will be. |
| radius   | number | 5       |          | When the entity is provided, players within the requested radius are returned. |
| timestamp | date  | <now>   |          | Get the latest cycle leaderboard earlier than the given timestamp. |
| team_instance_id | string | |          | ID of the team instance. Should be passed when the leaderboard is of type Team Instance. |
| merge    | string |         |          | Merges several leaderboards. The ranking is still based on the original leaderboard, however data of other leaderboards is also sent in the response. Leaderboards being merged can be of any cycle but need to be of the same entity type. <br/> Definition IDs of the leaderboards along with the cycle information are to be sent as comma separated query parameters. The leaderbooard ID and cycle need to be separated by a comma, e.g 'overall_leaderboard,alltime'. See manuals for more details.|
| detailed | string |         |          | Sends details of actions that contributed to the player's leaderboard score. Only available to player leaderboards. |
| scope    | string |         |          | Scope of the leaderboard. Available only in custom leaderboards. See manuals for more details. |


#### Response

* View leaderboard in ascending order.

```
GET /leaderboards/game_leaderboard?sort=ascending&cycle=alltime
```

```json
{
  "data": [
    {
      "player": {
        "id": "droid",
        "alias": "Droid!"
      },
      "score": "5",
      "rank": 4
    },
    {
      "player": {
        "id": "clone",
        "alias": "Clonie"
      },
      "score": "12",
      "rank": 3
    },
    {
      "player": {
        "id": "hercules",
        "alias": "hercules"
      },
      "score": "44",
      "rank": 2
    },
    {
      "player": {
        "id": "darky",
        "alias": "darky"
      },
      "score": "87",
      "rank": 1
    }
  ],
  "total": 4
}
```

* View the leaderboard with a particular entity id and radius.

```
GET /leaderboards/game_leaderboard?player_id=droid&ranking=relative&radius=1&entity_id=clone&cycle=alltime
```

```json
{
      "data": [
        {
          "player": {
            "id": "droid",
            "alias": "Droid!"
          },
          "score": "5",
          "rank": 4
        },
        {
          "player": {
            "id": "clone",
            "alias": "Clonie"
          },
          "score": "12",
          "rank": 3
        },
        {
          "player": {
            "id": "hercules",
            "alias": "hercules"
          },
          "score": "44",
          "rank": 2
        }
      ],
      "total": 4
}
```

* Merge leaderboards

```
GET /leaderboards/level_leaderboard?player_id=droid&cycle=alltime&merge_with=kills_leaderboard|alltime,deaths_leaderboard|alltime
```

```json
{
  "data": [
    {
      "player": {
        "id": "ripley",
        "alias": "The Amazing Mr.Ripley!"
      },
      "score": [
        "level_leaderboard",
        "49",
        "kills_leaderboard",
        "10000",
        "deaths_leaderboard",
        "2000"
      ],
      "rank": 1
    },
    {
      "player": {
        "id": "droid",
        "alias": "Droid!"
      },
      "score": [
        "level_leaderboard",
        "37",
        "kill_leaderboard",
        "1700",
        "deaths_leaderboard",
        "180"
      ],
      "rank": 2
    }
  ],
  "total": 2
}
```


## Get a Player or Team Rank Trend

```
GET /leaderboard/:id/trend
```

Get a player or team rank trend. The trend can be viewed at the following cycles:

* Daily
* Weekly
* Monthly
* Yearly

Trends can be viewed only for those cycles for which the specified leaderboard has been designed. So if a leaderboard has monthly and weekly cycles, you will only be able to view only monthly and weekly trends.

When the leaderboard is of type teams, the team_instance_id is required to specify the team.

The response is an an array of objects. The data key of those objects contains rank information. A rank of -1 is returned when the player was not on the leaderboard at the given time period.

|   Name   | Type   | Default | Required |   Description   |
|----------|--------|---------|----------|-----------------|
| cycle    | string |         | ✓        | Leaderboard cycle. Possible values : daily, weekly, monthly, yearly. |
| entities | string |         | ✓        | Comma separated string containing IDs of players whose rank trends are required. |
| start    | date   |         | ✓        | Valid date string, format - YYYY-mm-dd (eg. 2013-12-13). Specifies the start date for the trend. |
| end      | date   |         | ✓        | Valid date string, format - YYYY-mm-dd (eg. 2013-12-13). Specifies the end date for the trend. |
| team     | string |         | ✓        | ID of the team instance, required when the leaderboard is of type Team Instance. |

#### Response

```
GET leaderboard/game_leaderboard/trend?cycle=daily&start=2013-05-01&end=2013-05-07&entities=peter,johny
```

```json
[
  {
    "data": [
      {
        "date": "2013-05-01",
        "score": "9",
        "rank": 2
      },
      {
        "date": "2013-05-02",
        "score": "9",
        "rank": 5
      },
      {
        "date": "2013-05-03",
        "score": "11",
        "rank": 6
      },
      {
        "date": "2013-05-04",
        "score": "11",
        "rank": 6
      },
      {
        "date": "2013-05-05",
        "rank": -1
      },
      {
        "date": "2013-05-06",
        "rank": -1
      },
      {
        "date": "2013-05-07",
        "rank": -1
      }
    ],
    "player": {
      "id": "peter",
      "alias": "Peter!!"
    }
  },
  {
    "data": [
      {
        "date": "2013-05-01",
        "rank": -1
      },
      {
        "date": "2013-05-02",
        "rank": -1
      },
      {
        "date": "2013-05-03",
        "score": "111",
        "rank": 1
      },
      {
        "date": "2013-05-04",
        "score": "122",
        "rank": 1
      },
      {
        "date": "2013-05-05",
        "rank": -1
      },
      {
        "date": "2013-05-06",
        "rank": -1
      },
      {
        "date": "2013-05-07",
        "rank": -1
      }
    ],
    "player": {
      "id": "johny",
      "alias": "JohnyJ"
    }
  }
]
```
