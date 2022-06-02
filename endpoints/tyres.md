# Tyres

- [Create tyres](#create-tyres)
- [Create tyre documents](#create-tyre-documents)

## Create tyres

`POST /public/v1/tyres` creates a new tyre with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing tyres into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/tyres
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "",           // string, required if registration is blank, must match existing fleet number in the system
  registration: "",           // string, required if fleet_number is blank, must match existing registration in the system
  date_reported: "",          // date, required, format: 'yyyy-mm-dd'
  odometer_open: "",          // integer, optional
  hours_open: "",             // integer, optional
  fault_number: "",           // string, optional
  defect_number: "",          // string, optional
  purchase_order: "",         // string, optional, maximum 20 characters
  job_number: "",             // string, optional, maximum 20 characters
  tyre_items: [               // array, required, size must be less than or equal to 5
    {
      tyre_types: [           // array, required, size must be less than or equal to 5
        {
          type_label: "",     // string, required, maximum 45 characters, will create a new tyre type if a match is not found
        }
      ]
      problem: "",            // string, optional, maximum 500 characters
      cause: "",              // string, optional, maximum 500 characters
      solution: "",           // string, optional, maximum 500 characters
      status: "",             // string, required, must match open, monitor, deferred, closed, cancelled
      date_closed: "",        // date, required if status is closed, format: 'yyyy-mm-dd'
      hours_closed: "",       // integer, optional
      odometer_closed: "",    // integer, optional
      cost: "",               // float, optional, format: 100.00
      invoice: "",            // string, optional, maximum 45 characters
      invoice_date: "",       // date, optional, format: 'yyyy-mm-dd'
      date_scheduled: "",     // datetime, optional, format: yyyy-mm-dd hh:mm
      date_scheduled_end: "", // datetime, optional, format: yyyy-mm-dd hh:mm
      general_ledger_code: "",// string, optional, if a general ledger code match is not found than an error is thrown
      parts: [                // array, optional, size must be less than or equal to 5
        {
          part_number: "",    // string, required, if a part match is not found than an error is thrown
          quantity: "",       // float, optional, format: 1.00
          each: ""            // float, optional, format: 5.50
        }
      ],
      repairers: [            // array, optional, size must be less than or equal to 10
        {
          name: "",           // string, required if code is blank, must match existing repairers name in the system
          code: ""            // string, required if name is blank, must match existing repairers code in the system
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
  "id": 123,
  "tyre_item_ids": [123, 124]
}
```

###### Example

```
curl --location --request POST 'https://api.gearbox.com.au/public/v1/tyres' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    "fleet_number": "PM01",
    "registration": "ABC123",
    "date_reported": "2021-01-01",
    "odometer_open": "1000",
    "hours_open": "10",
    "fault_number": "F#123",
    "defect_number": "D#2",
    "purchase_order": "PO#2",
    "job_number": "J#2",
    "tyre_items": [
      {
        "tyre_types": [
          {
            "type_label": "Puncture"
          }
        ]
        "problem": "Puncture",
        "cause": "Nail in tyre",
        "solution": "Replace tyre",
        "status": "open",
        "date_closed": "",
        "hours_closed": "",
        "odometer_closed": "",
        "cost": 0.00,
        "invoice": "INV #123",
        "invoice_date": "2021-01-02",
        "date_scheduled": "2021-01-02 13:00"",
        "date_scheduled_end": "2021-01-02 15:00",
        "general_ledger_code": "5000-05",
        "parts": [
          {
            "part_number": "New tyre",
            "quantity": 1,
            "each": 100.50
          }
        ],
        "repairers": [
          {
            "name": "Inhouse mechanic",
            "code": ""
          }
        ]
      }
    ]
}'
```

## Create tyre documents

`POST /public/v1/tyre_item/:id/documents` creates a document attaching it to a provided tyre item `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/tyre_item/1/documents?name=image.png
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/tyre_item/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```