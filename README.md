# NSOPW API Schema Documentation
This is a repository of the [NSOPW](https://www.nsopw.gov) API/Web Service Schema Documentation. This hosts all the schemas and documetation used by NSOPW to interact with jurisdictions along with sample queries and responses.

- [NSOPW API Schema Documentation](#nsopw-api-schema-documentation)
- [Overview](#overview)
- [Methods](#methods)
  - [JSON](#json)
  - [XML](#xml)
  - [File Upload](#file-upload)

# Overview
The NSOPW system has the ability to query jurisidictions for sex offender information by a few different methodologies. The prefered method is via an [API](json/v1.0/README.md)/[web service](xml/v2.0/). This allows the public to have the most accurate data as it is live. Another option is for jurisdictions to daily [upload files](text/v2.0/README.md) to the system via [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol). 

When the public makes a search on the NSOPW site, the queries are handed to a back end process that then queries each individual jurisdiction for the potentially matching sex offender data, queries the database of the uploaded offender data, then combines it and returns it to the public.

# Methods
Below are the different methods for providing sex offender information to NSOPW. 

## JSON
This is a [JSON schema](https://json-schema.org/) interaction using a [RESTful](https://aws.amazon.com/what-is/restful-api/) services for jurisdictions to use.

- [Version 1.0](json/v1.0/README.md)
  - **Query** - This is what the jurisdiction's API should expect from NSOPW.
    - [Query Details](json/v1.0/QueryDetails.md) - Detailed information on how queries are structured and their rules.
    - [Query Schema](json/v1.0/schema/query.schema.json) - JSON schema for how a jurisdiction should expect a query to be formatted. 
  - **Response** - This is what NSOPW will expect from the jurisdiction's API.
    - [Response Details](json/v1.0/ResponseDetails.md) - Detailed information on how the results are structured and their rules.
    - [Response Schema](json/v1.0/schema/response.schema.json) - JSON schema for what NSOPW will expect from the jurisdiction
  - **Samples** - Samples of different queries
    - [Valid Queries](json/v1.0/samples/valid/README.md) - Samples of valid queries and their responses.
    - [Invalid Queries](json/v1.0/samples/invalid/README.md) - Samples of invalid queries and their responses.

## XML
This is [XML schemas](https://en.wikipedia.org/wiki/XML_schema) for interaction using [web services](https://en.wikipedia.org/wiki/Web_service). This is no longer the prefered method and is being phased out. A [JSON](#json) based service is the new prefered method.

- [Version 2.0](xml/v2.0/) - This is the schema for XML interactions using [NIEM](https://www.niem.gov/) schema.
- [Version 1.0](xml/v1.0/) - This is the schema for XML interactions using [GJXDM](https://bja.ojp.gov/program/it/national-initiatives/gjxdm) schema. 

## File Upload
This is instructions on how you can upload files to the system instead of providing an API. 

- [Version 2.0](text/v2.0/README.md) - This is for uploading a 3 files with the offender information information.