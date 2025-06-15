# Using the PensionPal Test Server API

  

## Overview

  

The API responds to HTTPS requests arriving at

  

>https://pensionpal2test.azurewebsites.net/api

  

Data in the request body and response body is in JSON format.

  

Standard HTTP return codes are used, with `200 OK` indicating that the server has successfully processed the request.

  

## Authentication

  

To use the API, a user must first authenticate with the API.

  

On successful authentication, the API returns a bearer token that needs to be passed in all future requests in a `HTTP Authorization` header. The Bearer token is valid for 4 hours and the user must repeat authentication to use the API once the Bearer token has expired.

  

Authentication is acheived by issuing a HTTPS POST to

  

> /api/logon

  

with the request body containing the username and password

  

```json

{

"username": "<string>",

"password": "<string>",

}

```

  

All responses are of the following format

  

> :memo: **Note:** This includes several fields that are only used when the user requires an MFA code to be sent to them by email or SMS. The accounts set up for Bristol University students do not need them, so I have not explained their usage.

  

```json

{

"credentials_ok": "<boolean>",

"new_password_required": "<boolean>",

"code_required": "<boolean>",

"authentication_complete": "<boolean>",

"message": "<string>",

"bearer_token": "<string>",

"bearer_token_expiry": "<datetime>",

"device_token": "<string>",

"real_name": "<string>",

}

```

  

If the authentication fails, then the data in the response will have `authentication_complete` set to false and an erorr message will be in the `message` field. For example:

  

```json

{

"authentication_complete": false,

"message": "Invalid credentials"

}

```

  

If the authentication suceeds, then `authentication_complete` will be true, and a `bearer_token` will be returned along with information on when the toekn expires. For example:

  

```json

{

"authentication_complete": true,

"bearer_token": "XXXXXXX",

"bearer_token_expiry": "2025-04-26T13:43:22Z",

}

```

  

### Example logon followed by an authenticated request

The following example , using `curl` to generate the requests, shows a user authenticating and then issuing a GET request to the `/api/currentUser` endpoint:

  

```

curl --insecure

--url https://pensionpal2test.azurewebsites.net/api/logon

--header "Content-Type: application/json"

--data "{\"username\":\"ruixiong\",\"password\":\"<password>\"}"

```

  

returns

  

```json

{

"credentials_ok": true,

"code_required": false,

"authentication_complete": true,

"bearer_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VySWQiOiI1MTgiLCJBdXRoVHlwZSI6IkJlYXJlciIsImV4cCI6MTc0OTA2MDE1NywiaXNzIjoiUGVuc2lvblBhbCBMdGQiLCJhdWQiOiJQZW5zaW9uUGFsMiBUZXN0IFNpdGUifQ.6Nu9UO1MyvVw5d1D_PzbeBRd7UNswVL8cVrZB7eYpVM",

"bearer_token_expiry": "2025-06-04T18:02:37Z",

"device_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VySWQiOiI1MTgiLCJleHAiOjE3NDk5MTMzNTgsImlzcyI6IlBlbnNpb25QYWwgTHRkIiwiYXVkIjoiUGVuc2lvblBhbDIgVGVzdCBTaXRlIn0.zWSYcYf8B1vFoCkq3R3iNzouLnaUQyKAmxpyWA0CEy8",

"real_name": "Rui Xiong",

"mfa_required": false

}

```

  

```

curl --header "Content-Type: application/json"

--header "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VySWQiOiI1MTgiLCJBdXRoVHlwZSI6IkJlYXJlciIsImV4cCI6MTc0OTA2MDE1NywiaXNzIjoiUGVuc2lvblBhbCBMdGQiLCJhdWQiOiJQZW5zaW9uUGFsMiBUZXN0IFNpdGUifQ.6Nu9UO1MyvVw5d1D_PzbeBRd7UNswVL8cVrZB7eYpVM"

--url pensionpal2test.azurewebsites.net/api/currentUser

  

```

  

returns

  

```json

{

"id": 518,

"userName": "ruixiong",

"schemes": [

{

"id": 47,

"name": "The ABC Pension Scheme",

"securityOptions": {

"serverContactDays": 90,

"documentDeleteDays": 120

},

"permissions": {

"canViewFullConflictsRegister": true,

"canViewFullGiftAndHospitalityRegister": true

}

}

],

"securityOptions": { "inactivityLockMinutes": 60 }

}

```

  

## Listing the pension schemes that a user is allowed to access

  

> /api/currentUser

  

This API returns information about the authenticated user and the set of pension schemes that they are allowed to access. The `id` for a scheme will be needed in subsequent API requests.

  

```

curl --header "Content-Type: application/json"

--header "Authorization: Bearer XXX"

--url pensionpal2test.azurewebsites.net/api/currentUser

```

  

returns

  

```json

{

"id": <integer>,

"userName": <string>,

"schemes": [

{

"id": <integer>,

"name": <string>

},

{

"id": <integer>,

"name": <string>

},

...

],

}

```

  

## Meeting Information

  

### List of meetings

  

> /api/scheme/`{schemeId}`/meetings

  

This API returns an array of meeting information. One entry in the array for each meeting that the authenticated user can access in the indicated scheme. The `schemeId` comes from the `/api/currentUser` results.

  

```

curl --header "Content-Type: application/json"

--header "Authorization: Bearer XXX"

--url https://pensionpal2test.azurewebsites.net/api/scheme/47/meetings

```

  

returns

  

```json

[

{

"id": 713,

"name": "Trustee Meeting",

"date": "2025-04-21T00:00:00",

"startTime": "2025-06-04T10:00:00",

"location": "ABC House"

},

{

"id": 714,

"name": "Trustee Meeting",

"date": "2024-11-18T00:00:00",

"startTime": "2025-06-04T10:00:00",

"location": "ABC House"

}

]

```

  

### Meeting details

  

> /api/scheme/`{schemeId}`/meetings/`{meetingId}`

  

This API returns details of one meeting.

- `schemeId` comes from the `/api/currentUser` results.

- `meetingId` comes from the `/api/scheme/{schemeId}/meetings` results.

  

The data returned is quite complex. Its basic structure is:

  

```json

{

"id": <integer>,

"name": <string>,

"date": <date>,

"startTime": <time>,

"location": <string>,

"attendees": [ ...an array containing information about each attendee.. ],

"agenda": [ ...an array containing each of the agenda items... ]

"attachment": [ ...an array containing information about anything attached to the meeting... ]

]

}

```

  

Each entry in the array of `attendees`, has the follow structure:

  

```json

{

"id": <integer>,

"name": "<string>,

"attending": <boolean> (optional - if omitted the user has not yet indicated their attendance)

},

  

```

  

Each entry in the `agenda` array has the following structure:

  

```

{

"id": <integer>,

"number": <string>,

"title": <string>,

"calculatedStartTime": <time>,

"lengthMinutes": <integer>,

"owner": <string>,

"action": <string>,

"htmlText": "<html markup>",

"attachment": [ ...an array containing information about each item attached to this agenda item ... ]

},

```

  

For example:

  

```

curl --header "Content-Type: application/json"

--header "Authorization: Bearer XXX"

--url https://pensionpal2test.azurewebsites.net/api/scheme/47/meetings/713

```

  

returns

  

```json

{

"id": 713,

"name": "Trustee Meeting",

"date": "2025-04-21T00:00:00",

"startTime": "2025-06-04T10:00:00",

"location": "ABC House",

"attendees": [

{

"id": 6133,

"name": "Emma Barker",

"attending": true,

"userCanEdit": true

},

{

"id": 6136,

"name": "Fred Brown",

"attending": true,

"userCanEdit": true

},

{

"id": 6134,

"name": "James Smith",

"attending": true,

"userCanEdit": true

},

{

"id": 6137,

"name": "Oliver Wright",

"attending": false,

"comment": "Not able to attend",

"userCanEdit": true

},

{

"id": 6135,

"name": "Rose Patel",

"attending": true,

"userCanEdit": true

},

{

"id": 6132,

"name": "Rui Xiong",

"attending": true,

"userCanEdit": true

}

],

"agenda": [

{

"id": 9835,

"number": "1",

"title": "Welcome and apologies",

"indent": 0,

"calculatedStartTime": "2025-06-04T10:00:00",

"lengthMinutes": 2,

"owner": "James Smith",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9836,

"number": "2",

"title": "Conflicts of Interest",

"indent": 0,

"calculatedStartTime": "2025-06-04T10:02:00",

"lengthMinutes": 3,

"owner": "James Smith",

"action": "Declaration",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

{

"file": {

"id": 17812,

"name": "PensionPal Document Template.docx",

"lengthInBytes": 11447

}

}

]

},

{

"id": 9837,

"number": "3",

"title": "Minutes of previous meeting",

"indent": 0,

"calculatedStartTime": "2025-06-04T10:05:00",

"lengthMinutes": 10,

"owner": "James Smith",

"action": "Approval",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9838,

"number": "4",

"title": "Matters arising",

"indent": 0,

"calculatedStartTime": "2025-06-04T10:15:00",

"lengthMinutes": 20,

"owner": "James Smith",

"action": "Discussion",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9839,

"number": "4.1",

"title": "Mr Smith",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9840,

"number": "5",

"title": "Administration and Accounts",

"indent": 0,

"calculatedStartTime": "2025-06-04T10:35:00",

"lengthMinutes": 30,

"owner": "Emma Barker",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9841,

"number": "5.1",

"title": "Regular Administration Report",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

{

"file": {

"id": 17813,

"name": "PensionPal Document Template.pdf",

"lengthInBytes": 368777

}

}

]

},

{

"id": 9842,

"number": "5.2",

"title": "Trustee Report and Accounts",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9843,

"number": "6",

"title": "Investment",

"indent": 0,

"calculatedStartTime": "2025-06-04T11:05:00",

"lengthMinutes": 45,

"owner": "Emma Barker",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9844,

"number": "6.1",

"title": "Investment Strategy",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9845,

"number": "6.2",

"title": "Investment Monitoring Report",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9846,

"number": "6.3",

"title": "Statement of Investment Principles",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9847,

"number": "6.4",

"title": "Implementation Report",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9848,

"number": "7",

"title": "Scheme Funding",

"indent": 0,

"calculatedStartTime": "2025-06-04T11:50:00",

"lengthMinutes": 45,

"owner": "Emma Barker",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9849,

"number": "7.1",

"title": "Actuarial Valuation (31 December 2024)",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9850,

"number": "8",

"title": "Governance",

"indent": 0,

"calculatedStartTime": "2025-06-04T12:35:00",

"lengthMinutes": 30,

"owner": "Rose Patel",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9851,

"number": "8.1",

"title": "Risk Register",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9852,

"number": "8.2",

"title": "Data Protection Policy",

"indent": 1,

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9853,

"number": "9",

"title": "Cyber Security Policy",

"indent": 0,

"calculatedStartTime": "2025-06-04T13:05:00",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

},

{

"id": 9854,

"number": "10",

"title": "Any Other Business",

"indent": 0,

"owner": "James Smith",

"action_colour": "0x39c0ed",

"htmlText": "",

"attachment": [

]

}

],

"attachment": [

]

}

```


> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMDg0OTMwNzldfQ==
-->