# Odometers

- [Create odometers](#create-odometers)

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
  odometer: 0,      // integer, required if hours is blank
  hours: 0          // integer, required if odometer is blank
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
