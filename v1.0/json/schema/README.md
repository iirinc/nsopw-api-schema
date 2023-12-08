# NSOPW JSON Schema v1.0 Overview
This folder holds the query and response schemas to be used with NSOPW. This page also has some documentation on status codes but look at the samples folder for detailed information on doing queries.

- [NSOPW JSON Schema v1.0 Overview](#nsopw-json-schema-v10-overview)
- [Query](#query)
  - [Property Definitions](#property-definitions)
  - [Query Types](#query-types)
    - [Name Search](#name-search)
    - [Zip Code Search](#zip-code-search)
    - [Geolocation Search](#geolocation-search)
    - [All Results](#all-results)
- [Response](#response)
  - [Validation Messages](#validation-messages)
    - [Primary Status Code](#primary-status-code)
      - [HTTP 200 - OK](#http-200---ok)
        - [Status MessagePer Jurisdiction](#status-messageper-jurisdiction)
      - [HTTP 422 - Validation Error](#http-422---validation-error)
      - [HTTP 429 - Too many requests](#http-429---too-many-requests)
      - [HTTP 500 - Internal Server Error](#http-500---internal-server-error)
      - [HTTP 503 - Service Unavailable](#http-503---service-unavailable)

# Query
This is an overview of the query that you would be receiving from the NSOPW system.

## Property Definitions
This is an overview of the properties within a query. These are all defined in the [schema](query.schema.json) file as well.

| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `id` | Search Identifier | String | No | This is a unique id of the search provided. This would be used for diagnostics and logging to track queries on the NSOPW site and potentially the jurisdiction side. | 000000-0000-0000-00000000000 |
| `jurisdictions` | Jurisdictions To Search | Array | Yes | This is an array of jurisdictions to search. This normally would only be one jurisdiction but in the case where a system hosts multiple jurisdictions, it could allow for more being queried at once. | [ "DC" ] |
| `maxResults` | Maximum Results | Integer | Yes |  This is the maximum number of results to return. If more records than this value are found, return a 511 in the status code for the jurisdiction. This number is per jurisdiction searched, not all of them combined. If the value is 0, this would be the optional [all results](#all-results) query. | 300 |
| `firstName` | First Name | String | No, except for [name search](#name-search) | The first name (given name) of the offender to search. | John |
| `lastName` | Last Name | String | No, except for [name search](#name-search) | The last name (sur or family name) of the offender to search. | Smith |
| `city` | City | String | No | The city to search for an offender. | Washington |
| `county` | County | String | No | The county to search for an offender. | Fairfax |
| `zips` | Zip Code(s) | Array | No, except for [zip code search](#zip-code-search) | Array of 0 to 5 zip codes to search. This would only include zip codes that are within your jurisdiction. This is just the first 5 of the zip and excludes the Zip+4. | [ "20001"] |
| `latitude` | Latitude | Number | No, except for [geolocation search](#geolocation-search) | The latitude to search for an offender. Must be between -90 and 90 and not 0. | 38.897957 |
| `longitude` | Longitude | Number | No, except for [geolocation search](#geolocation-search) | The longitude to search for an offender. Must be between -180 and 180 and not 0. | -77.03656 |
| `distance` | Distance | Number | No, except for [geolocation search](#geolocation-search) | The distance in miles to search for an offender. This must be either 0.25, 0.5, 1, 2, or 3 | 3 |


## Query Types
The NSOPW system will only allow specific searches and they would follow these types. All queries are validated by the NSOPW system but this documents how the jurisdiction should handle the queries against their database.

Users can search 3 ways, by [name](#name-search) (including city, county and zip), by [zip code(s)](#zip-code-search), by [geolocation](#geolocation-search).  The system has an optional additional query to pull [all results](#all-results).

### Name Search
A name search query would include the following fields 

- `jurisdictions` - Required for all searches.
- `maxResults` - Required for all searches.
- `firstName` - Required for a name search.
- `lastName` - Required for a name search.
- `city` - Optional but should be an inclusive query.
- `county` - Optional but should be an inclusive query.
- `zips`  - Optional but should be an inclusive query.

Most searches will just be a first and last name but if any of the optional fields are sent, the filter should be inclusive so you are querying on offender records that match based on `firstName`, `lastName`, and the additional fields. 

### Zip Code Search
A zip code search would only be zip code(s) to search and would not include any other filter values. You could expect up to 5 zip codes to be queried and you would only be queried zip codes that are within your jurisdiction. If users search zip codes not in your jurisdiction, you would not recieve that as part of the query.

- `jurisdictions` - Required for all searches.
- `maxResults` - Required for all searches.
- `zips` - Required for a zip code search

### Geolocation Search
A geolocation search would only be a `latitude`, `longitude`, and `distance` to query. The jurisdiction should return all offenders that are within the distance of the latitude and longitude provided. This query would not include any other fields. 

Note that a geolocation search will only include a single jurisdiction which would be your jurisidiction. 

- `jurisdictions` - Required for all searches.
- `maxResults` - Required for all searches.
- `latitude` - Required
- `longitude` - Required
- `distance` - Required

### All Results
There is an optional query that doesn't have to be supported by your jurisdicition but this would allow the NSOPW system to query the API and get all resuls back. This would only be called once a day and the results would be cached. This would then allow for the instance where the jurisdiction is offline, the NSOPW system would use the cached values. This query would not include any other fields and would only include one jurisdiction per query.

- `jurisdictions` - Would only include one jurisdiction.
- `maxResults` - The value would be 0.

# Response
This is an overview of the response that would be expected from the NSOPW system.

## Validation Messages
These are the messages that should be returned by your API based on different statuses. 

The NSOPW back-end when querying the API would not break the search standards but if it did, these would be the status messages to be returned so the system would understand the error.

### Primary Status Code
Based in the schema, these would be returned via the HTTP status and within the JSON in the `statusCode` field for the whole result.

#### HTTP 200 - OK
On a succesful query, return a [HTTP 200](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200) and one of the following values in the `statusCode`.

- **200** - Sucessful search.
- **201** - Sucessful search but the jurisdictions passed in the query do not match the jurisdiction that responded. 

##### Status MessagePer Jurisdiction
In addition to the base `statusCode`, within the `jurisdictionStatus` array, per juridiction query you would need to return an additional `statusCode`. This is a list of what these values should be.

- **200** - Successful Search
- **201** - Successful SoundEx Search
- **500** - There was an error searching the jurisdiction.
- **504** - The jurisdiction timed out during the query.
- **507** - The jurisdiction is offline.
- **511** - Too many matching offenders from this jurisdiction.
- **560** - The jurisdiction does not support a geographic search.

#### HTTP 422 - Validation Error
These are `statusCode` values to return if there is a validation issue. The API should return a [HTTP 422](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422) and should return a response in the body. 

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
- **110** - *Jurisdiction(s) Response Missing* - The response from the back-end did not have any jurisdictions returned. There was either a failure with the search or the jurisdictions provided to the query were invalid.
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