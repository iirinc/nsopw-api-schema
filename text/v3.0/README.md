# Uploading Offender Files (Version 3.0)
The NSOPW database, which is set up to store the sex offender information that is uploaded from some registration jurisdictions, has been redesigned so the data is structured properly. A Secure File Transfer Protocol (SFTP) server is available for the jurisdictions that wish to continue to upload their sex offender information. The SFTP server login information will be provided via email.  
  
# File Structure for Upload  
Registration jurisdictions should upload a single JSON file with all the offender data within. 

```json
[
    {
        "jurisdictionId": "DC",
        "offenderId": "DC1000",
        "dob": "1970-01-01",
        "gender": "M",
        "url": "http://www.somesite.net/offenderdetails.php?OfndrID=10720166&AgencyID=54150",
        "thumbnail": "https://www.somesite.net/offenders/offenderimage.jpg",
        "isAbsconder": false,
        "names": {
            "id": 6767194,
            "prefix": "Dr",
            "firstName": "John",
            "middleName": "Jacob",
            "lastName": "Smith",
            "suffix": "Jr",
            "isPrimary": true
        },
        "addresses":
        [
          {
            "id": "31324314",
            "name": "The White House",
            "type": "R",
            "street": "1600 Pennsylvania Avenue NW",
            "city": "Washington",
            "county": "Fairfax",
            "state": "DC",
            "zip": "20500-0000",
            "latitude": 38.89795,
            "longitude": -77.03656
            }
        ],
        "md5": "35780DF17571E47B2F1021B203F1EAEFF"
    },
    {
        // Next offender
    }
]
```

### Offender
This is an overview of the properties of an offender.

| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `jurisdictionId` | Identifier | String | Yes | Identifier of the jurisdiction | DC |
| `offenderId` | Identifier | String | Yes | Identifier of the offender | DC1000 |
| `names` | Names and Aliases | [Name](#name) | Yes | Name information of the offender | | 
| `gender` | Gender | String | No | THe gender of the offender. Either M for Male, F for Female, or U for Other/Unknown | M |
| `dob` | Date of Birth | Date | No | The date of birth of the offender formatted in YYYY-MM-dd | 1970-01-01 |
| `addresses` | Addresses | Array of [Location](#location) | No | Array of locations/addresses of the offender | |
| `url` | Uri to Offender Record | String | Yes | A uri to view more information about the offender on the jurisdiction's website | https://www.somesite.net/offenderdetails.php?OfndrID=10720166&AgencyID=54150 |
| `thumbnail` | Uri to Offender Image | String | No | A uri to a thumbnail of the offender available on the jurisdiction's website | https://www.somesite.net/offenders/offenderimage.jpg |
| `isAbsconder` | Absconder | True/False | No | Boolean indicating if the offender is absconded or not. | false |
| `md5` | MD5 Hash | String | No | Optional MD5 hash of the offender record to assure no information has been changed | 35780DF17571E47B2F1021B203F1EAEFF |

#### Name
This is an overview of the properties of a name.

| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `id` | Identifier | String | Yes | This is the identifier of the name record. Could just be the offender's ID | DC1000-N1 |
| `prefix` | Prefix | String | No | The prefix of the offender's name. | Dr |
| `firstName` | First Name | String | Yes | The first name (given name) of the offender. | John |
| `middleName` | Middle Name | String | No | The middle name or initial of the offender. | J | 
| `lastName` | Last Name | String | No | The last name (sur or family name) of the offender. | Smith |
| `suffix` | Suffix | String | No | The suffix of the offender's name. | Jr |
| `isPrimary` | Is Primary | Boolean | Yes | Indicates if the name record is the primary name | true |

#### Location
This is an overview of the properties of an address/location
| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `id` | Identifier | String | Yes | This is the identifier of the address record. Could just be the offender's ID | DC1000-A1 |
| `name` | Location's Name | String | No | Name of the location such as company or school name. | The White House |
| `type` | Location Type | String | Yes | The type of address of the offender's location. <ul><li>R = Residence</li><li>S = School</li><li>E = Employment/Work</li></ul> | R |
| `street` | Street Address | String | No | The street address of the offender's address including number and direction. | 1600 Pennsylvania Avenue NW |
| `city` | City | String | No | The city of the offender's location. | Washington |
| `county` | County | String | No | The county of the offender's location. | Fairfax |
| `state` | State | String | No | Two letter [USPS State Abbreviation](https://faq.usps.com/s/article/What-are-the-USPS-abbreviations-for-U-S-states-and-territories) for the state or territory | DC |
| `zip` | Zip Code | String | No | Zip Code of the offender. Can include [Zip+4](https://en.wikipedia.org/wiki/ZIP_Code#ZIP+4). | 20500-0000 |
| `latitude` | Latitude | Number | No | The latitude of the offender's location in [decimal degrees](https://en.wikipedia.org/wiki/Decimal_degrees). 5 decimal places at a minimum would be prefered for accuracy.  | 38.89795 |
| `longitude` | Longitude | Number | No | The longitude of the offender's location in [decimal degrees](https://en.wikipedia.org/wiki/Decimal_degrees). 5 decimal places at a minimum would be prefered for accuracy. | -77.03656 |
