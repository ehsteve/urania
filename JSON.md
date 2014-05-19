
JSON-Urania
========

|Author| Steven Christe|
|-------| --------------|
|Last Updated | May 17, 2014 |
| Version | 0.1 (draft)

This documents describes a standard for the interchange of astronomy data. It describes a 
standard Web API as well as the data output (in JSON).

=======

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
