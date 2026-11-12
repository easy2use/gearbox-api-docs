# Premium Prestarts

## Create premium prestarts

`POST /public/v1/premium/prestarts` creates a prestart with the provided information and supports signature and image attachments.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing prestarts into Gearbox.
- Snake case object keys are required.
- The OAuth application must have Premium API and prestart access enabled.
- Attachment `base64` values should be Base64 data URIs

### Request

```
URL: https://api.gearbox.com.au/public/v1/premium/prestarts
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "",       // string, required, must match existing fleet number in system
  completed_at: "",       // datetime, required, format: `yyyy-mm-dd hh:mm:ss +0000` - if timezone is not included it will be inferred from the businesses timezone
  employee: "",           // string, required, if a match is not found then it is stored as a string
  employee_number: "",    // integer, optional, if there are multiple employees with the same name you may use this field to specify your search
  odometer: "",           // integer, optional
  hours: "",              // integer, optional
  spare1: "",             // string, optional, maximum 256 characters
  spare2: "",             // string, optional, maximum 256 characters
  spare3: "",             // string, optional, maximum 256 characters
  latitude: "",           // float, optional
  longitude: "",          // float, optional
  notes: "",              // string, optional, maximum 500 characters
  employee_signature: {   // object, optional
    name: "",             // string, optional, filename
    base64: ""            // string, optional, Base64 data URI; use base64
  },
  items: [                // array, required, size must be less than or equal to 50
    {
      item_question: "",  // string, required, maximum 256 characters
      passed: "",         // boolean, required, true or false
      fail_reason: "",    // string, optional, maximum 256 characters
      images: [           // array, optional
        {
          name: "",       // string, optional, filename
          base64: ""      // string, optional, Base64 data URI; use base64
        }
      ]
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/premium/prestarts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: ******' \
--data-raw '{
  "fleet_number": "PM01",
  "completed_at": "2021-10-01 12:00:00",
  "employee": "John Smith",
  "employee_number": "7",
  "notes": "Some notes",
  "odometer": 5000,
  "hours": 1000,
  "employee_signature": {
    "name": "signature.png",
    "base64": "data:image/png;base64,<base64-encoded-content>"
  },
  "items": [
    {
      "item_question": "Check registration label is intact and current",
      "passed": true,
      "fail_reason": "",
      "images": [
        {
          "name": "registration-label.jpg",
          "base64": "data:image/jpeg;base64,<base64-encoded-content>"
        }
      ]
    },
    {
      "item_question": "Check all tyres for tread, damage & inflation - including spares",
      "passed": false,
      "fail_reason": "Damage to spare tyre"
    }
  ]
}'
```
