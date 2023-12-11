# Query
This is an overview of the query that you would be receiving from the NSOPW system.

- [Query](#query)
  - [Property Definitions](#property-definitions)
  - [Query Types](#query-types)
    - [Name Search](#name-search)
    - [Zip Code Search](#zip-code-search)
    - [Geolocation Search](#geolocation-search)
    - [All Results](#all-results)
- [Jurisdiction](#jurisdiction)
  - [States](#states)
  - [Territories](#territories)
  - [Tribal](#tribal)


## Property Definitions
This is an overview of the properties within a query. These are all defined in the [schema](schema/query.schema.json) file as well.

| Field | Title | Type | Is Required | Description | Example |
|-------|-------|------|-------------|-------------|---------|
| `id` | Search Identifier | String | No | This is a unique id of the search provided. This would be used for diagnostics and logging to track queries on the NSOPW site and potentially the jurisdiction side. | 000000-0000-0000-00000000000 |
| `jurisdictions` | Jurisdictions To Search | Array | Yes | This is an array of jurisdictions to search. This normally would only be one jurisdiction but in the case where a system hosts multiple jurisdictions, it could allow for more being queried at once. See [Jurisdiction](#jurisdiction) for a list of the potential values to expect from NSOPW. | [ "DC" ] |
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

# Jurisdiction
This is a list of the jurisdiction identifier and the name of the jurisdiction.

## States

| Identifier | Name |
|------------|------|
| AK | Alaska |
| AL | Alabama |
| AR | Arkansas |
| AZ | Arizona |
| CA | California |
| CO | Colorado |
| CT | Connecticut |
| DC | District of Columbia |
| DE | Delaware |
| FL | Florida |
| GA | Georgia |
| HI | Hawaii |
| IA | Iowa |
| ID | Idaho |
| IL | Illinois |
| IN | Indiana |
| KS | Kansas |
| KY | Kentucky |
| LA | Louisiana |
| MA | Massachusetts |
| MD | Maryland |
| ME | Maine |
| MI | Michigan |
| MN1 | Minnesota - Department of Corrections |
| MN2 | Minnesota - Bureau of Criminal Apprehension |
| MO | Missouri |
| MS | Mississippi |
| MT | Montana |
| NC | North Carolina |
| ND | North Dakota |
| NE | Nebraska |
| NH | New Hampshire |
| NJ | New Jersey |
| NM | New Mexico |
| NV | Nevada |
| NY | New York |
| OH | Ohio |
| OK | Oklahoma |
| OR | Oregon |
| PA | Pennsylvania |
| RI | Rhode Island |
| SC | South Carolina |
| SD | South Dakota |
| TN | Tennessee |
| TX | Texas |
| UT | Utah |
| VA | Virginia |
| VT | Vermont |
| WA | Washington |
| WI | Wisconsin |
| WV | West Virginia |
| WY | Wyoming |

## Territories

| Identifier | Name |
|------------|------|
| AMERICANSAMOA | American Samoa |
| CNMI | Northern Mariana Islands |
| GU | Guam |
| PR | Puerto Rico |
| USVI | U.S. Virgin Islands |

## Tribal

| Identifier | Name |
|------------|------|
| ACTRIBE | Alabama-Coushatta Tribes of Texas |
| AKCHIN | Ak-Chin Indian Community |
| ASTRIBE | Absentee Shawnee Tribe of Oklahoma |
| BAYMILLS | Bay Mills Indian Community |
| BLACKFEET | Blackfeet Nation |
| BOISFORTE | Bois Forte Band of Chippewa |
| CADDO | Caddo Nation of Oklahoma |
| CATRIBES | Cheyenne and Arapaho Tribes of Oklahoma |
| CCST | Crow Creek Sioux Tribe |
| CHEHALIS | Confederated Tribe of the Chehalis Reservation |
| CHEROKEE | Cherokee Nation |
| CHEYENNERIVER | Cheyenne River Sioux Tribe |
| CHICKASAW | Chickasaw Nation |
| CHIPPEWACREE | Chippewa Cree Tribe |
| CHITIMACHA | Chitimacha Tribe of Louisiana |
| CHOCTAW | Mississippi Band of Choctaw Indians |
| COCHITI | Pueblo of Cochiti |
| COCOPAH | Cocopah Tribe |
| COEURDALENE | Coeur d'Alene Tribe |
| COLVILLETRIBES | Confederated Tribes of the Colville Reservation |
| COMANCHE | Comanche Nation |
| CRIT | Colorado River Indian Tribe |
| CROWNATIONS | Crow Tribe |
| CTUIR | Confederated Tribes of the Umatilla Indian Reservation |
| DNATION | Delaware Nation |
| ELWHA | Lower Elwha Klallam Tribe |
| ELY | Ely Shoshone Tribe |
| ESTOO | Eastern Shawnee Tribe of Oklahoma |
| FORTPECKTRIBES | Fort Peck Assiniboine and Sioux Tribes |
| FSST | Flandreau Santee Sioux Tribe |
| FTBELKNAP | Fort Belknap Indian Community |
| FTMCDOWELL | Fort McDowell Yavapai Nation |
| GRIC | Gila River Indian Community |
| GTB | Grand Traverse Band of Ottawa and Chippewa Indians |
| HANNAHVILLE | Hannahville Indian Community |
| HAVASUPAI | Havasupai Tribe |
| HOPI | Hopi Tribe |
| HUALAPAI | Hualapai Tribe |
| IOWANATION | Iowa Tribe of Oklahoma |
| IOWATRIBE | Iowa Tribe of Kansas and Nebraska |
| ISLETA | Pueblo of Isleta |
| JEMEZ | Pueblo of Jemez |
| JICARILLA | Jicarilla Apache Nation |
| KAIBABPAIUTE | Kaibab Paiute Tribe |
| KALISPELTRIBE | Kalispel Tribe of Indians |
| KAW | Kaw Nation |
| KBIC | Keweenaw Bay Indian Community |
| KICKAPOO | Kickapoo Tribe of Oklahoma |
| KOOTENAI | Kootenai Tribe of Idaho |
| LACVIEUX | Lac Vieux Desert Band of Lake Superior Chippewa Indians |
| LAGUNA | Pueblo of Laguna |
| LITTLERIVERBAND | Little River Band of Ottawa Indians |
| LOWERBRULE | Lower Brule Sioux Tribe |
| LUMMI | Lummi Nation |
| MAKAH | Makah Indian Tribe |
| MATCH-E-BE-NASH-SHE-WISH | Match-e-be-nash-she-wish Band of Pottawatomi Indians of Michigan |
| MESCALEROAPACHE | Mescalero Apache Tribe |
| MESKWAKI | Sac and Fox Tribe of the Mississippi in Iowa (Meskwaki Nation) |
| METLAKATLA | Metlakatla Indian Community |
| MHANATION | Three Affiliated Tribes |
| MIAMINATION | Miami Tribe of Oklahoma |
| MICCOSUKEETRIBE | Miccosukee Tribe of Indians of Florida |
| MITW | Menominee Indian Tribe of Wisconsin |
| MODOC | Modoc Tribe of Oklahoma |
| MOJAVEINDIANTRIBE | Fort Mojave Indian Tribe |
| MPTN | Mashantucket Pequot Tribal Nation |
| MUCKLESHOOT | Muckleshoot Indian Tribe |
| MUSCOGEE | Muscogee (Creek) Nation |
| NAVAJO | Navajo Nation |
| NCCHEROKEE | Eastern Band of Cherokee Indians |
| NEZPERCE | Nez Perce Tribe |
| NHBPI | Nottawaseppi Huron Band of the Potawatomi |
| NISQUALLY | Nisqually Indian Tribe |
| NOOKSACK | Nooksack Indian Tribe |
| NORTHERNARAPAHO | Northern Arapaho Tribe of the Wind River Reservation |
| NORTHERNCHEYENNE | Northern Cheyenne Tribe |
| ODAWA | Little Traverse Bay Band of Odawa Indians |
| OGLALA | Oglala Sioux Tribe |
| OHKAYOWINGEH | Ohkay Owingeh Tribe |
| OMAHA | Omaha Nation |
| OMTRIBE | Otoe-Missouria Tribe |
| ONEIDA | Oneida Indian Nation |
| ONONDAGA | Onondaga Indian Nation |
| OSAGE | Osage Nation |
| OTTAWATRIBE | Ottawa Tribe of Oklahoma |
| PASCUAYAQUI | Pascua Yaqui Tribe |
| PAWNEENATION | Pawnee Nation of Oklahoma |
| PBPNATION | Prairie Band Potawatomi Nation |
| PCI | Poarch Band of Creek Indians |
| PEORIATRIBE | Peoria Tribe of Indians of Oklahoma |
| PLPT | Pyramid Lake Paiute Tribe of Nevada |
| POKAGON | Pokagon Band of Potawatomi Indians |
| PORTGAMBLE | Port Gamble S'Klallam Tribe |
| POTAWATOMI | Citizen Potawatomi Nation |
| PUEBLOOFACOMA | Pueblo of Acoma |
| PUYALLUPTRIBE | Puyallup Tribe of Indians |
| QUAPAW | Quapaw Tribe of Oklahoma  |
| QUINAULT | Quinault Indian Nation |
| REDLAKE | Red Lake Band of Chippewa Indians |
| ROSEBUD | Rosebud Sioux Tribe |"
| RSIC | Reno-Sparks Indian Colony |
| SACANDFOXNATION | Sac and Fox Nation of Oklahoma |
| SAGINAW | Saginaw Chippewa Indian Tribe of Michigan |
| SANFELIPE | Pueblo of San Felipe |
| SANIPUEBLO | Pueblo de San Ildefonso |
| SANTAANA | Pueblo of Santa Ana |
| SANTACLARA | Pueblo of Santa Clara |
| SANTEE | Santee Sioux Nation |
| SANTODOMINGO | Kewa Pueblo |
| SAULTSAINTEMARIE | Sault Sainte Marie Tribe of Chippewa Indians of Michigan |
| SBTRIBES | Shoshone-Bannock Tribes |
| SCAT | San Carlos Apache Tribe |
| SCTRIBE | Seneca-Cayuga Tribe of Oklahoma |
| SEMINOLEFL | Seminole Indian Tribe |
| SEMINOLENATION | Seminole Nation of Oklahoma |
| SHOALWATERBAY | Shoalwater Bay Indian Tribe |
| SHOSHONEPAIUTE | Shoshone-Paiute Tribes |
| SHOSHONEPAIUTE2 | Shoshone-Paiute Tribes (in Idaho) |
| SHOSHONEWY | Eastern Shoshone Tribe of the Wind River Reservation |
| SKOKOMISH | Skokomish Indian Tribe |
| SOUTHERNUTE | Southern Ute Indian Tribe |
| SPIRITLAKE | Spirit Lake Nation |
| SPOKANETRIBE | Spokane Tribe of Indians |
| SQUAXINISLAND | Squaxin Island Tribe |
| SRPMIC | Salt River Pima-Maricopa Indian Community |
| SRST | Standing Rock Sioux Tribe |
| SUQUAMISH | Suquamish Tribe |
| SWINOMISH | Swinomish Indian Tribal Community |
| SWO | Sisseton Wahpeton Oyate |
| TEMOAKTRIBE | Te-Moak Tribe of Western Shoshone |
| TESUQUE | Pueblo of Tesuque |
| TMBCI | Turtle Mountain Band of Chippewa Indians |
| TONATION | Tohono O'odham Nation |
| TONAWANDA | Tonawanda Band of Seneca |
| TONKAWA | Tonkawa Tribe of Oklahoma |
| TONTOAPACHE | Tonto Apache Tribe |
| TULALIP | Tulalip Tribes |
| TUSCARORA | Tuscarora Nation |
| UNITEDKEETOOWAHBAND | United Keetoowah Band of Cherokee Indians  |
| UPPERSKAGIT | Upper Skagit Indian Tribe |
| UTETRIBE | Ute Indian Tribe |
| WAMPANOAG | Wampanoag Tribe of Gay Head (Aquinnah) |
| WARMSPRINGS | Confederated Tribes of the Warm Springs Reservation |
| WASHOETRIBE | Washoe Tribe of Nevada and California |
| WINNEBAGOTRIBE | Winnebago Tribe of Nebraska |
| WMAT | White Mountain Apache Tribe |
| WYANDOTTE | Wyandotte Nation |
| YAKAMA | Confederated Tribes and Bands of the Yakama Nation |
| YANKTON | Yankton Sioux Tribe |
| YAVAPAIAPACHE | Yavapai-Apache Nation |
| YPIT | Yavapai-Prescott Indian Tribe |
| ZUNI | Pueblo of Zuni |
