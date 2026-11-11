# Premium Fault Reports

- [Create premium fault reports](#create-premium-fault-reports)

## Create premium fault reports

`POST /public/v1/premium/fault_reports` creates a fault report with the provided information and supports signature and image attachments.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing fault reports into Gearbox.
- Snake case object keys are required.
- The OAuth application must have Premium API and fault report access enabled.
- Attachment `base64` values should be Base64 data URIs.

### Request

```
URL: https://api.gearbox.com.au/public/v1/premium/fault_reports
Method: POST
Content-Type: application/json
Authorization: $ACCESS_TOKEN

{
  fleet_number: "",       // string, required, must match existing fleet number in system
  created_at: "",         // datetime, required, format: `yyyy-mm-dd hh:mm:ss +0000` - if timezone is not included it will be inferred from the businesses timezone
  employee: "",           // string, required, if a match is not found then it is stored as a string
  employee_number: "",    // integer, optional, if there are multiple employees with the same name you may use this field to specify your search
  fail_reason: "",        // string, optional, maximum 256 characters
  notes: "",              // string, optional, maximum 500 characters
  odometer_open: "",      // integer, optional
  hours_open: "",         // integer, optional
  signature: "",          // string, optional, deprecated Base64-encoded signature
  employee_signature: {   // object, optional
    name: "",             // string, optional, filename
    base64: ""            // string, optional, Base64 data URI; use base64
  },
  images: [               // array, optional
    {
      name: "",           // string, optional, filename
      base64: ""          // string, optional, Base64 data URI; use base64
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/premium/fault_reports' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: ******' \
--data-raw '{
  "fleet_number": "PM01",
  "created_at": "2021-10-01 12:00:00",
  "employee": "John Smith",
  "employee_number": 7,
  "fail_reason": "Broken",
  "notes": "Some notes",
  "odometer_open": 5000,
  "hours_open": 1000,
  "employee_signature": {
    "name": "signature.png",
    "base64": "data:image/png;base64,<base64-encoded-content>"
  },
  "images": [
    {
      "name": "fault.jpg",
      "base64": "data:image/jpeg;base64,<base64-encoded-content>"
    }
  ]
}'
```