# Prestarts

## Create prestarts

`POST /public/v1/prestarts` creates new prestarts with the provided information.

+ Content-Type header `Content-Type: application/json` is required when POSTing prestarts into Gearbox.
+ Root element `prestarts` and snake case object keys are required.

### Request
```
URL: https://api.gearbox.com.au/public/v1/prestarts
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

prestarts: [{
  fleet_number: "", // string, must match existing fleet number in system, required
  completed_at: "", // datetime, required, format: yyyy-mm-dd hh:mm:ss
  employee: "", // string, required. If a match is not found then it is stored as a string
  odometer: "", // integer, not required
  hours: "", // integer, not required
  spare1: "", // string, maximum 256 characters, not required
  spare2: "", // string, maximum 256 characters, not required
  spare3: "", // string, maximum 256 characters, not required
  latitude: "", // float, not required
  longitude: "", // float, not required
  notes: "", // string, maximum 500 characters, not required
  items: [{ // required
    item_question: "", // string, maximum 256 characters, required
    passed: "", // boolean; ‘true’ or ‘false’, required
    fail_reason: "" // string, maximum 256 characters, not required
  }]
}]
```

### Response status codes:
 - 200: OK
 - 400: Bad Request
 - 401: Unauthorised
 - 403: Forbidden
 - 413: Request Entity Too Large
 - 415: Unsupported media type

###### Example
```
curl --location --request POST 'https://api.gearbox.com.au/public/v1/prestarts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    "prestarts": [
        {
            "fleet_number": "PM01",
            "completed_at": "2021-10-01 12:00:00",
            "employee": "John Smith",
            "notes": "Some notes",
            "hours": 1000,
            "odometer": 5000,
            "items": [
                {
                    "item_question": "Check registration label is in tact and current",
                    "passed": true,
                    "fail_reason": ""
                },
                {
                    "item_question": "Check all tyres for tread, damage & inflation - including spares",
                    "passed": false,
                    "fail_reason": "Damage to spare tyre"
                }
            ]
        }
    ]
}'
```
