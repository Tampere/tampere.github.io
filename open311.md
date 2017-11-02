## Open311

An API for reporting issues and giving feedback

API location: `update here`

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
