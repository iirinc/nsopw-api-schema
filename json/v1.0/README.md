# NSOPW JSON 1.0 
This repository holds the schema and samples for queries using JSON with NSOPW. 

- **Query** - This is what the jurisdiction's API should expect from NSOPW.
  - [Query Details](QueryDetails.md) - Detailed information on how queries are structured and their rules
  - [Query Schema](schema/query.schema.json) - JSON schema for how a jurisdiction should expect a query to be formatted. 
- **Response** - This is what NSOPW will expect from the jurisdiction's API.
  - [Response Details](ResponseDetails.md) - Detailed information on how the results are structured and their rules.
  - [Response Schema](schema/response.schema.json) - JSON schema for what NSOPW will expect from the jurisdiction
- **Samples** - Samples of different queries
  - [Valid Queries](samples/valid/README.md) - Samples of valid queries and their responses.
  - [Invalid Queries](samples/invalid/README.md) - Samples of invalid queries and their responses.

## Security
We would suggest using a key in the query string or header of your API endpoint to ensure that the NSOPW system is querying the service. We would suggest not locking the API down by IP address as the querying IP address may change.

```
https://jurisdiction.gov/nsopw-api/v1.0/query?key=h6zy3CXMEGvdgsScxwbkHKpejA4BDLWmftTR9JPqQZ2n875uYF
```
