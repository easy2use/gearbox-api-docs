# Prestarts

## Create prestarts

`POST /public/v1/prestarts` creates a new prestart with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing prestarts into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/prestarts
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "",       // string, required, must match existing fleet number in system
  completed_at: "",       // datetime, required, format: yyyy-mm-dd hh:mm:ss
  employee: "",           // string, required, if a match is not found then it is stored as a string
  odometer: "",           // integer, optional
  hours: "",              // integer, optional
  spare1: "",             // string, optional, maximum 256 characters
  spare2: "",             // string, optional, maximum 256 characters
  spare3: "",             // string, optional, maximum 256 characters
  latitude: "",           // float, optional
  longitude: "",          // float, optional
  notes: "",              // string, optional, maximum 500 characters
  items: [                // array, required, size must be less than or equal to 50
    {
      item_question: "",  // string, required, maximum 256 characters
      passed: "",         // boolean, required, true or false
      fail_reason: ""     // string, optional, maximum 256 characters
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/prestarts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
  "fleet_number": "PM01",
  "completed_at": "2021-10-01 12:00:00",
  "employee": "John Smith",
  "notes": "Some notes",
  "hours": 1000,
  "odometer": 5000,
  "items": [
      {
          "item_question": "Check registration label is intact and current",
          "passed": true,
          "fail_reason": ""
      },
      {
          "item_question": "Check all tyres for tread, damage & inflation - including spares",
          "passed": false,
          "fail_reason": "Damage to spare tyre"
      }
  ]
}'
```
