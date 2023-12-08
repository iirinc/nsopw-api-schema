# Valid Query Overview
These are examples of valid queries and there responses. 

Note in the samples, the field `$schema`, this does not need to be included and is only there for us to validate the examples.

## Name Search
Per the rules of a [name search](/json/v1.0/QueryDetails.md#name-search), these are examples of valid queries and results.

- Simple search of one jurisdiction and a name
  - [Query](name-simple.query.json)
  - [Response](name-simple.response.json)
- Search of two jurisdictions and a name
  - [Query](name-multiple-jurisdictions.query.json)
  - [Response](name-multiple-jurisdictions.response.json)

## Geolocation Search
Per the rules of a [geolocation search](/json/v1.0/QueryDetails.md#geolocation-search), these are examples of valid queries and results.

- Search of just one jurisdiction
  - [Query](geolocation-simple.query.json)
  - [Response](geolocation-simple.response.json)

## Zip Code Search
Per the rules of a [zip code search](/json/v1.0/QueryDetails.md#zip-code-search), these are examples of valid queries and results.

- Search of multiple zip codes
  - [Query](zip.query.json)
  - [Response](zip.response.json)