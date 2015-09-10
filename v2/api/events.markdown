---
layout: page
title:  "Events"
tags: server
---

### POST `/rest/v1/api/tenants/{tenantId}/devices/{deviceId}/events` ###

Use this endpoint to report to the server 

|	Parameter	| Type 			| Required	|	Description		|
|:--------------|:--------------|:--------------|:--------------|
| `typeId`	 |	String		|	Yes  	|	The identifier of the event type.	This identifier can be defined by the implementation and should be a machine readable name without spaces. Limited to 24 characters.|
| `description` |	String		|	Yes  	| A human readable description of this particular event type. Limited to 255 characters. |
| `appId` 	 |	String		|	Yes  	| The id associated to the tenant app making the event call. |
| `severity` 	 |	Integer		|	Yes  	| This value should be an integer in the range of 1 to 5.	|
| `data`	 |	Object		|	Yes  	| An object containing a set of data related to the given event.	|
| `timestamp`	 |	Integer		|	Yes  	|	An epoch timestamp in milliseconds representing the point in time when this event transpired. |

Note: The `data` object should be traditional json including data types. The data should not be coerced into a string if it is an int, etc.

#### Example ####

	{
	    typeId: 0045,
	    description: ,
	    appKey: "IPSO-SFHWREG-ASDEGFE-SDGACXV-BSDF",
	    severity: 3,
	    data: {
	            source:"Software",
	            version:2.5,
	            active:true
	    },
	    timestamp: 1440014969567
	}

#### Responses ####

**201 Created**

Event was successfully created in the system.

**202 Accepted**

Event was saved and will be processed later.

**400 Bad Request**

Event data was malformed. This could mean there are incorrect fields, missing fields, etc. See error handling below to identify what the problem is.
	
**403 Forbidden**

The header api key is a valid api key but does not have access to this particular tenant or device. A valid user, but one not authorized to save this event.
	
**404 Not Found**

The tenant or device was not found or does not exist. See error handling below to identify which value is invalid.

**405 Method Not Allowed**

Should be thrown for any method other than a POST (i.e. GET, DELETE, PUT, PATCH)

**409 Conflict**

This event has already been created (This could be determined if there is an event for the same tenant, device, type, and timestamp. The other data should not be different either.)

**Error Handling**

Failure status codes have a json response with an error and errorMessage.

	{
	    error: <int>,
	    message: <string>
	}

Error Codes

|	Error Code	| Description		|
|:--------------|:--------------|
| -1 |	Server Unknown server error	|
| 0 |	Invalid license	|
| 4 |	Invalid tenant id	|
| 5 |	Invalid device id	|
| 6 |	Validation error	|