# Sites

- [Get sites](#get-sites)
- [Create sites](#create-sites)
- [Update a site](#update-a-site)

## Get sites

`GET /public/v1/sites` retrieves a [paginated list](../readme.md/#pagination) of sites.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>
  
- id
- label
- date_open
- date_closed
- address1
- address2
- city
- state
- postcode
- notes
- group
- closed
- vehicles.fleet_number
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/sites
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "sites": [
    {
      "id": 7711,
      "label": "Oran Park",
      "date_open": "2020-01-01",
      "date_closed": "2023-10-01",
      "address1": "1 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "notes": "Oran Park workshop",
      "group": "Sydney",
      "closed": true,
      "vehicles": [
        {
          "fleet_number": "PM001"
        }
      ]
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/sites' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create sites

`POST /public/v1/sites` creates a new site with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing sites into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/sites
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  label: "",       // string, required, maximum 50 characters
  date_open: "",   // date, required, format: 'yyyy-mm-dd'
  date_closed: "", // date, required if closed is true, format: 'yyyy-mm-dd'
  address1: "",    // string, optional, maximum 255 characters
  address2: "",    // string, optional, maximum 255 characters
  city: "",        // string, optional, maximum 255 characters
  state: "",       // string, optional, maximum 255 characters
  postcode: "",    // string, optional, maximum 255 characters
  notes: "",       // string, optional, maximum 255 characters
  group: "",       // string, optional, must matching existing Vehicle Group
  closed: "",      // boolean, optional
  vehicles: [      // array, optional, size must be less than or equal to 5
    {
      fleet_number: "" // string, optional, must match existing Vehicle fleet number or registration
    }
  ]
}
```

### Response status codes:

- 201: Created
- 400: Bad Request
- 401: Unauthorised
- 403: Forbidden
- 413: Request Entity Too Large
- 415: Unsupported media type
- 422: Unprocessable Entity

### 201 - Successful response

```JSON
{
  "id": 123
}
```

###### Example

```
curl --location --request POST http://api.gearbox.com.au/public/v1/sites \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "label": "Oran Park",
      "date_open": "2020-01-01",
      "date_closed": "2023-10-01",
      "address1": "1 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "notes": "Oran Park workshop",
      "group": "Sydney",
      "closed": true,
      "vehicles": [
        {
          "fleet_number": "PM001"
        }
      ]
    }' 
```

## Update a site

`PATCH /public/v1/sites/:id` updates the vehicle with the provided `:id`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing sites into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/sites/1
Method: PATCH
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  label: "",       // string, required, maximum 50 characters
  date_open: "",   // date, required, format: 'yyyy-mm-dd'
  date_closed: "", // date, required if closed is true, format: 'yyyy-mm-dd'
  address1: "",    // string, optional, maximum 255 characters
  address2: "",    // string, optional, maximum 255 characters
  city: "",        // string, optional, maximum 255 characters
  state: "",       // string, optional, maximum 255 characters
  postcode: "",    // string, optional, maximum 255 characters
  notes: "",       // string, optional, maximum 255 characters
  group: "",       // string, optional, must matching existing Vehicle Group
  closed: "",      // boolean, optional
  vehicles: [      // array, optional, size must be less than or equal to 5
    {
      fleet_number: "" // string, optional, must match existing Vehicle fleet number or registration
    }
  ]
}
```

### Response status codes:

- 200: OK
- 400: Bad Request
- 401: Unauthorised
- 403: Forbidden
- 404: Not Found
- 413: Request Entity Too Large
- 415: Unsupported media type
- 422: Unprocessable Entity

### 200 - Successful response

```JSON
{
  "id": 123
}
```

###### Example

```
curl --location --request PATCH http://api.gearbox.com.au/public/v1/sites/6245 \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "label": "Oran Park",
      "date_open": "2020-01-01",
      "date_closed": "2023-10-01",
      "address1": "1 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "notes": "Oran Park workshop",
      "group": "Sydney",
      "closed": true,
      "vehicles": [
        {
          "fleet_number": "PM001"
        }
      ]
    }' 
```
