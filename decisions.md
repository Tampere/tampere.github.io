## Open Decisions API

Open Decisions API is providing decisions made by a city in a harmonized and machine-readable format.

[Open decision API definition created by the six largest cities in Finland](https://github.com/6aika/api-paatos)

[Finnish municipal decision-making Django app](https://github.com/6aika/paatos)

[Web user interface for paatos-api](https://github.com/6aika/paatos-ui)

### API Methods

Open Decisions API is a read-only open API. Contents are imported from CaseM system periodically.

API root: [http://decisions.tampere.fi/v1/](http://decisions.tampere.fi/v1/)

#### API Endpoints

All endpoints allow `GET`, `HEAD` and `OPTIONS` methods. Data is returned in json -format with `Content-Type: application/json`.

Data is paginated with a default result set size of `20`. Different result sets can be requested with `limit` and `offset` -parameters, where limit is the number of results and offset tells how many results to omit from the start.

**Example response:**

    {
        "count": 2363,
        "next": "http://decisions.tampere.fi/v1/action/?limit=20&offset=20",
        "previous": null,
        "results": [
            {
                "url": "http://decisions.tampere.fi/v1/action/2325/",
                "id": 2325,
                "data_source": "tampere_casem",
                "contents": [
                    {
                        "data_source": "tampere_casem",
                        "created_at": "2017-11-17T08:46:44.923614Z",
                        "modified_at": "2017-11-17T08:46:44.923644Z",
                        "origin_id": "8-36125",
                        "ordering": 8,
                        "title": "Lisätietoja päätöksestä",
                        "type": "",
                        "hypertext": "Päätösvalmistelusihteeri Karoliina Vehmaanperä, puh. 040 142 4654, etunimi.sukunimi@tampere.fi"
                    }
            }   ]
        ]
    }

##### GET organizations

* URL: http://decisions.tampere.fi/v1/organization/
* Method: GET

Decision making bodies, such as `"Yhdyskuntalautakunnan ympäristö- ja rakennusjaosto"`. Organizations include an array of recent events.

**Example response:**

    [
        {
            "url": "http://decisions.tampere.fi/v1/organization/40/",
            "id": 40,
            "data_source": "tampere_casem",
            "events": [
                "http://decisions.tampere.fi/v1/event/299/",
                "http://decisions.tampere.fi/v1/event/300/"
            ],
            "posts": [],
            "created_at": "2017-11-17T08:45:49.183420Z",
            "modified_at": "2017-11-17T08:45:49.183486Z",
            "origin_id": "11403",
            "classification": "Jaosto",
            "name": "Yhdyskuntalautakunnan jätehuoltojaosto",
            "founding_date": null,
            "dissolution_date": null,
            "parent": null
        }
    ]

|Parameter name|Description|
|--------------|-----------|
|url|URL of this organization|
|id|Unique identifier of this organization|
|events|Array of event links|
|classification|Type of organization|
|name|Human readable name of organization|

##### GET events

* URL: http://decisions.tampere.fi/v1/event/
* Method: GET

Decision making event, such as `"Kokous 12.9.2017"`. Events include an array of actions.

**Example response:**

    {
        "url": "http://decisions.tampere.fi/v1/event/303/",
        "id": 303,
        "data_source": "tampere_casem",
        "actions": [
            "http://decisions.tampere.fi/v1/action/1892/",
            "http://decisions.tampere.fi/v1/action/1891/"
        ],
        "created_at": "2017-11-17T08:45:51.200072Z",
        "modified_at": "2017-11-17T08:45:51.200133Z",
        "origin_id": "12040",
        "name": "Kokous 22.3.2017",
        "start_date": "2017-03-22",
        "end_date": "2017-03-22",
        "organization": "http://decisions.tampere.fi/v1/organization/40/"
    }

|Parameter name|Description|
|--------------|-----------|
|url|URL to this event|
|id|Unique identifier of this event|
|actions|Array of actions taken in this event|
|name|Human readable name of event|
|start_date|Start date of the event|
|end_date|End date of the event|
|organization|URL to event's organization|

##### GET actions

* URL: http://decisions.tampere.fi/v1/action/
* Method: GET

Actions are actions taken in a decision making event, such as `"Yleisten alueiden suunnittelussa on valmistunut raportti \"Liikenteen kehitys Tampereella 2016\". Raportti koostuu kahdesta osaraportista: - Ajoneuvoliikenteen liikennemääräraportti 2016 - Liikenneonnettomuusraportti 2016."` (action condensed to one line and omitted HTML tags).

**Example response:**

    {
        "url": "http://decisions.tampere.fi/v1/action/1892/",
        "id": 1892,
        "data_source": "tampere_casem",
        "contents": [
            {
                "data_source": "tampere_casem",
                "created_at": "2017-11-17T08:45:51.697148Z",
                "modified_at": "2017-11-17T08:45:51.697183Z",
                "origin_id": "8-15261",
                "ordering": 8,
                "title": "Lisätietoja päätöksestä",
                "type": "",
                "hypertext": "Päätösvalmistelusihteeri Pauliina Aittala, puh. 040 485 1059, etunimi.sukunimi@tampere.fi "
            }
        ],
        "attachments": [],
        "created_at": "2017-11-17T08:45:51.606388Z",
        "modified_at": "2017-11-17T08:45:51.606444Z",
        "origin_id": "15261",
        "title": "Vireillepano kunnan ympäristönsuojeluviranomaisessa jätehuoltomääräysten noudattamiseksi",
        "ordering": 5,
        "article_number": "9",
        "resolution": "",
        "case": "http://decisions.tampere.fi/v1/case/1202/",
        "post": null,
        "event": "http://decisions.tampere.fi/v1/event/303/"
    }

|Parameter name|Description|
|--------------|-----------|
|url|URL to this action|
|contents|Array of content objects|
|attachments|Possible attachments|
|title|Human readable title of the action|
|case|URL to case the action was part of|
|event|URL to event the action was part of|

##### GET cases

* URL: http://decisions.tampere.fi/v1/case/
* Method: GET

Cases are issues allowing actions to be taken on, such as `"Lintuviidankadun katusuunnitelma, Lamminpää"`. Cases can include actions and attachments.

**Example response:**

    {
        "url": "http://decisions.tampere.fi/v1/case/1202/",
        "id": 1202,
        "data_source": "tampere_casem",
        "actions": [
            "http://decisions.tampere.fi/v1/action/1892/"
        ],
        "geometries": [],
        "created_at": "2017-11-17T08:45:49.229603Z",
        "modified_at": "2017-11-17T08:45:49.229636Z",
        "origin_id": "TRE:1701/11.02.04/2017",
        "title": "Vireillepano kunnan ympäristönsuojeluviranomaisessa jätehuoltomääräysten noudattamiseksi",
        "register_id": "TRE:1701/11.02.04/2017",
        "function": "http://decisions.tampere.fi/v1/function/43/",
        "attachments": []
    }

|Parameter name|Description|
|--------------|-----------|
|url|URL to this case|
|id|Unique identifier to this case|
|actions|Array of actions attached to this case|
|title|Human readable title of case|
|attachments|Array of possible attachments|

##### GET functions

* URL: http://decisions.tampere.fi/v1/function/
* Method: GET

Functions are source system data.

##### GET posts

* URL: http://decisions.tampere.fi/v1/post/
* Method: GET

Currently empy.
