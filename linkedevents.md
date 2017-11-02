## Lined Events

Aggregates data about events around the city

API location: [http://linkedevents.tampere.fi/v1/](http://linkedevents.tampere.fi/v1/)

API endpoints:

[http://linkedevents.tampere.fi/v1/event](http://linkedevents.tampere.fi/v1/event)

[http://linkedevents.tampere.fi/v1/keyword](http://linkedevents.tampere.fi/v1/keyword)

[http://linkedevents.tampere.fi/v1/place](http://linkedevents.tampere.fi/v1/place)

[http://linkedevents.tampere.fi/v1/search](http://linkedevents.tampere.fi/v1/search)

Linked Events provides categorized data on events and places for Tampere region. The API contains all event data from the Visit Tampere API. In addition, Linked Events will be containing all suitable event data from Menovinkki.

The location information is not linked to any locational database and exists only as a part of the event information.

In the API, you can search data by query parameter filters or freetext.

The API provides data in JSON-LD format.

### Example usage (more examples on API page)

#### Get All Events

This endpoint retrieves all events as paginated object array.

**HTTP Request**

GET `http://linkedevents.tampere.fi/v1/event`

**Query Parameters**

| Parameter |	Default |	Description |
| --------- |:-------:| ------------|
| page      |	1	      | Chuck of paginated results |
| start     |	null    |	Event start at. yyyy-mm-dd or today |
| end       |	null    |	Event end at. yyyy-mm-dd or today |
| keyword   |	null    |	Event keyword(s). To restrict the retrieved events by category, use the query parameter keyword, separating values by commas if you wish to query for several keywords. Keyword ids are found at the keyword endpoint. |
| location	| null    |	Event location(s). To restrict the retrieved events by location, use the query parameter location, separating values by commas if you wish to query for several locations. Location ids are found at the location endpoint. |
| data_source |	null  |	Data source id. Currently only visittampere. |
| recurring |	null    |	Search for events based on whether they are part of recurring event set. 'super' specifies recurring, while 'sub' is non-recurring. |
| min_duration | null |	Search for events that are longer than given time in seconds. |
| max_duration | null |	Search for events that are shorter than given time in seconds. |
| sort      |	start_time |	Sort the returned events in the given order. Possible sorting criteria are `start_time`, `end_time` and `last_modified_time`. |
| last_modified_since |	null |	Event has been modified at or since. yyyy-mm-dd or today |
