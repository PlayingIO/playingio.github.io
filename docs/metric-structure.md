---
title: Metric Structure
---

## Base Structure

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the metric. |
| name        | A name for the metric. |
| description | Brief description of the metric.
| image       | An image which represents the metric. |
| type        | Type of the metric: one of point, set, state and compound. |
| constraints | The constraints on a metric differ depending on what type of metric it is. Refer to the linked sections for each metric type:<br/><ul><li>Point Metric Constraints</li><li>Set Metric Constraints</li><li>State Metric Constraints</li><li>Compound Metric Constraints</li></ul> |
| tags        | Array of strings which can be used when querying to get all designs based on certain tags specified |

### Point Metric Constraints

|    Field    |    Description    |
|-------------|-------------------|
| min         | The minimum value the point metric can have. |
| max         | The maximum value the point metric can have. |
| default      | The value which will be used in calculations if this metric is not already set on the player. |

