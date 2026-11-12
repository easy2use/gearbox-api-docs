# Vehicle Inspection Types

- [Get vehicle inspection types](#get-vehicle-inspection-types)
- [Create vehicle inspection types](#create-vehicle-inspection-types)

## Get vehicle inspection types

`GET /public/v1/vehicle_inspection_types` retrieves a [paginated list](../readme.md/#pagination) of vehicle inspection types.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attribute names</summary>
<br>

- id
- vehicle_id
- fleet_number
- registration
- inspection_type
- last_date
- interval
- next_due
- active
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicle_inspection_types
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "vehicle_inspection_types": [
    {
      "id": 5678,
      "vehicle_id": 1234,
      "fleet_number": "PM05",
      "registration": "ABC123",
      "inspection_type": "Standard Inspection",
      "last_date": "2026-08-01",
      "interval": 12,
      "next_due": "2027-08-01",
      "active": true,
      "created_at": "2026-08-09T23:30:26.000+10:00",
      "updated_at": "2026-08-10T10:47:10.100+10:00"
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/vehicle_inspection_types' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create vehicle inspection types

`POST /public/v1/vehicle_inspection_types` creates a new vehicle inspection type with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing vehicle inspection types into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicle_inspection_types
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
    fleet_number: "",          // string, required if registration is blank, must match existing fleet number in the system
    registration: "",          // string, required if fleet_number is blank, must match existing registration in the system
    inspection_type: "",       // string, required, if a match is not found then an error is thrown
    next_due: "",              // date, optional, format: 'yyyy-mm-dd'
    active: ""                 // boolean, optional, true or false
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/vehicle_inspection_types' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    "fleet_number": "PM01",
    "registration": "ABC123",
    "inspection_type": "Standard Inspection",
    "next_due": "2026-08-01",
    "active": true
}'
```
