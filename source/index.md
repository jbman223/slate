---
title: Zipcode Analytics API Reference

language_tabs:
  - json

toc_footers:
  - <a href='/User/Register'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Zipcode Analytics API reference. THis documentation will guide you through all of the requests available in the Zipcode Analytics API, from populating a created profile to accessing the specific calculated rank data for a Zipcode.

# Authentication

Authentication with the API happens with each request. Every request to the API should include your API key in the query string, like so 
    
`apikey=YOUR_API_KEY`

`https://zipcodeanalytics.com/api/profile?apikey=YOUR_API_KEY`

# Profile

## Current API Key Info

This endpoint retrieves the current profile.

### HTTP Request

`GET http://zipcodeanalytics.com/api/profile`

### Query Parameters

Parameter | Required | Description | Parameter Type
--------- | ------- | ----------- | ---------------
apikey | true | The API key attached to the current profile. | Query String


### Result
A successful call will return the profile name and a friendly message.

> An example successful call

```json
{
   "message":"Hello! Your api key is valid.",
   "name":"Jacob's Even Cooler Profile"
}
```

<aside class="warning">
An invalid API key will cause the request to return a 404 error.
</aside>

## Create New Profile

This endpoint allows users to replace all zipcode data in a profile with the uploaded zipcode data.

### HTTP Request

`GET http://zipcodeanalytics.com/api/profile/create`

### Query Parameters

Parameter | Required | Description | Parameter Type
--------- | ------- | ----------- | ---------------
apikey | true | The API key attached to the current profile. | Query String
type | true | The type of data being uploaded in this request. | Query String

###Input Types

Type ID | Description | Example
--- | --- | ---
1 | Binary input data. A zipcode is entered into a list one time for each potential sale, with a binary 1 or 0 representing a successful or unsuccessful sale respectively. | `34109,1;34109,0`

###Post Parameters

In the post body, a list of zipcodes should be included in the proper format type supplied in the URL.

> Sample post body response

```
34109, 1
34109 : 0
34108:0;
34102 1
34012	1
```

### Result
The data is added to your profile asynchronosly, and may not be available immediately after your response completes. Longer data sets may take seconds to complete.

> Successful response message

```json
{
   "message":"Data added to queue."
}
```

> An example error response

```json
{
	"error": "Format not recieved."
}
```

<aside class="warning">
Not including the proper type of data in the URL will cause the API to throw an error.
</aside>

<aside class="warning">
An invalid API key will cause the request to return a 404 error.
</aside>

## Update Profile

This endpoint allows users to append the provided data to a current profile, and mark the profile for recalculation.

### HTTP Request

`GET http://zipcodeanalytics.com/api/profile/update`

### Query Parameters

Parameter | Required | Description | Parameter Type
--------- | ------- | ----------- | ---------------
apikey | true | The API key attached to the current profile. | Query String
type | true | The type of data being uploaded in this request. | Query String

###Input Types

Type ID | Description | Example
--- | --- | ---
1 | Binary input data. A zipcode is entered into a list one time for each potential sale, with a binary 1 or 0 representing a successful or unsuccessful sale respectively. | `34109,1;34109,0`

###Post Parameters

In the post body, a list of zipcodes should be included in the proper format type supplied in the URL.

> Sample post body response

```
34109, 1
34109 : 0
34108:0;
34102 1
34012	1
```

### Result
The data is added to your profile asynchronosly, and may not be available immediately after your response completes. Longer data sets may take seconds to complete.

> Successful response message

```json
{
   "message":"Data added to queue."
}
```

> An example error response

```json
{
	"error": "Format not recieved."
}
```

<aside class="warning">
Not including the proper type of data in the URL will cause the API to throw an error.
</aside>

<aside class="warning">
An invalid API key will cause the request to return a 404 error.
</aside>

## Delete Profile

This endpoint allows users to remove all data attached to a current profile. This action is final.

### HTTP Request

`GET http://zipcodeanalytics.com/api/profile/delete`

### Query Parameters

Parameter | Required | Description | Parameter Type
--------- | ------- | ----------- | ---------------
apikey | true | The API key attached to the current profile. | Query String

### Result

A successful response will report that the data has been cleared from the specified profile.

> A successful Delete response

```json
{
	"message":"Data deleted from profile"
}
```

<aside class="warning">
An invalid API key will cause the request to return a 404 error.
</aside>

# Zipcode
## Current API Key Info

This endpoint retrieves information about the API key.

### HTTP Request

`GET http://zipcodeanalytics.com/api/zipcode`

### Query Parameters

Parameter | Required | Description | Parameter Type
--------- | ------- | ----------- | ---------------
apikey | true | The API key attached to the current profile. | Query String


### Result

The server will return a response with your API key and a message.

> A successful call to the API will return a message and your API key.

```json
{
	"message":"You are successfully accessing the API.",
    "apiKey":"YOUR API KEY"
}
```

<aside class="warning">
An invalid API key will cause the request to return a 404 error.
</aside>

## Specific Zipcode Information

This API endpoint retrieves the current information about the specified zipcode.

### HTTP Request

`GET http://zipcodeanalytics.com/api/zipcode/{zipcode}`

### Query Parameters

Parameter | Required | Description | Parameter Type
--------- | ------- | ----------- | ---------------
apikey | true | The API key attached to the current profile. | Query String
{zipcode} | true | The integer zipcode to pull information for | URL


### Result

A successful response will include the zipcode, the zipcode's rating, and a null warning.

> A successful response

```json
{
	"ZipNum":34109,
    "Rating":1,
    "Warning":null
}
```

If the `warning` parameter is not null, there has been an update to your data that has not yet been processed by the server. Data is proccesed in bulk about every 5 minutes.

> A pending update response

```json
{
	"ZipNum":34109,
    "Rating":1,
    "Warning":"This zipcode has data in the queue and will be updated soon. The current information in this request will change soon."
}
```

If the `Rating` parameter is 0, the data is currently being recalculated by the server and will not be available for about 10 seconds. This only happens after the data is first created or recently updated.

> A currently updating

```json
{
	"ZipNum":34109,
    "Rating":0,
    "Warning":"This zipcode data is being recalculated. Please refresh the page in 5 seconds to view the updated zipcode rating."
}
```

<aside class="warning">
An invalid API key will cause the request to return a 404 error.
</aside>
