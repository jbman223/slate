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
    
    apikey=*YOUR API KEY*

# Profile

## Test Current Profile

This endpoint retrieves the current profile.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
apikey | true | The API key attached to the current profile.

### Result

```json
{
   "message":"Hello! Your api key is valid.",
   "name":"Jacob's Even Cooler Profile"
}```

<aside class="error">
An invalid API key will cause the request to return a 404 error.
</aside>
