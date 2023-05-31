# Services

- [Get services](#get-services)
- [Create services](#create-services)
- [Create service documents](#create-service-documents)

## Get services

`GET /public/v1/services` retrieves a [paginated list](../readme.md/#pagination) of services.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>
  
- id
- fleet_number
- registration
- service_type
- service_number
- date_open
- odometer_scheduled
- hours_scheduled
- jobcard_notes
- repairer_notes
- closed
- date_closed
- hours_closed
- odometer_closed
- tax
- cost
- purchase_order
- invoice
- invoice_date
- location
- date_scheduled
- date_scheduled_end
- created_at
- site
- general_ledger_code
- parts.part_number
- parts.quantity
- parts.each
- parts.total
- repairers.name
- repairers.code
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/services
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "services": [
    {
      "fleet_number": "PM05",
      "registration": "PM005",
      "service_type": "B",
      "service_type_alias": "Oil and Filters",
      "service_number": 123,
      "fault_num": "FB456",
      "date_open": "2020-01-14",
      "odometer_scheduled": 1000000,
      "hours_scheduled": 100,
      "jobcard_notes": "Carry out B Service",
      "repairer_notes": "Performed B Service",
      "closed": false,
      "date_closed": "2020-01-14",
      "hours_closed": 100,
      "odometer_closed": 1000000,
      "tolerance_kms": 123,
      "tolerance_hours": 6,
      "tolerance_days": 123,
      "interval_kms": 123,
      "interval_hours": 123,
      "interval_days": 123,
      "actual_interval_kms": 123,
      "actual_interval_hours": 6,
      "actual_interval_days": 123,
      "kms_ontime_status_id": "ontime",
      "hours_ontime_status_id": "overdue",
      "days_ontime_status_id": "no_prior_service",
      "cost": 55.0,
      "purchase_order": "ABC123",
      "invoice": "ABC123",
      "invoice_date": "2020-01-14",
      "location": "Sydney Workshop",
      "date_scheduled": "2020-01-22T06:00:00.000+11:00",
      "date_scheduled_end": "2020-01-22T07:30:00.000+11:00",
      "created_at": "2020-01-22T07:30:00.000+11:00",
      "site": "Sydney",
      "general_ledger_code": "ABC123 - Maintenance",
      "parts": [
        {
            "each": 55.0,
            "total": 55.0,
            "quantity": 1.0,
            "part_number": "DD123"
        }
      ],
      "repairers": [
        {
            "code": "ABC123",
            "name": "Gearbox"
        }
      ]
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/services' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create services

`POST /public/v1/services` creates a new service with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing services into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/services
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "",         // string, required if registration is blank, must match existing fleet number in the system
  registration: "",         // string, required if fleet_number is blank, must match existing registration in the system
  service_type: "",         // character, required, values allowed: A, B, C, D, E
  date_open: "",            // date, required, format: 'yyyy-mm-dd'
  odometer_scheduled: "",   // integer, optional
  hours_scheduled: "",      // integer, optional
  jobcard_notes: "",        // string, optional, maximum 500 characters
  repairer_notes: "",       // string, optional, maximum 500 characters
  closed: "",               // boolean, required, true or false
  date_closed: "",          // date, required if closed is true, format: 'yyyy-mm-dd'
  hours_closed: "",         // integer, optional
  odometer_closed: ""       // integer, optional
  cost: "",                 // float, optional, format: 100.00
  purchase_order: "",       // string, optional, maximum 20 characters
  invoice: "",              // string, optional, maximum 45 characters
  invoice_date: "",         // date, optional, format: 'yyyy-mm-dd'
  location: "",             // string, optional, maximum 50 characters
  date_scheduled: "",       // datetime, optional, format: yyyy-mm-dd hh:mm
  date_scheduled_end: "",   // datetime, optional, format: yyyy-mm-dd hh:mm
  site: "",                 // string, optional, if a site match is not found than an error is thrown
  general_ledger_code: "",  // string, optional, if a general ledger code match is not found than an error is thrown
  parts: [                  // array, optional, size must be less than or equal to 5
    {
      part_number: "",      // string, required, if a part match is not found than an error is thrown
      quantity: "",         // float, optional, format: 1.00
      each: ""              // float, optional, format: 5.50
    }
  ],
  repairers: [              // array, optional, size must be less than or equal to 5
    {
      name: "",             // string, required if code is blank, must match existing repairers name in the system
      code: ""              // string, required if name is blank, must match existing repairers code in the system
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/services' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    "fleet_number": "PM01",
    "registration": "ABC123",
    "service_type": "A",
    "date_open": "2021-01-01",
    "odometer_scheduled": 100,
    "hours_scheduled": 150,
    "jobcard_notes": "Windscreen wipers are loose",
    "repairer_notes": "Replaced the windscreen wipers",
    "closed": false,
    "date_closed": "",
    "hours_closed": "",
    "odometer_closed": "",
    "cost": 0.00,
    "purchase_order": "Purchase order #123",
    "invoice": "Invoice #123",
    "invoice_date": "2021-01-02",
    "location": "Sydney",
    "date_scheduled": "2021-01-02 13:00",
    "date_scheduled_end": "2021-01-02 15:00",
    "site": "Sydney",
    "general_ledger_code": "5000-05",
    "parts": [
        {
            "part_number": "Windscreen wiper",
            "quantity": 1,
            "each": 11.50
        }
    ],
    "repairers": [
        {
            "name": "Inhouse mechanic",
            "code": ""
        }
    ]
}'
```

## Create service documents

`POST /public/v1/service/:id/documents` creates a document attaching it to a provided service `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/service/1/documents?name=image.png
Method: POST
Content-Type: image/png
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
  "id": 123
}
```

###### Example

```
curl --location --request POST 'https://api.gearbox.com.au/public/v1/service/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```
