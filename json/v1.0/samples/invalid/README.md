# Invalid Searches
These are examples of invalid searches and the expected response. 

- [Invalid Searches](#invalid-searches)
- [Invalid Query](#invalid-query)
  - [Poorly Formatted Query - Status Code 100](#poorly-formatted-query---status-code-100)
  - [Mixed Search - Status Code 102](#mixed-search---status-code-102)
  - [Missing First or Last Name - Status Code 103](#missing-first-or-last-name---status-code-103)
  - [Invalid Characters - Status Code 109](#invalid-characters---status-code-109)
  - [Too Many Characters in a Search Field - Status Code 111](#too-many-characters-in-a-search-field---status-code-111)
- [Invalid Geographic Queries](#invalid-geographic-queries)
  - [Invalid Latitude or Longitude - Status Code 104](#invalid-latitude-or-longitude---status-code-104)
  - [Distance Missing - Status Code 105](#distance-missing---status-code-105)
  - [Distance Invalid - Status Code 106](#distance-invalid---status-code-106)
  - [Jurisdiction Not Searchable - Status Code 107](#jurisdiction-not-searchable---status-code-107)
- [Invalid Zip Code Queries](#invalid-zip-code-queries)
  - [Zip Code Must Be 5 Characters - Status Code 112](#zip-code-must-be-5-characters---status-code-112)
  - [Too Many Zip Codes - Status Code 113](#too-many-zip-codes---status-code-113)
  - [Invalid Zip Code - Status Code 114](#invalid-zip-code---status-code-114)
- [Jurisdiction Issues](#jurisdiction-issues)
  - [Jurisdiction(s) Missing - Status Code 108](#jurisdictions-missing---status-code-108)
  - [Jurisdiction(s) Response Missing - Status Code 110](#jurisdictions-response-missing---status-code-110)
  - [Invalid Jurisdiction - Status Code 115](#invalid-jurisdiction---status-code-115)


# Invalid Query

## Poorly Formatted Query - Status Code 100
There was not any fields filled out for the search. The search must be a name, zip, or geographic search in order to work. For a name search, first and last name is required. For a zip search, at least one zip code is required. For a geographic search, latitude, longitude, and distance are required. All these search options require at least one jurisdiction to be valid as well.

- [Query](100/query.json) 
- [Results](100/response.json)

## Mixed Search - Status Code 102
You cannot submit a first and last name with a latitude, longitude, and distance. These must be individual searches.

- [Query](102/query.json) 
- [Results](102/response.json)

## Missing First or Last Name - Status Code 103
For a name search, you must submit a first and last name to work.

- [Query](103/query.json) 
- [Results](103/response.json)

## Invalid Characters - Status Code 109
Invalid characters in the search were found. 

- [Query](109/query.json) 
- [Results](109/response.json)

Currently the system will reject queries with the following characters:

- !
- @
- \#
- $
- %
- ^
- &
- \*
- (
- )
- :
- ;
- \
- /
- <
- \>
- \+
- =
- ?
- [
- ]
- {
- }
- |
- \
- _
- \-\-

## Too Many Characters in a Search Field - Status Code 111
There are too many characters in either first name, last name, city, or county. The maximum number of characters per field allowed is 50.

- [Query](111/query.json) 
- [Results](111/response.json)

# Invalid Geographic Queries

## Invalid Latitude or Longitude - Status Code 104
This search has an invalid latitude or longitude. Latitudes must be between -90 and 90 while longitudes must be between -180 and 180. Neither value can be 0 as well. Both must be provided.

- [Query](104/query.json) 
- [Results](104/response.json)

## Distance Missing - Status Code 105
For a geographic search, a distance must be provided.

- [Query](105/query.json) 
- [Results](105/response.json)

## Distance Invalid - Status Code 106
For a geographic search, distance must be a whole number with the value of 0.25. 0.5, 1, 2, or 3.

- [Query](106/query.json) 
- [Results](106/response.json)

## Jurisdiction Not Searchable - Status Code 107
One or more jurisdictions that was provided to do a geographic search does not support geographic searches and cannot be completed.

# Invalid Zip Code Queries

## Zip Code Must Be 5 Characters - Status Code 112
There are either too many or too few characters in one or more of the zip codes. The zip code must be 5 characters in length.

- [Query](112/query.json) 
- [Results](112/response.json)

## Too Many Zip Codes - Status Code 113
There are too many zip codes searched. The maximum number of zip codes allowed is 5.

- [Query](113/query.json) 
- [Results](113/response.json)

## Invalid Zip Code - Status Code 114
One or more of the zip codes searched is invalid. Zip codes must only contain numbers.

- [Query](114/query.json) 
- [Results](114/response.json)

# Jurisdiction Issues

## Jurisdiction(s) Missing - Status Code 108
There were no jurisdictions were supplied for the search.

- [Query](108/query.json) 
- [Results](108/response.json)

## Jurisdiction(s) Response Missing - Status Code 110
The response from the back-end did not have any jurisdictions returned. There was either a failure with the search or the jurisdictions provided to the query were invalid.

- [Query](110/query.json) 
- [Results](110/response.json)

## Invalid Jurisdiction - Status Code 115
One or more of the jurisdictions identifiers provided does not match what is expected.

- [Query](115/query.json) 
- [Results](115/response.json)