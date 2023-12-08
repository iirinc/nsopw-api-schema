# NSOPW JSON Schema v1.0
This folder holds the query and response schemas to be used with NSOPW. This page also has some documentation on status codes but look at the samples folder for detailed information on doing queries.

- [NSOPW JSON Schema v1.0](#nsopw-json-schema-v10)
  - [Response Validation Messages](#response-validation-messages)
    - [Primary Status Code](#primary-status-code)
      - [HTTP 200 - OK](#http-200---ok)
      - [HTTP 422 - Validation Error](#http-422---validation-error)
      - [HTTP 429 - Too many requests](#http-429---too-many-requests)
      - [HTTP 500 - Internal Server Error](#http-500---internal-server-error)
      - [HTTP 503 - Service Unavailable](#http-503---service-unavailable)

## Response Validation Messages
These are the messages that should be returned by your API based on different statuses. 

The NSOPW back-end when querying the API would not break the search standards but if it did, these would be the status messages to be returned so the system would understand the error.

### Primary Status Code
Based in the schema, these would be returned via the HTTP status and within the JSON in the `statusCode` field for the whole result.

#### HTTP 200 - OK
On a succesful query, return a [HTTP 200](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200) and one of the following values in the `statusCode`.

- **200** - Sucessful search.
- **201** - Sucessful search but the jurisdictions passed in the query do not match the jurisdiction that responded. 

#### HTTP 422 - Validation Error
These are `statusCode` values to return if there is a validation issue. The API should return a [HTTP 422](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422). 

- **100** - *Invalid Query* - There was not any fields filled out for the search. The search must be a name, zip, or geographic search in order to work. For a name search, first and last name is required. For a zip search, at least one zip code is required. For a geographic search, latitude, longitude, and distance are required. All these search options require at least one jurisdiction to be valid as well.
- **101** - *Invalid Query* - There was an unhandled and unexpected exception while doing the query.
- **102** - *Mixed Search* - You cannot submit a first and last name with a latitude, longitude, and distance. These must be individual searches.
- **103** - *Missing First or Last Name* - For a name search, you must submit a first and last name to work.
- **104** - *Geographic - Invalid Lat/Lon* - This search has an invalid latitude or longitude. Latitudes must be between -90 and 90 while longitudes must be between -180 and 180.
- **105** - *Geographic - Distance Missing* - For a geographic search, a distance must be provided.
- **106** - *Geographic - Distance Invalid* - For a geographic search, distance must be a whole number with the value of 0.25. 0.5, 1, 2, or 3.
- **107** - *Geographic - Jurisdiction Not Searchable* - One or more jurisdictions that was provided to do a geographic search does not support geographic searches and cannot be completed.
- **108** - *Jurisdiction(s) Missing* - There were no jurisdictions were supplied for the search.
- **109** - *Invalid Characters* - Invalid characters in the search were found.
- **110** - *Jurisdiction(s) Response Missing *- The response from the back-end did not have any jurisdictions returned. There was either a failure with the search or the jurisdictions provided to the query were invalid.
- **111** - *Too many characters in a search field* - There are too many characters in either first name, last name, city, or county. The maximum number of characters per field allowed is 50.
- **112** - *Zip code must be 5 characters* - There are either too many or too few characters in one or more of the zip codes. The zip code must be 5 characters in length.
- **113** - *Too many zip codes searched* - There are too many zip codes searched. The maximum number of zip codes allowed is 5.
- **114** - *Invalid Zip Code* - One or more of the zip codes searched is invalid. Zip codes must only contain numbers.
- **115** - *Invalid Jurisdiction* - One or more of the jurisdictions identifiers provided does not match what is expected.

#### HTTP 429 - Too many requests
If the API has a limit to the number of searches made and the request limit has exceded this number, return a [HTTP 429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429). No response would be expected in the body.

#### HTTP 500 - Internal Server Error
If the jurisdiction's API has an unhandled exception, return a [HTTP 500](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500). No response would be expected in the body.

#### HTTP 503 - Service Unavailable
If the API is unavilable, return a [HTTP 503](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503). No response would be expected in the body.