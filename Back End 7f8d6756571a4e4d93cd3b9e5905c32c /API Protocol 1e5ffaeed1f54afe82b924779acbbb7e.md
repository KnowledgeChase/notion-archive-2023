# API Protocol

The table below describes the endpoints for the Live API server including the information required in each request and returned in each response.

<aside>
💡 **Table of Contents**

</aside>

# Notes

- The [response codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) follow the [FRC 9110](https://httpwg.org/specs/rfc9110.html#overview.of.status.codes) standard ([list](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)).
- DateTime strings are transmitted as ISO 8601 strings which can be created using [Date.toISOString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString).   Dates are in UTC time which is 4 hours ahead of EST. For example, 9:00 am on May 12, 2023 can be written with EST time and an offset (**2023-05-12T09:00:00.000-04:00)** or directly in UTC time with ‘Z’ at the end (**2023-05-12T13:00:00.000Z)** or as you can see in the MongoDB database in UTC time with a 0 offset (**2023-05-12T13:00:00.000+00:00)**. See [Helpful guide](https://dd.engineering/blog/a-guide-to-handling-date-and-time-for-full-stack-javascript-developers#storing-dates-in-the-database) on using dates.
- All routes that require a Bearer token will return status code *401: Unauthorized* if the token is missing or invalid.

# Users

## Create User

| Method | POST |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| Endpoint | /user |  |  |  |  |
| Request requires Bearer token in Auth Header | false |  |  |  |  |
| Request body | property | type | required | default value | constraints |
|  | email | String | yes |  | valid email |
|  | password | String | yes |  | min length: 8 |
|  | phone | String | yes |  | valid phone |
|  | firstName | String | yes |  | non empty |
|  | lastName | String | yes |  | non empty |
| Response code / data | 201 Created | No data is returned in response | email is sent to user |  |  |
|  | 400 Bad Request |  |  |  |  |
|  | 500 Internal Server Error |  |  |  |  |

## Verify Email

| Method | POST |  |  |
| --- | --- | --- | --- |
| Endpoint | /user/verification |  |  |
| Request requires Bearer token in Auth Header | false |  |  |
| Request query string | parameter | value | constraint |
|  | token | String | String contains a valid email verification token |
| Response code / data | 200 OK |  |  |
|  | 400 Bad Request |  |  |
|  | 500 Internal Server Error |  |  |

## Resend Verification Email

| Method | GET |  |  |
| --- | --- | --- | --- |
| Endpoint | /user/verification |  |  |
| Request requires Bearer token in Auth Header | false |  |  |
| Request query string | parameter | required |  |
|  | email | yes |  |
|  | password | yes |  |
| Response code / data | 200 OK | email is sent to user |  |
|  | 400 Bad Request |  |  |
|  | 500 Internal Server Error |  |  |

## Log in

| Method | POST |  |  |
| --- | --- | --- | --- |
| Endpoint | /user/login |  |  |
| Request requires Bearer token in Auth Header | false |  |  |
| Request body | property | type | required |
|  | email | String | yes |
|  | password | String | yes |
| Response code / data | 200 OK | { “user”: User, “token”: String } |  |
|  | 401 Unauthorized | “Email has not been verified.” |  |
|  | 500 Internal Server Error | Cannot find user |  |

## Log out

| Method | POST |
| --- | --- |
| Endpoint | /user/logout |
| Request requires Bearer token in Auth Header | true |
| Response code / data | 200 OK |
|  | 500 Internal Server Error |

## Get User Profile

| Method | GET |  |
| --- | --- | --- |
| Endpoint | /user/profile |  |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK | User |

## Update User Profile

| Method | PATCH |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /user/profile |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | email | String | no | valid email |
|  | password | String | no | min length: 8 |
|  | phone | String | no | valid phone |
|  | firstName | String | no | non empty |
|  | lastName | String | no | non empty |
| Response code / data | 200 OK | User |  |  |
|  | 400 Bad Request | “One or more invalid properties.” |  |  |
|  | 500 Internal Server Error |  |  |  |

## Send Reset Password Email

| Method | GET |  |  |
| --- | --- | --- | --- |
| Endpoint | /user/passwordResetEmail |  |  |
| Request requires Bearer token in Auth Header | false |  |  |
| Request query string | parameter | value | constraint |
|  | email | String |  |
| Response code / data | 200 OK |  |  |
|  | 500 Internal Server Error |  |  |

## Reset Password

| Method | PATCH |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /user/password |  |  |  |
| Request requires Bearer token in Auth Header | false |  |  |  |
| Request query string | parameter | value | constraint |  |
|  | token | String | valid String in User.resetPasswordToken |  |
| Request body | property | type | required | constraints |
|  | password | String | yes | length >8 |
| Response code / data | 200 OK | User |  |  |
|  | 401 Unauthorized | “No token” |  |  |
|  | 400 Bad request | "Request must contain password.” |  |  |
|  | 500 Internal Server Error |  |  |  |

## Add/Delete Favorite Contests

| Method | PATCH |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /user/favoriteContests |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | add | Object [] | no | Each object is as follows: { “name”: String, “favId”: Contest.id } |
|  | remove | Id [] | no | Ids are _ids for existing favorites. |
| Response code / data | 200 OK | User |  |  |
|  | 500 Internal Server Error |  |  |  |

## Add/Delete Favorite Missions

| Method | PATCH |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /user/favoriteMissions |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | add | Object [] | no | Each object is as follows:{ “name”: String, “favId”: Mission.id } |
|  | remove | Id [] | no | Ids are _ids for existing favorites. |
| Response code / data | 200 OK | User |  |  |
|  | 500 Internal Server Error |  |  |  |

## Get Notifications

| Method | GET |  |
| --- | --- | --- |
| Endpoint | /user/notifications |  |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK | notifications [] |

## Delete Single Notifications

| Method | DELETE |  |
| --- | --- | --- |
| Endpoint | /user/notifications/{id} | {id} is a valid notification id |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK | notifications [] |
|  | 500 Internal Server Error |  |

## Delete All Notifications

| Method | DELETE |  |
| --- | --- | --- |
| Endpoint | /user/notifications/all |  |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK | notifications [] |
|  | 500 Internal Server Error |  |

## Toggle Notification Read Status

| Method | PATCH |  |
| --- | --- | --- |
| Endpoint | /user/notifications/{id} | {id} is a valid notification id |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK | notifications [] |
|  | 400 Bad request | "Invalid notification id.” |
|  | 500 Internal Server Error |  |

# Contests

## Create Contest

| Method | POST |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| Endpoint | /contest |  |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |  |
| Request body | property | type | required | default value | constraints |
|  | name | String | yes |  |  |
|  | description | String | no |  |  |
|  | prize | String | no |  |  |
|  | startDateTime | String | yes |  | https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString |
|  | endDateTime | String | yes |  | https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString |
|  | private | Boolean | no | false |  |
|  | participationApprovalRequired | Boolean | no | false |  |
|  | hostJudgesAllMissions | Boolean | no | true |  |
| Response code / data | 201 Created | contest object |  |  |  |
|  | 401 Unauthorized |  | "User has reached limit on number of hosted contests.” |  |  |
|  | 500 Internal Server Error |  |  |  |  |

## Delete Contest

| Method | DELETE | FRONTEND DONE |
| --- | --- | --- |
| Endpoint | /contest/{id} | {id} is a valid contest Id |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK |  |
|  | 400 Bad Request | “Invalid contestId.” |
|  | 401 Unauthorized |  |
|  | 500 Internal Server Error |  |

## Update Contest

| Method | PATCH |  |  | FRONTEND DONE |
| --- | --- | --- | --- | --- |
| Endpoint | /contest/{id} | {id} is a valid contestId |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | name | String | no |  |
|  | description | String | no |  |
|  | prize | String | no |  |
|  | startDateTime | String | no | https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString |
|  | endDateTime | String | no | https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString |
|  | private | Boolean | no |  |
|  | participationApprovalRequired | Boolean | no |  |
|  | hostJudgesAllMissions | Boolean | no |  |
| Response code / data | 200 OK | Contest |  |  |
|  | 400 Bad request | “Invalid contestId.” |  |  |
|  | 400 Bad request | “One or more invalid properties.” |  |  |
|  | 401 Unauthorized |  |  |  |
|  | 500 Internal Server Error |  |  |  |

## Get Single Contest/Mission (public, hosting, cohosting, invited, participating, or judging)

| Method | GET |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{id} | {id} is a valid contestId or missionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request query string | parameter | required | default value |
|  | idType=contest | mission | no | contest |
| Response code / data | 200 OK | Contest Object |  |
|  | 400 Bad request | “Invalid idType.” |  |
|  |  | "Invalid missionId.” |  |
|  |  | "Contest not found.” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Error |  |  |

## ***Get/Search Contests

| Method | GET |  |  |
| --- | --- | --- | --- |
| Endpoint | /contests |  |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request query string | parameter |  |  |
|  | ongoing=true|false |  |  |
|  | limit=Number |  |  |
|  | skip=Number |  |  |
|  | sortBy=field-name:asc|desc | field-name can also include numberOfContestants |  |
|  | search=String |  |  |
|  | private=true|false |  |  |
|  | role=host | constestant | judge | invitedToCohost | invitedToJudge | invitedToparticipate | favorites |  |  |
| Response code / data | 200 OK | { “contests”: [], total: Number } |  |
|  | 400 Bad request | "Private must be true or false.” |  |
|  | 500 Internal Server Error |  |  |

## Transfer Ownership

| Method | PATCH |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{id}/ownership |  |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request body | property | type | required |
|  | id | UserId (of Cohost) | yes |
|  | message | String | no |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | "Id required in body of request.” |  |
|  |  | "Invalid contestId.” |  |
|  |  | "Id is not a cohost.” |  |
|  |  | "Invalid id.” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Error |  |  |

# Cohosts

## Invite Cohost

| Method | POST |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| Endpoint | /contest/{id}/cohosts/invitations |  |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |  |
| Request body | property | type | required | default value | constraints |
|  | emailAddresses | [String] | yes |  | Each string is a valid email address |
|  | message | String | no |  |  |
| Response code / data | 200 OK | {
invalid: [],
alreadyInvited: [],
alreadyCohost: [],
newlyInvited: [],
accountNotFound: []
} |  |  |  |
|  | 400 Bad request | “Request must contain emailAddresses.” |  |  |  |
|  |  | 'EmailAddresses must be an array. |  |  |  |
|  |  | ‘Emailaddresses must be a nonempty array.’ |  |  |  |
|  |  | 'Contest does not exist.’ |  |  |  |
|  | 401 Unauthorized |  |  |  |  |
|  | 500 Internal Server Error |  |  |  |  |

## Delete Cohost Invitation

| Method | DELETE |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /contest/{id}/cohosts/invitations |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | emailAddresses | [String] | yes | Each string is a valid email address |
|  | message | String | no |  |
| Response code / data | 200 OK | {
invalid: [],
notInvited: [],
newlyRemoved: [],
accountNotFound: []
} |  |  |
|  | 400 Bad request | “Request must contain emailAddresses.” |  |  |
|  |  | 'EmailAddresses must be an array. |  |  |
|  |  | ‘Emailaddresses must be a nonempty array.’ |  |  |
|  |  | 'Contest does not exist.’ |  |  |
|  | 401 Unauthorized |  |  |  |
|  | 500 Internal Server Error |  |  |  |

## Accept/Reject Cohost Invitation

| Method | PATCH |  |
| --- | --- | --- |
| Endpoint | /contest/{id}/cohosts/invitations |  |
| Request requires Bearer token in Auth Header | true |  |
| Request query string | parameter | value |
|  | action | accept | reject |
| Response code / data | 200 OK |  |
|  | 400 Bad request | "Invalid action.” |
|  |  | 'Contest does not exist.’ |
|  |  | "Already a cohost.” |
|  | 401 Unauthorized |  |
|  | 500 Internal Server Error |  |

## Delete Cohost

| Method | DELETE |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /contest/{id}/cohosts |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | userIds | [userId] | yes | Each userId is a valid user id |
|  | message | String | no |  |
| Response code / data | 200 OK | {
notCohost: [],
removed: []
} |  |  |
|  | 400 Bad request | "Request must contain userIds.” |  |  |
|  |  | 'UserIds must be an array.’ |  |  |
|  |  | ‘UserIds must be a nonempty array.” |  |  |
|  |  | 'Contest does not exist.’ |  |  |
|  | 401 Unauthorized |  |  |  |
|  | 500 Internal Server Error |  |  |  |

## Cohost Removes Self as Cohost

| Method | PATCH |  |
| --- | --- | --- |
| Endpoint | /contest/{id}/cohosts |  |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK |  |
|  | 400 Bad request | 'Contest does not exist.’ |
|  | 401 Unauthorized |  |
|  | 500 Internal Server Error |  |

# Contestants

## Join (public - no approval needed) or Leave (any) Contest

| Method | PATCH |  |
| --- | --- | --- |
| Endpoint | /contest/{id}/contestants | {id} is a valid contestId |
| Request requires Bearer token in Auth Header | true |  |
| Request query string | parameter |  |
|  | action = join | leave |  |
| Response code / data | 200 OK |  |
|  | 400 Bad request | "Contest does not exist.” |
|  |  | "Invalid action.” |
|  |  | "Already participant.” |
|  |  | "Not an active contestant.” |
|  | 401 Unauthorized |  |
|  | 500 Internal Server Error |  |

## Create Invitations

| Method | POST |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /contest/{id}/contestants/invitations | {id} is a valid contestId |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | emailAddresses | String [] | yes | Each String is a valid email address |
|  | message | String | no |  |
| Response code / data | 200 OK | Results Object |  |  |
|  | 400 Bad request | "Request must contain emailAddresses.” |  |  |
|  |  | 'EmailAddresses must be an array.’ |  |  |
|  |  | "EmailAddresses must be a nonempty array.” |  |  |
|  |  | “Contest does not exist.” |  |  |
|  | 401 Unauthorized |  |  |  |
|  | 500 Internal Server Eror |  |  |  |

## Remove Invitations

| Method | DELETE |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /contest/{id}/contestants/invitations | {id} is a valid contestId |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | emailAddresses | String [] | yes | Each String is a valid email address |
|  | message | String | no |  |
| Response code / data | 200 OK | Results Object |  |  |
|  | 400 Bad request | "Request must contain emailAddresses.” |  |  |
|  |  | 'EmailAddresses must be an array.’ |  |  |
|  |  | "EmailAddresses must be a nonempty array.” |  |  |
|  |  | “Contest does not exist.” |  |  |
|  | 401 Unauthorized |  |  |  |
|  | 500 Internal Server Eror |  |  |  |

## Accept/Reject Invitation

| Method | PATCH |  |
| --- | --- | --- |
| Endpoint | /contest/{id}/contestants/invitations | {id} is a valid contestId |
| Request requires Bearer token in Auth Header | true |  |
| Request query string | parameter |  |
|  | action = accept | reject |  |
| Response code / data | 200 OK |  |
|  | 400 Bad request | "Invalid action.” |
|  |  | “Contest does not exist.” |
|  |  | "Already a contestant.” |
|  | 401 Not Authorized |  |
|  | 500 Internal Server Error |  |

## Request to Participate

| Method | POST |  |
| --- | --- | --- |
| Endpoint | /contest/{id}/contestants/participation-requests | {id} is a valid contestId |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK |  |
|  | 400 Bad request | “'Contest does not exist.” |
|  |  | “Contest is private” |
|  |  | “Contest does not require requests to join.” |
|  |  | "User is already in request list.” |
|  |  | "User already participant.” |
|  | 500 Internal Server Error |  |

## Evaluate Requests (host)

| Method | PATCH |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{id}/contestants/participation-requests | {id} is a valid contestId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request body | property | type | required |
|  | acceptedUsers | Array[UserId] | no |
|  | rejectedUsers | Array[UserId] | no |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | “Contest does not exist.” |  |
|  |  | “AcceptedUsers must be an array.” |  |
|  |  | “RejectedUsers must be an array.” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Error |  |  |

## Remove & Ban Contestant

| Method | DELETE |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /contest/{id}/contestants | {id} is a valid contestId |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request body | property | type | required | constraints |
|  | userIds | UserId [] | yes | Each UserId is a valid user id |
|  | message | String | no |  |
| Response code / data | 200 OK | Results Object |  |  |
|  | 400 Bad request | “Request must contain userIds.” |  |  |
|  | 400 Bad request | “UserIds must be an array.” |  |  |
|  | 400 Bad request | "UserIds must be a nonempty array.” |  |  |
|  | 400 Bad request | “Contest does not exist.” |  |  |
|  | 401 Unauthorized |  |  |  |
|  | 500 Internal Server Eror |  |  |  |

## ***Remove Contestant Ban

| Method | PATCH |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{id}/bannedUsers | {id} is a valid contestId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request body | property | type | required |
|  | userIds | Array[UserId] | yes |
|  | message | String | no |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | "Request must contain userIds.” |  |
|  |  | 'UserIds must be an array.’ |  |
|  |  | "UserIds must be a nonempty array.” |  |
|  |  | 'Contest does not exist.’ |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Error |  |  |

# Missions

## ***Create Mission

| Method | POST |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| Endpoint | /contest/{id}/mission | {id} is a valid contestId |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |  |
| Request query string | parameter | value | required | default value |  |
|  | setUserAsJudge | true | false | no | true |  |
| Request body | property | type | required | default value | constraints |
|  | name | String | yes |  | valid email |
|  | description | String | yes |  | min length: 8 |
|  | pointValue | Number | no | 1 | valid phone |
|  | maxCompletionsPerUser | Number | no | 1 | non empty |
|  | startDateTime | String | yes |  | https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString |
|  | endDateTime | String | yes |  | https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString |
|  | validationMethod | String | yes |  | in [”TextUpload”, “SingleFileUpload”, “UserListUpload”] |
| Response code / data | 201 Created | Mission object |  |  |  |
|  | 401 Unauthorized |  |  |  |  |
|  | 400 Bad request | “Contest does not exist.” |  |  |  |
|  |  | "Mission start must be after contest start.” |  |  |  |
|  |  | 'Mission end must be before contest end.’ |  |  |  |
|  | 500 Internal Server Error |  |  |  |  |

## Update Mission

| Method | PATCH |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid} | {cid} is a valid contestId and {mid} is a valid missionId |  |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |  |
| Request body | property | type | required | default value | constraints |
|  | name | String | no |  | valid email |
|  | description | String | no |  | min length: 8 |
|  | pointValue | Number | no |  | valid phone |
|  | maxCompletionsPerUser | Number | no |  | non empty |
|  | startDateTime | String | no |  | https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString |
|  | endDateTime | String | no |  | https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString |
|  | validationMethod | String | no |  | in [”TextUpload”, “SingleFileUpload”, “UserListUpload”] |
| Response code / data | 200 OK | mission Object |  |  |  |
|  | 401 Unauthorized |  |  |  |  |
|  | 400 Bad request | "One or more invalid properties.” |  |  |  |
|  |  | “Contest does not exist.” |  |  |  |
|  |  | "MissionId does not belong to specified contest.” |  |  |  |
|  |  | "Mission does not exist." |  |  |  |
|  |  | "Mission start must be after contest start.” |  |  |  |
|  |  | 'Mission end must be before contest end.’ |  |  |  |
|  | 500 Internal Server Error |  |  |  |  |

## Delete Mission

| Method | DELETE |  |
| --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid} | {cid} is a valid contest Id and {mid} is a valid mission Id |
| Request requires Bearer token in Auth Header | true |  |
| Response code / data | 200 OK |  |
|  | 401 Unauthorized |  |
|  | 400 | "Contest does not exist.” |
|  |  | "Mission does not exist.” |
|  | 500 Internal Server Error |  |

## Get/Search Missions

| Method | GET |  |  |
| --- | --- | --- | --- |
| Endpoint | /missions |  |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request query string | parameter | default |  |
|  | ongoing=true|false |  |  |
|  | limit=Number |  |  |
|  | skip=Number |  |  |
|  | sortBy=field-name:asc|desc |  |  |
|  | search=String |  |  |
|  | private=true|false |  |  |
|  | favorites=true | searches only favoriteMissions |  |
| Response code / data | 200 OK | { “contests”: [], total: Number } |  |
|  | 400 Bad request | "Private must be true or false.” |  |
|  | 500 Internal Server Error |  |  |

# Judges

## Create Invitations to Judge Mission

| Method | POST |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/judges/invitations | {cid} is a valid contestId and {mid} is a valid missionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request body | property | type | required |
|  | emailAddresses | [EmailAddress] | yes |
|  | message | String | no |
| Response code / data | 200 OK | Results |  |
|  | 400 Bad request | “EmailAddresses must be an array.” |  |
|  |  | “Contest does not exist.” |  |
|  |  | "Mission does not exist” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Eror |  |  |

## Remove Invitation to Judge Mission

| Method | DELETE |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/judges/invitations | {cid} is a valid contestId and {mid} is a valid missionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request body | property | type | required |
|  | emailAddresses | Array[EmailAddress] | yes |
|  | message | String | no |
| Response code / data | 200 OK | {
	invalid: [],
	alreadyInvited: [],
	alreadyJudge: [],
	newlyInvited: [],
	accountNotFound: []
} |  |
|  | 400 Bad request | “Contest does not exist.” |  |
|  |  | "Mission does not exist” |  |
|  |  | “EmailAddresses must be an array.” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Eror |  |  |

## Accept/Reject Invitation to Judge

| Method | PATCH |  |
| --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/judges/invitations | {cid} is a valid contestId and {mid} is a valid missionId |
| Request requires Bearer token in Auth Header | true |  |
| Request query string | parameter |  |
|  | action = accept | reject |  |
| Response code / data | 200 OK | Results |
|  | 400 Bad request | “Contest does not exist.” |
|  |  | "Mission does not exist” |
|  |  | "Invalid action.” |
|  |  | “Already judging.” |
|  | 401 Unauthorized |  |
|  | 500 Internal Server Eror |  |

## Delete Judges

| Method | DELETE |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/judges | {cid} is a valid contestId and {mid} is a valid missionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request body | property | type | required |
|  | userIds | UserIds[] | yes |
|  | message | String | no |
| Response code / data | 200 OK | Results |  |
|  | 400 Bad request | “Contest does not exist.” |  |
|  | 400 Bad request | "Mission does not exist” |  |
|  | 400 Bad request | “UserIds must be an array.” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Eror |  |  |

## Remove Self as Judge

| Method | PATCH |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/judges | {cid} is a valid contestId and {mid} is a valid missionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Response code / data | 200 OK | Results |  |
|  | 400 Bad request | “Contest does not exist.” |  |
|  | 400 Bad request | "Mission does not exist” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Eror |  |  |

# Mission Submissions

## Submit Mission for TextUpload

| Method | POST |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/submission | {cid} is a valid contestId and {mid} is a valid missionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request query string | parameter | value |  |
|  | validationMethod | TextUpload |  |
| Request body | property | type | required |
|  | judgeId | UserId | yes |
|  | text | String | yes |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | “Contest does not exist.” |  |
|  |  | "Mission does not exist” |  |
|  |  | “Invalid judgeId.” |  |
|  |  | "Max completions per user exceeded.” |  |
|  |  | "Text not found.” |  |
|  | 401 Unauthorized |  |  |
|  | 401 Unauthorized | "Submission not within the mission window.” |  |
|  | 500 Internal Server Error |  |  |

## Submit Mission for SingleFileUpload

| Method | POST |  |  |  |
| --- | --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/submission | {cid} is a valid contestId and {mid} is a valid missionId |  |  |
| Request requires Bearer token in Auth Header | true |  |  |  |
| Request query string | parameter | value |  |  |
|  | validationMethod | SingleFileUpload |  |  |
| Request body | form-data (not JSON). See http://www.n0code.net/wp/csci430/uploading-and-retrieving-images-in-the-client/#Create_Method_to_Upload_an_Image. | type | required | Constraints |
|  | judgeId | UserId | yes |  |
|  | file | File | yes | valid types:  pdf, txt, jpg, jpeg, png, java, js, html, css, py, c, cpp, h, hpp, rs, zip, tgz, 7z |
|  | text | String  | no |  |
| Response code / data | 200 OK |  |  |  |
|  | 400 Bad request | “Contest does not exist.” |  |  |
|  |  | "Mission does not exist” |  |  |
|  |  | “Invalid judgeId.” |  |  |
|  |  | "Max completions per user exceeded.” |  |  |
|  |  | "File not found.” |  |  |
|  | 415 Unsupported Media Type | “Invalid file type” |  |  |
|  | 401 Unauthorized | "Submission not within the mission window.” |  |  |
|  | 500 Internal Server Eror |  |  |  |

## Get Submission

| Method | GET |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/submission/{sid} | {cid} is a valid contestId, {mid} is a valid missionId, and {sid} is a valid submissionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | “Contest does not exist.” |  |
|  |  | "Mission does not exist” |  |
|  |  | “Submission does not exist.” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Eror |  |  |

## Get Submissions

| Method | GET |  |
| --- | --- | --- |
| Endpoint | /submissions |  |
| Request requires Bearer token in Auth Header | true |  |
| Request query string | parameter | default |
|  | role=host | judge | contestant | contestant |
|  | limit=Number |  |
|  | skip=Number |  |
|  | sortBy=field-name:asc|desc |  |
|  | status=pending | approved | rejected |  |
| Response code / data | 200 OK | { submissions: [], total: Number } |
|  | 500 Internal Server Error |  |

## Modify Submission

| Method | PATCH |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/adjudication/{sid} | {cid} is a valid contestId, {mid} is a valid missionId, and {sid} is a valid submissionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request query string | parameter | value |  |
|  | validationMethod | TextUpload | SingleFileUpload |  |
| Request body 1 | property | type | required |
|  | judgeId | UserId | no |
|  | text | String | no |
| Request body 2 | form-data (not JSON). See http://www.n0code.net/wp/csci430/uploading-and-retrieving-images-in-the-client/#Create_Method_to_Upload_an_Image. | type | required |
|  | judgeId | UserId | no |
|  | file | File | no |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | “Contest does not exist.” |  |
|  |  | "Mission does not exist” |  |
|  |  | “Submission does not exist.” |  |
|  |  | “Cannot modify an approved submission.” |  |
|  |  | "Cannot modify submission. Max completions per user exceeded.” |  |
|  |  | "Submission not within the mission window.” |  |
|  |  | "Request must contain modifiable information.” |  |
|  |  | "Invalid judgeId.” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Eror |  |  |

## Delete Submission

| Method | DELETE |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/adjudication/{sid} | {cid} is a valid contestId, {mid} is a valid missionId, and {sid} is a valid submissionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | “Contest does not exist.” |  |
|  |  | "Mission does not exist” |  |
|  |  | “Submission does not exist.” |  |
|  |  | "Cannot delete an approved submission.” |  |
|  | 401 Unauthorized |  |  |
|  | 500 Internal Server Eror |  |  |

# Adjudications

## Adjudicate Submission

| Method | PATCH |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/adjudication | {cid} is a valid contestId, {mid} is a valid missionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request body | property | type | required |
|  | Case [] | Each case consists of the following:  { sid: submissionId, status: Approved | Rejected , justification: String (optional) } | no |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | “Request must contain an array.” |  |
|  |  | “Contest does not exist.” |  |
|  |  | "Mission does not exist” |  |
|  | 500 Internal Server Eror |  |  |

## UserListUpload Submission (by Judges)

| Method | PATCH |  |  |
| --- | --- | --- | --- |
| Endpoint | /contest/{cid}/mission/{mid}/user-list-upload | {cid} is a valid contestId, {mid} is a valid missionId |  |
| Request requires Bearer token in Auth Header | true |  |  |
| Request body | property | type | required |
|  | Id [] | Each id is a userId | yes |
| Response code / data | 200 OK |  |  |
|  | 400 Bad request | “Request must contain an array.” |  |
|  |  | “Contest does not exist.” |  |
|  |  | "Mission does not exist” |  |
|  | 500 Internal Server Error |  |  |