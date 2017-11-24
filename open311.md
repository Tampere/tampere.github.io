## Open311

An API for reporting issues and giving feedback

API location: [http://feedback.tampere.fi](http://feedback.tampere.fi)

The issue reporting API is used by applications for sending service requests to the City of Tampere.

At the moment, the API is mostly used for sending service requests about broken parts of city infrastructure 
like street signs, potholes, etc. These kinds of service requests are typically handled 
by the Public Works Department. However, the API is increasingly used for other types of feedback as well.

**The API enables its clients to:**

- Submit new service requests
- Query individual or multiple service requests and their descriptions and statuses
- List service request types and definitions of additional attributes

The Issue Reporting API is based on GeoReport API version 2, better known as the Open311 specification. 
The interface is designed in such a way that any GeoReport v2 compatible client is able to use the interface. 
This interface is compliant with some CitySDK specific enhancements to Open311 and parameters specific 
to it are marked CitySDK specific.

### Tampere specific details

#### API keys and testing

Client applications submitting new service requests to the feedback system will need an API key. 
None of the other methods require an API key.

The City of Tampere Ope311 has a shared API key for testing developer applications 
before connecting to the production system. The API key developers use in the test environment 
is different than the one used in production. The standard developer workflow goes like this:

- Using the test API key, the developer implements and tests their client against the system.
- The developer signs up for an API key for the production system. After email verification, the production API key is created and given to the developer.
- The developer's application is deployed using the production system API key and its requests are persisted in the production system.

### API Methods

#### GET Service List

Provides a list of acceptable service request types and their associated service codes.

* URL address http://feedback.tampere.fi/v1/services.json
* Required API key: No
* Method: GET

**Response parameters:**

| Parameter name | Description |
|----------------|-------------|
|service_code|The unique identifier for the service request type|
|service_name|The human readable name of the service request type|
|description|A brief description of the service request type|
|keywords|A comma separated list of tags or keywords to help users identify the request type.|
|group|A category to group this service type within. This provides a way to group several service request types under one category.|

**Example response:**

    [ 
        {
            "service_code": "246",
            "service_name": "Roskaaminen",
            "description": "Ilmoita, jos havaitset suuria määriä roskia yleisillä alueilla, puistoissa tai metsissä.",
            "metadata": false,
            "type": "realtime",
            "keywords": "roska,jäte",
            "group": "Puhtaanapito"
        },
        {
            "service_code": "176",
            "service_name": "Töhryjen poisto",
            "description": "Ilmoita, jos paikkoja tai seiniä on töhritty.",
            "metadata": false,
            "type": "realtime",
            "keywords": "roska,jäte",
            "group": "Puhtaanapito"
        }
    ]

#### POST Service Request

This method is used for sending service requests to the city's feedback system.

* URL: http://feedback.tampere.fi/v1/requests.json
* Posting formats: Content-Type application/x-www-form-urlencoded or multipart/form-data
* Requires API key: Yes
* Method: POST

**Request parameters:**

|Parameter name|Description|Required|Notes|
|--------------|-----------|--------|-----|
|api_key|Api key submitting service requests|Yes||
|service_code|The unique identifier for the service request type|Yes||
|description|A full description of the service request|Yes|This is free form text having min 10 and max 5,000 characters.|
|title|Title of the service request|No||
|lat|Latitude using the WGS84 projection|No|Required with long|
|long|Longitude using the WGS84 projection|No|Required with lat|
|address_string|Human readable address or description of the location|No||
|email|The email address of the person submitting the request|No|If email address is given, service will notify the email address when service request is processes|
|first_name|The given name of the person submitting the request|No||
|last_name|The family name of the person submitting the request|No||
|phone|The phone number of the person submitting the request|No||
|media_url|A URL to media associated with the request, e.g. an image|No|API supports the most typical media formats like jpg, png, gif,.. Note, that URL links must end with a correct filetype like ".jpg", e.g. http://pbs.twimg.com/media/Ay9wUStCEAEtoPC.jpg.|
|media|Array of file uploads|No|A client may POST multiple files as multipart/form-data. This is the equivalent of having multiple &lt;input type="file" name="media[]" /&gt; inputs.|

**Response parameters:**

|Parameter name|Description|
|--------------|-----------|
|service_request_id|The unique ID of the service request created|
|service_notice|Currently empty string|

**Possible errors:**

* Invalid or missing service_code
* Invalid or missing api_key
* Invalid or missing description
* File uploads over 40MB

**Example response:**

    [
        {
            "service_request_id":"8fmht6g1470b3qk8pthg",
            "service_notice":""
        }
    ]

#### GET Service Requests

This method is used to query the current status of multiple requests.

* URL: http://feedback.tampere.fi/v1/requests.json
* Requires API key: No
* Method: GET

**Request parameters:**

_All parameters are optional_

|Parameter name|Description|Notes|
|--------------|-----------|-----|
|service_request_id|To call multiple Service Requests at once, multiple service_request_id can be declared; comma delimited.|This overrides all other arguments|
|service_code|Specify the service type by calling the unique ID of the service_code.|This defaults to all service codes when not declared; can be declared multiple times, comma delimited|
|start_date|Earliest requested_datetime to include in the search|Must use W3C format, e.g 2010-01-01T00:00:00Z|
|end_date|Latest requested_datetime to include in the search|Must use W3C format, e.g 2010-01-01T00:00:00Z|
|status|Allows one to search for requests which have a specific status. This defaults to all statuses||
|lat|Latitude|Location based search if lat, long and radius are given|
|long|Longitude|Location based search if lat, long and radius are given|
|radius|Radius (meters) in witch location based search is performed|Location based search if lat, long and radius are given|
|updated_after|Earliest updated_datetime to include in search. Allows one to search for requests which have an updated_datetime between the updated_after time and updated_before time (or now). This is useful for downloading a changeset that includes changes to older requests or to just query very recent changes|Must use w3 format, eg 2010-01-01T00:00:00Z|
|updated_before|Latest updated_datetime to include in search. Allows one to search for requests which have an updated_datetime between the updated_after time and the updated_before time. This is useful for downloading a changeset that includes changes to older requests or to just query very recent changes|When not specified (updated_after is used without updated_before) then updated_before is assumed to be now. Must use w3 format, eg 2010-01-01T00:00:00Z|

**Response parameters:**

|Parameter name|Description|Required|
|--------------|-----------|--------|
|service_request_id|The unique ID of the service request created|Yes|
|status_notes|Explanation of why the status was changed to the current state or more details on the current status than is conveyed with status alone|Yes|
|status|The current status of the service request. Open or closed|Yes|
|service_code|The unique identifier for the service request type|Yes|
|service_name|The human readable name of the service request type|No|
|description|A full description of the request or report submitted|Yes|
|requested_datetime|The date and time when the service request was made|Yes|
|updated_datetime|The date and time when the service request was last modified. For requests with status=closed, this will be the date the request was closed|Yes|
|address|Human readable address or description of location|No|
|lat|latitude using the (WGS84) projection|No|
|long|longitude using the (WGS84) projection|No|
|media_url|A URL to media associated with the request, eg an image|No|
|media_urls|Contains URLs to media associated with the request|No|

**Example response:**

    [
        {
            "service_request_id": "1ju4p6v3e04pcpobfv7u",
            "service_code": "178",
            "description": "Tilapäiset merkit voisi ihan hyvin sijoittaa Lautatarhankatu ja Hämeentie kauppakeskus Arabian kohdalla.",
            "service_notice": "",
            "requested_datetime": "2013-04-30T10:52:55+03:00",
            "updated_datetime": "2013-05-01T14:35:01+03:00",
            "status": "closed",
            "status_notes": "Kiitos ilmoituksestanne. Olen välittänyt asian alueen tarkastajalle, joka valvoo tilapäisiä liikennejärjestelyjä ja kaivauksia.",
            "agency_responsible": "",
            "service_name": "",
            "address": "",
            "address_id": "",
            "zip_code": "",
            "lat": "60.189587726063884",
            "long": "24.966438613848045"
        }
    ]

#### GET Service Request

This method is used to query the current status of a service request.

* URL: http://feedback.tampere.fi/v1/requests/[service_request_id].json
* Requires API key: No
* Method: GET

Response parameter are the same as with the previous example.

**Possible errors:**

* The service request requested is invalid or has been removed

### Moderation of Service Requests

When a service request is posted to the city's issue reporting system, it is not yet shown publicly in the API's service request listings. Each service request is pre-moderated and only after publishing is the service request visible through the API's listing views.

Before the service request is moderated, the service request details are visible and can be queried by using the service_request_id.

### Uploading media

Uploading of media files like images is supported in two ways. POSTing a Service Request can reference an image with the media_url parameter or a media file can be uploaded directly as multipart/form-data. More than one image is supported. If applications send more than one image, special care should be taken on the application side because of the long upload time. It is also possible to upload using the media_url reference and directly multipart/form-data in one shot.

### Notes

This documentation is an adaptation of the [City of Helsinki Open311 documentation](https://dev.hel.fi/apis/open311/).
