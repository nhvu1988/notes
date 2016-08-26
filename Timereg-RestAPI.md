# TIMEREG SYSTEM API v2

## What's news from v1?

* Support REST-API following [SCIM 2.0](http://www.simplecloud.info/)
* Using object model to define the resource
* Using HTTP Status Code to verify the success/failure of the request

## Authenticate with JWT Token
Timereg API using JWT authenticate, `token` will be included in _query string_

```HTTP
GET /api/v2/user?token={token} HTTP/1.1	
```

or _HTTP request body_

```HTTP
POST /api/v2/user HTTP/1.1

{
	"token": "{token}"
}
```

or _HTTP request header_ `x-access-token`

```HTTP
POST /api/v2/user HTTP/1.1	
x-access-token: {token}
```

> If `token` is not provided or invalid, the request will be responded immediately with HTTP Status Code `401 Unauthorized`

## APIs

### User API
---
> #### GET /api/v2/user

##### Description
Get list of users in system with filters

##### Parameters 
_[not implement yet]_

##### Response
An array of [User](#user-model)

---
> #### GET /api/v2/user/{userId}

##### Description
Get information of a user

##### Response Header
<table>
	<tr>
		<td>HTTP Status Code</td>
		<td>Reason</td>
	</tr>
	<tr>
		<td>200</td>
		<td>User found and returned in body</td>
	</tr>
	<tr>
		<td>404</td>
		<td>User not found</td>
	</tr>
</table>

##### Response Body
An object of [User](#user-model)

---
> #### GET /api/v2/user/{userId}/timeregs

##### Description
Get all timereg records of a user

##### Parameters
<table>
	<tr>
		<td><b>key</b></td>
		<td><b>type</b></td>
		<td><b>required</b></td>
		<td><b>description</b></td>
	</tr>
	<tr>
		<td>fromDateTime</td>
		<td>datetime</td>
		<td>no</td>
		<td>Get timereg records from this datetime</td>
	</tr>
	<tr>
		<td>toDateTime</td>
		<td>datetime</td>
		<td>no</td>
		<td>Get timereg records until this datetime</td>
	</tr>
</table>

##### Response Body
An array of [Timereg](#timereg-model)


---
> #### GET /api/v2/user/{userId}/billableAccounts

##### Description
Get all billable accounts of a user

##### Response Body
An array of [ProjectPhase](#projectphase-model)

### Timereg API

---
> #### GET /api/v2/timereg

##### Description
Get all timereg records with filters

##### Parameters
<table>
	<tr>
		<td><b>key</b></td>
		<td><b>type</b></td>
		<td><b>required</b></td>
		<td><b>description</b></td>
	</tr>
	<tr>
		<td>userId</td>
		<td>string</td>
		<td>no</td>
		<td>ID of filtered user</td>
	</tr>
	<tr>
		<td>timeregPeriodId</td>
		<td>string</td>
		<td>no</td>
		<td>ID of filtered timereg period</td>
	</tr>
	<tr>
		<td>projectPhaseId</td>
		<td>string</td>
		<td>no</td>
		<td>ID of filtered project phase</td>
	</tr>
	<tr>
		<td>fromDateTime</td>
		<td>datetime</td>
		<td>no</td>
		<td>Get timereg records from this datetime</td>
	</tr>
	<tr>
		<td>toDateTime</td>
		<td>datetime</td>
		<td>no</td>
		<td>Get timereg records until this datetime</td>
	</tr>
</table>

##### Response Body
An array of [Timereg](#timereg-model)

---
> #### POST /api/v2/timereg

##### Description
Create a new timereg record

##### Request Body
An object of [Timereg](#timereg-model)

##### Response Header
<table>
	<tr>
		<td>HTTP Status Code</td>
		<td>Reason</td>
	</tr>
	<tr>
		<td>200</td>
		<td>Timereg record was saved successfully</td>
	</tr>
	<tr>
		<td>400</td>
		<td>Bad Request, check error message in response body for detail</td>
	</tr>
	<tr>
		<td>409</td>
		<td>Conflict, the from/toDateTime overlap with another record </td>
	</tr>
</table>

##### Response Body
An object of created [Timereg](#timereg-model)

---
> #### GET /api/v2/timereg/{timeregId}

##### Description
Get a timereg detail

##### Response Header
<table>
	<tr>
		<td>HTTP Status Code</td>
		<td>Reason</td>
	</tr>
	<tr>
		<td>200</td>
		<td>Timereg record found and returned in body</td>
	</tr>
	<tr>
		<td>404</td>
		<td>Timereg record not found</td>
	</tr>
</table>

##### Response Body
An object of found [Timereg](#timereg-model)

## Models

### Timereg Model

{

* id [String] timereg's ID
* userId [String] 
* timeregPeriodId [String]
* fromDateTime [DateTime]
* toDateTime [DateTime]
* projectPhaseId [String]
* detail [String]

}


### User Model

{

* id [String] user's ID
* firstName [String]
* lastName [String]
* gender [String] is one value of _[male, female]_
* dateOfBirth [DateTime]
* startWorking [DateTime] user's start working date 
* teamsUri [String] string url to get all user's teams 
* email [String] personal email
* globeteamEmail [String]
* rangstrupEmail [String]
* dropboxEmail [String]
* liveEmail [String]
* googleEmail [String]
* skypeName [String]
* phone [String]
* billableAccountsUri [String] url to get all user's billable accounts (project phases)
* timeregsUri [String] url to get all user's timeregs

}

### ProjectPhase Model

{

* id [String] project phase's ID
* name [String] project phase's name
* projectId [String] ID of the project followed by phase  

}

[String]: https://tools.ietf.org/html/rfc7643#section-2.3.1
[Boolean]: https://tools.ietf.org/html/rfc7643#section-2.3.2
[DateTime]: https://tools.ietf.org/html/rfc7643#section-2.3.5

