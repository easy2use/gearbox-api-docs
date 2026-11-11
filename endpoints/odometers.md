# Odometers

- [Get odometers](#get-odometers)
- [Create odometers](#create-odometers)
- [Create odometer documents](#create-odometer-documents)
- [Update an odometer](#update-an-odometer)

## Get odometers

`GET /public/v1/odometers` retrieves a [paginated list](../readme.md/#pagination) of odometers.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>

- id
- fleet_number
- registration
- date
- number
- odometer
- odometer_visual_reading
- odometer_offset_used
- hours
- hours_visual_reading
- hours_offset_used
- created_at
- updated_at
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/odometers
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "odometers": [
    {
      "id": 123,
      "fleet_number": "PM01",
      "registration": "ABC123",
      "date": "2021-01-01",
      "number": 45,
      "odometer": 1000,
      "odometer_visual_reading": 0,
      "odometer_offset_used": 0,
      "hours": 10,
      "hours_visual_reading": 0,
      "hours_offset_used": 0,
      "created_at": "2021-01-01T10:00:00.000+11:00",
      "updated_at": "2021-01-01T10:00:00.000+11:00"
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/odometers' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create odometers

`POST /public/v1/odometers` creates a new odometer with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing odometers into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/odometers
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "", // string, required if registration is blank, must match existing fleet number in the system
  registration: "", // string, required if fleet_number is blank, must match existing registration in the system
  date: "",         // date, required, format: 'yyyy-mm-dd'
  odometer: 0,      // integer, required if hours is blank, must provide the visual usage reading of the odometer
  hours: 0          // integer, required if odometer is blank, must provide the visual usage reading for hours
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/odometers' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    "fleet_number": "PM01",
    "registration": "ABC123",
    "date": "2021-01-01",
    "odometer": "1000",
    "hours": "10"
}'
```

## Create odometer documents

`POST /public/v1/odometer/:id/documents` creates a document attaching it to a provided odometer `:id`.

Please note:

- This endpoint requires Documents API access for the OAuth application and the Documents module to be enabled for your account.
- Use the odometer ID returned when creating or retrieving an odometer.
- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/odometer/123/documents?name=certificate.pdf
Method: POST
Content-Type: application/pdf
Content-Length: 123
Authorization: Bearer $ACCESS_TOKEN
```

### Response status codes:

- 201: Created
- 400: Bad Request
- 401: Unauthorised
- 403: Forbidden
- 411: Length Required
- 413: Request Entity Too Large
- 415: Unsupported media type
- 422: Unprocessable Entity

### 201 - Successful response

```JSON
{
  "id": 456
}
```

The returned ID identifies the created document.

###### Example

```shell
curl --location --request POST 'https://api.gearbox.com.au/public/v1/odometer/123/documents?name=certificate.pdf' \
--header 'Content-Type: application/pdf' \
--header 'Accept: application/json' \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data-binary '@certificate.pdf'
```

Curl sets the Content-Length header from the file size.

## Update an odometer

`PATCH /public/v1/odometers/:id` updates the odometer with the provided `:id`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing odometers into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/odometers/1
Method: PATCH
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  mileage: {
    fleet_number: "", // string, optional, maximum length 12 characters, must match an existing fleet number
    registration: "", // string, optional, maximum length 12 characters, must match an existing registration
    date: "",         // date, optional, format: 'yyyy-mm-dd'
    odometer: 0,      // integer, optional, must provide the visual usage reading of the odometer
    hours: 0          // integer, optional, must provide the visual usage reading for hours
  }
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
curl --location --request PATCH 'https://api.gearbox.com.au/public/v1/odometers/123' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data-raw '{
  "mileage": {
    "date": "2021-01-02",
    "odometer": 1200,
    "hours": 12
  }
}'
```
