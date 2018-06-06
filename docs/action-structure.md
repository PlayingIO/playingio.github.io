---
title: Action Structure
---

## Base Structure

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the action. |
| name        | A name for the action. |
| description | Brief description of the action. |
| image       | An image which represents the action. |
| probability | The probability that the player gets the rewards on completing the action. |
| rate        | Rate Limiting an action so that users do not abuse the system.<br/>This is an array of three items, which are:</br><ul><li>Count - The number of times the player can perform this action within the window.</li><li>Timeframe - Counted in milliseconds, this is the window or timeframe within which the player can perform this action Count number of times.</li><li>Type - The type of rate limiting being used, which can be one of **ROLLING**, **FIXED**, **LEAKY**.</li></ul> |

