Web API - Requests
=======

|Author| Steven Christe|
|-------| --------------|
|Last Updated | May 18, 2014 |
| Version | 0.1 (draft)

This documents describes the standard Urania web api interface. A server is compatible with this description is said to be Urania-compatible.

Base URL
--------
All URLs referenced in this documentation have the following base:

`http://www.mywebsite.com/`

This API is served over HTTPS. To ensure data privacy, unencrypted HTTP is not supported.

Request Time Series Data
-------------------
To request time series data, make an HTTP POST request.

Required Parameters

| Name | Type | Description | Example | Remarks |
|------|------|-------------|---------|
|dateStart | string | The start of the requested observation time | "2012-04-04 01:53" | All times are assumed to be in UT |
|dateEnd | string | The end of the requested observation time | "2012-04-05 01:53" | All times are assumed to be in UT |
| telescope | string | The name of the telescope | GOES-14 | |
| measurement | string | If the telescope has multiple measurement types, specify it with this parameter | x-ray | If the telescope has only one measurement then this parameter is optional |

Request Metadata
----------------
Requests can be made to get additional information about what types of data are provided by the server. The API is modeled after the data request above for time series data. If the `dateStart` and `dateEnd` are not provided the server will assume that metadata is being requested. To request information the data parameter passed must be "?". So for example to a list of the telescopes which are supported one can use

```
&telescope=?
```
This request returns a meta object.


Error Handling
--------------

Describe the response of the server to a bad request