
JSON-Urania
========

|Author| Steven Christe|
|-------| --------------|
|Last Updated | May 17, 2014 |
| Version | 0.1 (draft)

This documents describes a standard for the interchange of astronomy data. It describes a 
standard Web API as well as the data output (in JSON).

=======

Web API - Requests
=======



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

Data Response
=================

Time Series Data
----------------

All data can be returned in a [JSON](http://json.org) (Javascript Object Notation) format along with a [JSON Schema](http://json-schema.org). A JSON Schema provides a contract for the JSON data required by a given application, and how that data can be modified. XML responses are also supported.

Here is the format for a response.

```
{
    cols:[{id: 'requested-resource1', label: 'string', type: 'string']},
          {id: 'requested-resource2', label: 'string', type: 'string']},
          <...>
          ],
    rows: [
        "<requested-resource1>":[
            {"dateTime":<record-date>,"value":<value>},
            {"dateTime":<record-date>,"value":<value>},
            <...>
            ],
        "<requested-resource1>":[
            {"dateTime":<record-date>,"value":<value>},
            {"dateTime":<record-date>,"value":<value>},
            <...>
            ],
        <...>
        ]
}
```

* `cols` Property
    * An array of objects describing the ID and type of each column. Each property is an object with the following properties (case-sensitive):
        * `type` - The data type of the data in the column. Supports the following string variables.
            * `boolean` - JavaScript boolean value ('true' or 'false').
            * `number` - JavaScript number value.
        * `id` - String ID of the column. Must be unique in the table. Only basic alphanumeric characters, so the host page does not require fancy escapes to access the column in JavaScript.
        * `label` - String value to label the y axis of the data.
        * `units` - String value to provide the units of the data.
* `rows` Property
    * Holds an array of data objects for each entry in the `cols` property. Each data objects is name after the id value in the `cols` property. Each data object contains the following properties.
        * `dateTime` - String value which encodes the date and time of the measurement. These strings are formatted as "%Y/%M/%D %H:%M:%S" (for example "2013-03-05 23:24.24.105").
        * `value` - The value for the given time given in the format given by `type`.
        
Metadata
--------

The response to a metadata request is as simple as possible. Here is the format for a response.

```
{
    "<requested-resource>":[
            {"value"},
            {"value"},
            <...>
            ]
}
```
This response type is used to reply to requests of type `telescope`, `measurement`, `dateStart`, `dateEnd`. The request for dates will provide the dateTime for the earliest and latest data available respectively.

Here are a few examples

request 

```
&telescope="?"
```

response

```
{
    "telescope":[
            {"telescope-name-1"},
            {"telescope-name-2"},
            {"telescope-name-3"},
            ]
}
```

request

```
?telescope="telescope-name-1"&measurement="?"
```
response

```
{
    "measurement":[
            {"measurement-name-1"},
            {"measurement-name-2"},
            {"measurement-name-3"},
            ]
}
```

request

```
?telescope="telescope-name-1"&measurement="measurement-name"&dateStart="?"
```
response

```
{
    "dateStart":[
            {"dateTime"},
            ]
}
```
