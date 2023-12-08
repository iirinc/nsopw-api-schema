# Query
This is an overview of the query that you would be receiving from the NSOPW system.

- [Query](#query)
  - [Property Definitions](#property-definitions)
  - [Query Types](#query-types)
    - [Name Search](#name-search)
    - [Zip Code Search](#zip-code-search)
    - [Geolocation Search](#geolocation-search)
    - [All Results](#all-results)


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

Review a sample [query](samples/valid/name-simple.query.json) and [response](samples/valid/name-simple.response.json).

### Zip Code Search
A zip code search would only be zip code(s) to search and would not include any other filter values. You could expect up to 5 zip codes to be queried and you would only be queried zip codes that are within your jurisdiction. If users search zip codes not in your jurisdiction, you would not recieve that as part of the query.

- `jurisdictions` - Required for all searches.
- `maxResults` - Required for all searches.
- `zips` - Required for a zip code search

Review a sample [query](samples/valid/zip.query.json) and [response](samples/valid/zip.response.json).

### Geolocation Search
A geolocation search would only be a `latitude`, `longitude`, and `distance` to query. The jurisdiction should return all offenders that are within the distance of the latitude and longitude provided. This query would not include any other fields. 

Note that a geolocation search will only include a single jurisdiction which would be your jurisidiction. 

- `jurisdictions` - Required for all searches.
- `maxResults` - Required for all searches.
- `latitude` - Required
- `longitude` - Required
- `distance` - Required

Review a sample [query](samples/valid/geolocation-simple.query.json) and [response](samples/valid/geolocation-simple.response.json).

### All Results
There is an optional query that doesn't have to be supported by your jurisdicition but this would allow the NSOPW system to query the API and get all resuls back. This would only be called once a day and the results would be cached. This would then allow for the instance where the jurisdiction is offline, the NSOPW system would use the cached values. This query would not include any other fields and would only include one jurisdiction per query.

- `jurisdictions` - Would only include one jurisdiction.
- `maxResults` - The value would be 0.