---
title: Team Structure
---

## Base Structure

|    Field    |    Description    |
|-------------|-------------------|
| id          | A unique ID for the team. |
| name        | A name for the team. |
| description | Brief description of the team. |
| image       | An Image which represents the team. You need to do a multi-part form post to send the image data. |
| settings    | Settings for the whole team. See below for Settings section for details |
| permissions | An structure which defines the roles present in each team, and the permissions associated with each role. <br />This is an array of permission rows. Each permission row is an array, with two elements:<ul><li>role: The name of the role.</li><li>permissions: A map of all permissions, each having a boolean value.</li></ul>The list of permissions are as follows:<ul><li>lock: player can lock the team, so no member can join or leave the team</li><li>peer: player can invite others to the same role</li><li>assign: player can invite others to lower roles</li><li>leave: player can leave the team.</li></ul>*Note: the player here means anyone having a role with that permission. |
| creator_roles |  An array of roles which will be assigned to the player who creates an instance of this.<br/>Note: Only the roles defined in the permissions key can be used. |
