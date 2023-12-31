# Response
This is an overview of the response that would be expected from the NSOPW system.

- [Response](#response)
  - [Response Time](#response-time)
  - [Validation Messages](#validation-messages)
    - [Primary Status Code](#primary-status-code)
      - [HTTP 200 - OK](#http-200---ok)
        - [Status Message Per Jurisdiction](#status-message-per-jurisdiction)
      - [HTTP 422 - Validation Error](#http-422---validation-error)
      - [HTTP 429 - Too many requests](#http-429---too-many-requests)
      - [HTTP 500 - Internal Server Error](#http-500---internal-server-error)
      - [HTTP 503 - Service Unavailable](#http-503---service-unavailable)
  - [Property Definitions](#property-definitions)
    - [Offender](#offender)
      - [Name](#name)
      - [Location](#location)
    - [Jurisdiction Status](#jurisdiction-status)

## Response Time
The system expects that when it queries a jurisdiction, the jurisdiction will respond within 15 seconds. If the jurisdiction doesn't respond within 15 seconds, the system closes the connection and flags the jurisdiction as timing out. If the system notices too many time out within a period of time, the system will remove the jurisdiction from being queried by the public for 5 minutes. After 5 minutes, the system will test to see if a search for the jurisdiction is completed within 15 seconds or not. If it does, the system brings the jurisdiction back online for the public to search. If not, it will try again in 5 minutes.

This keeps the public from having to wait a long time for their results and not to slow down all the other searches due to one jurisdiction having issues.

If a jurisdiction is offline for more than 30 minutes, an automated email will be sent to the contacts on file letting them know.

## Validation Messages
These are the messages that should be returned by your API based on different statuses. 

The NSOPW back-end when querying the API would not break the search standards but if it did, these would be the status messages to be returned so the system would understand the error.

### Primary Status Code
Based in the schema, these would be returned via the HTTP status and within the JSON in the `statusCode` field for the whole result.

#### HTTP 200 - OK
On a succesful query, return a [HTTP 200](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200) and one of the following values in the `statusCode`.

- **200** - Sucessful search.
- **201** - Sucessful search but the jurisdictions passed in the query do not match the jurisdiction that responded. 

##### Status Message Per Jurisdiction
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

- **100** - *[Invalid Query](samples/invalid/README.md#poorly-formatted-query---status-code-100)* - There was not any fields filled out for the search. The search must be a name, zip, or geographic search in order to work. For a name search, first and last name is required. For a zip search, at least one zip code is required. For a geographic search, latitude, longitude, and distance are required. All these search options require at least one jurisdiction to be valid as well.
- **102** - *[Mixed Search](samples/invalid/README.md#mixed-search---status-code-102)* - You cannot submit a first and last name with a latitude, longitude, and distance. These must be individual searches.
- **103** - *[Missing First or Last Name](samples/invalid/README.md#missing-first-or-last-name---status-code-103)* - For a name search, you must submit a first and last name to work.
- **104** - *[Geographic - Invalid Lat/Lon](samples/invalid/README.md#invalid-latitude-or-longitude---status-code-104)* - This search has an invalid latitude or longitude. Latitudes must be between -90 and 90 while longitudes must be between -180 and 180.
- **105** - *[Geographic - Distance Missing](samples/invalid/README.md#distance-missing---status-code-105)* - For a geographic search, a distance must be provided.
- **106** - *[Geographic - Distance Invalid](samples/invalid/README.md#distance-invalid---status-code-106)* - For a geographic search, distance must be a whole number with the value of 0.25. 0.5, 1, 2, or 3.
- **107** - *[Geographic - Jurisdiction Not Searchable](samples/invalid/README.md#jurisdiction-not-searchable---status-code-107)* - One or more jurisdictions that was provided to do a geographic search does not support geographic searches and cannot be completed.
- **108** - *[Jurisdiction(s) Missing](samples/invalid/README.md#jurisdictions-missing---status-code-108)* - There were no jurisdictions were supplied for the search.
- **109** - *[Invalid Characters](samples/invalid/README.md#invalid-characters---status-code-109)* - Invalid characters in the search were found.
- **110** - *[Jurisdiction(s) Response Missing](samples/invalid/README.md#jurisdictions-response-missing---status-code-110)* - The response from the back-end did not have any jurisdictions returned. There was either a failure with the search or the jurisdictions provided to the query were invalid.
- **111** - *[Too many characters in a search field](samples/invalid/README.md#too-many-characters-in-a-search-field---status-code-111)* - There are too many characters in either first name, last name, city, or county. The maximum number of characters per field allowed is 50.
- **112** - *[Zip code must be 5 characters](samples/invalid/README.md#zip-code-must-be-5-characters---status-code-112)* - There are either too many or too few characters in one or more of the zip codes. The zip code must be 5 characters in length.
- **113** - *[Too many zip codes searched](samples/invalid/README.md#too-many-zip-codes---status-code-113)* - There are too many zip codes searched. The maximum number of zip codes allowed is 5.
- **114** - *[Invalid Zip Code](samples/invalid/README.md#invalid-zip-code---status-code-114)* - One or more of the zip codes searched is invalid. Zip codes must only contain numbers.
- **115** - *[Invalid Jurisdiction](samples/invalid/README.md#invalid-jurisdiction---status-code-115)* - One or more of the jurisdictions identifiers provided does not match what is expected.

#### HTTP 429 - Too many requests
If the API has a limit to the number of searches made and the request limit has exceded this number, return a [HTTP 429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429). No response would be expected in the body.

#### HTTP 500 - Internal Server Error
If the jurisdiction's API has an unhandled exception, return a [HTTP 500](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500). No response would be expected in the body.

#### HTTP 503 - Service Unavailable
If the API is unavilable, return a [HTTP 503](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503). No response would be expected in the body.

## Property Definitions
This is an overview of the properties within a response. These are all defined in the [schema](schema/response.schema.json) file as well.

```json
{
  "id": "000000-0000-0000-00000000000",
  "statusCode": 200,
  "offenders": [
    {
      "id": "DC1000",
      "name": {
        "prefix": "Dr",
        "firstName": "John",
        "middleName": "Jacob",
        "lastName": "Smith",
        "suffix": "Jr"
      },
      "aliases": [
        {
          "prefix": "Dr",
          "firstName": "John",
          "middleName": "J",
          "lastName": "Smith",
          "suffix": "Jr"
        }
      ],
      "gender": "M",
      "dob": "1970-01-01",
      "locations": [
        {
          "name": "The White House",
          "type": "R",
          "streetAddress": "1600 Pennsylvania Avenue NW",
          "city": "Washington",
          "county": "Fairfax",
          "state": "DC",
          "zipCode": "20500-0000",
          "latitude": 38.89795,
          "longitude": -77.03656
        }
      ],
      "offenderUri": "http://www.somesite.net/offenderdetails.php?OfndrID=10720166&AgencyID=54150",
      "imageUri": "https://www.somesite.net/offenders/offenderimage.jpg",
      "absconder": false,
      "jurisdictionId": "DC"
    }
  ],
  "jurisdictionStatus": [
    {
      "jurisdictionId": "DC",
      "statusCode": 200,
      "records": 1
    }
  ]
}
```


| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `id` | Search Identifier | String | No | This is a unique id of the search provided. This would be used for diagnostics and logging to track queries on the NSOPW site and potentially the jurisdiction side. | 000000-0000-0000-00000000000 |
| `statusCode` | Status Code | Integer | Yes | This is the status of the whole search as detailed in [primary status code](#primary-status-code) | 200 |
| `offenders` | Offenders | Array of [Offender](#offender) | No | This is an array of offenders that match the search | | 
| `jurisdictionStatus` | Jurisdiction Status | Array of [Jurisdiction Status](#jurisdiction-status) | Yes if search is successful | This is an array of jurisdictions that were searched along with their status | |

### Offender
This is an overview of the properties of an offender.

| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `id` | Identifier | String | No | Identifier of the offender | DC1000 |
| `name` | Primary Name | [Name](#name) | Yes | Primary name information of the offender | | 
| `aliases` | Aliases | Array of [Name](#name) | No | Any aliases of the offender | |
| `gender` | Gender | String | No | THe gender of the offender. Either M for Male, F for Female, or U for Other/Unknown | M |
| `dob` | Date of Birth | Date | No | The date of birth of the offender formatted in YYYY-MM-dd | 1970-01-01 |
| `locations` | Locations | Array of [Location](#location) | No | Array of locations/addresses of the offender | |
| `offenderUri` | Uri to Offender Record | String | Yes | A uri to view more information about the offender on the jurisdiction's website | https://www.somesite.net/offenderdetails.php?OfndrID=10720166&AgencyID=54150 |
| `imageUri` | Uri to Offender Image | String | No | A uri to a thumbnail of the offender available on the jurisdiction's website | https://www.somesite.net/offenders/offenderimage.jpg |
| `absconder` | Absconder | True/False | No | Boolean indicating if the offender is absconded or not. | false |
| `jurisdictionId` | Offender [Jurisdiction Identifier](QueryDetails.md#jurisdiction) | String | Yes | Identifier of the jurisdiction from which the offender is from. | DC |

#### Name
This is an overview of the properties of a name.

| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `prefix` | Prefix | String | No | The prefix of the offender's name. | Dr |
| `firstName` | First Name | String | Yes | The first name (given name) of the offender. | John |
| `middleName` | Middle Name | String | No | The middle name or initial of the offender. | J | 
| `lastName` | Last Name | String | No | The last name (sur or family name) of the offender. | Smith |
| `suffix` | Suffix | String | No | The suffix of the offender's name. | Jr |

#### Location
This is an overview of the properties of a location
| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `name` | Location's Name | String | No | Name of the location such as company or school name. | The White House |
| `type` | Location Type | String | Yes | The type of address of the offender's location. <ul><li>R = Residence</li><li>S = School</li><li>E = Employment/Work</li></ul> | R |
| `streetAddress` | Street Address | String | No | The street address of the offender's address including number and direction. | 1600 Pennsylvania Avenue NW |
| `city` | City | String | No | The city of the offender's location. | Washington |
| `county` | County | String | No | The county of the offender's location. | Fairfax |
| `state` | State | String | No | Two letter [USPS State Abbreviation](https://faq.usps.com/s/article/What-are-the-USPS-abbreviations-for-U-S-states-and-territories) for the state or territory | DC |
| `zipCode` | Zip Code | String | No | Zip Code of the offender. Can include [Zip+4](https://en.wikipedia.org/wiki/ZIP_Code#ZIP+4). | 20500-0000 |
| `latitude` | Latitude | Number | No | The latitude of the offender's location in [decimal degrees](https://en.wikipedia.org/wiki/Decimal_degrees). 5 decimal places at a minimum would be prefered for accuracy.  | 38.89795 |
| `longitude` | Longitude | Number | No | The longitude of the offender's location in [decimal degrees](https://en.wikipedia.org/wiki/Decimal_degrees). 5 decimal places at a minimum would be prefered for accuracy. | -77.03656 |

### Jurisdiction Status
This is an overview of the properties of a jurisdiction status.

| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `jurisdictionId` | [Jurisdiction Identifier](QueryDetails.md#jurisdiction) | String | Yes | Identifier for the jurisdiction searched. | DC |
| `statusCode` | Status Code | Integer | Yes | This is the status of the single jurisdiction as detailed [above](#status-message-per-jurisdiction) | 200 |
| `records` | Records Found | Integer | Yes | This is the number of records found per jurisdiction | 2 |
| `message` | Message | String | No | Any additional information to pass about the search results such as errors. | |
