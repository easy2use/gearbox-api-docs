# Repairs

- [Get repairs](#get-repairs)
- [Create repairs](#create-repairs)
- [Update repairs](#update-repairs)
- [Create repair documents](#create-repair-documents)

## Get repairs

`GET /public/v1/repairs` retrieves a [paginated list](../readme.md/#pagination) of repairs.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>

- id
- vehicle
- fault
- fleet_number
- registration
- repair_number
- reported_to
- reported_by
- date_reported
- job_number
- defect
- defect_cleared_by_date
- defect_cleared_by
- odometer_open
- hours_open
- purchase_order
- location
- created_at
- updated_at
- site
- repair_items.repair_types.type
- repair_items.problem
- repair_items.notes
- repair_items.repairer_notes
- repair_items.invoice
- repair_items.cost
- repair_items.tax
- repair_items.date_closed
- repair_items.odometer_closed
- repair_items.hours_closed
- repair_items.status
- repair_items.jobcard_item_status
- repair_items.priority
- repair_items.repairers.name
- repair_items.repairers.code
- repair_items.assigned_user
- repair_items.general_ledger_code
- repair_items.parts.part_number
- repair_items.parts.quantity
- repair_items.parts.each
- repair_items.parts.total
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/repairs
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "repairs":
  [
    {
      "id": "10654825",
      "repair_number": 1,
      "date_reported": "2023-05-31",
      "fault_number": "F-123",
      "fleet_number": "CBO97M",
      "registration": "123ABC",
      "reported_to": "John Smith",
      "reported_by": "Ned Kelly",
      "location": "Townsville",
      "job_number": "12345",
      "defect": "ABC123",
      "defect_cleared_by_date": "2023-05-17",
      "defect_cleared_by": "AAP",
      "odometer_open": 500,
      "hours_open": 500,
      "created_at": "2023-06-01T09:50:56.154+10:00",
      "updated_at": "2023-06-01T10:15:12.154+10:00",
      "purchase_order": "PO-123",
      "site": "Example Site",
      "linked_fault_reports": "F-123, F-124",
      "repair_items":
      [
        {
          "id": 123,
          "tax": 10.0,
          "cost": 100.0,
          "notes": "example notes",
          "status": "open",
          "invoice": "NAI14943509",
          "problem": "example repair item problem",
          "priority": "important",
          "repairers": [
                          {
                            "name": "Jane Doe",
                            "code":"ABC123"
                          }
                        ],
          "assigned_users": [
                               {
                                 "user": "John Smith"
                               }
                             ],
          "invoice_date": "2023-06-01",
          "date_closed": "2023-06-01",
          "hours_closed": 1000,
          "jobcard_item_status": "Complete",
          "repair_types": [
                            {
                              "type": "Broken Valve"
                            }
                          ],
          "repairer_notes": "example notes",
          "odometer_closed": 1000,
          "parts": [
                      {
                        "each": 100.0,
                        "total": 1000.0,
                        "quantity": 1.0,
                        "part_number": "Example part"
                      }
                    ],
          "general_ledger_code": "7239 - GL code example"
        }
      ]
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/repairs' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create repairs

`POST /public/v1/repairs` creates a new repair with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing repairs into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/repairs
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
  repair_items: [               // array, required, size must be less than or equal to 5
    {
      repair_types: [           // array, required, size must be less than or equal to 5
        {
          type_label: "",     // string, required, maximum 45 characters, will create a new repair type if a match is not found
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
      date_scheduled: "",     // datetime, optional, format: `yyyy-mm-dd hh:mm:ss +0000` - if timezone is not included it will be inferred from the businesses timezone
      date_scheduled_end: "", // datetime, optional, format: `yyyy-mm-dd hh:mm:ss +0000` - if timezone is not included it will be inferred from the businesses timezone
      general_ledger_code: "",// string, optional, if a general ledger code match is not found than an error is thrown
      parts: [                // array, optional, size must be less than or equal to 5
        {
          part_number: "",    // string, required, if a part match is not found than an error is thrown
          quantity: "",       // float, optional, format: 1.00
          each: ""            // float, optional, format: 5.50
        }
      ],
      repairers: [            // array, optional, size must be less than or equal to 5
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
  "repair_item_ids": [123, 124]
}
```

###### Example

```
curl --location --request POST 'https://api.gearbox.com.au/public/v1/repairs' \
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
    "repair_items": [
      {
        "repair_types": [
          {
            "type_label": "Turbo"
          }
        ],
        "problem": "Blown turbo",
        "cause": "Lack of maintenance",
        "solution": "Replace turbo",
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
            "part_number": "New turbo",
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

## Update repairs

`PATCH /public/v1/repairs/:id` updates the repair with the provided `:id`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing repairs into Gearbox.
- Snake case object keys are required.
- All fields are optional; fields not provided are left unchanged.
- Providing a repair item with an `id` updates that item. Providing one without an `id` creates a new repair item.
- If `repair_items` is provided, it must contain between 1 and 5 items.
- New repair items require `status` and at least one `repair_types` entry. Existing repair items can omit unchanged fields.
- Providing an empty `assigned_users` array clears the repair item's assignments.

### Request

```
URL: https://api.gearbox.com.au/public/v1/repairs/123
Method: PATCH
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "",                 // string, optional, must match an existing fleet number
  registration: "",                 // string, optional, must match an existing registration
  date_reported: "",                // date, optional, format: 'yyyy-mm-dd'
  odometer_open: "",                // integer, optional
  hours_open: "",                   // integer, optional
  fault_number: "",                 // string, optional
  defect_number: "",                // string, optional
  purchase_order: "",               // string, optional
  job_number: "",                   // string, optional
  defect_cleared_by_date: "",       // date, optional, format: 'yyyy-mm-dd'
  defect_cleared_by: "",            // string, optional, must match an existing repairer
  reported_to: "",                  // string, optional, must match an existing employee
  reported_to_employee_number: "",  // integer, optional, identifies the employee when names are not unique
  reported_by: "",                  // string, optional, must match an existing employee
  reported_by_employee_number: "",  // integer, optional, identifies the employee when names are not unique
  location: "",                     // string, optional
  repair_items: [                   // array, optional, 1 to 5 items when provided
    {
      id: 123,                      // integer, optional; updates this item if present, otherwise creates an item
      problem: "",                  // string, optional, maximum 500 characters
      cause: "",                    // string, optional, maximum 500 characters
      solution: "",                 // string, optional, maximum 500 characters
      status: "",                   // string, required for new items; open, monitor, deferred, closed, cancelled
      date_closed: "",              // date, required when status is closed, format: 'yyyy-mm-dd'
      hours_closed: "",             // integer, optional
      odometer_closed: "",          // integer, optional
      cost: "",                     // float, optional, format: 100.00
      invoice: "",                  // string, optional, maximum 45 characters
      invoice_date: "",             // date, optional, format: 'yyyy-mm-dd'
      date_scheduled: "",           // datetime, optional, format: `yyyy-mm-dd hh:mm:ss +0000`
      date_scheduled_end: "",       // datetime, optional, format: `yyyy-mm-dd hh:mm:ss +0000`
      general_ledger_code: "",      // string, optional, must match an existing general ledger code
      repairer_notes: "",           // string, optional
      jobcard_item_status: "",      // string, optional; Unanswered, N/A, Incomplete, Complete, Advisory
      priority: "",                 // string, optional; normal, important, urgent
      assigned_users: [             // array, optional; an empty array clears assignments
        {
          name: ""                  // string, required, must match an existing user's name
        }
      ],
      repair_types: [               // array, required for new items, up to 5 entries
        {
          type_label: ""            // string, required; creates a repair type if no match is found
        }
      ],
      parts: [                      // array, optional, up to 5 entries
        {
          part_number: "",          // string, required, must match an existing part
          quantity: "",             // float, optional, format: 1.00
          each: ""                  // float, optional, format: 5.50
        }
      ],
      repairers: [                  // array, optional, up to 5 entries
        {
          name: "",                 // string, required if code is blank, must match an existing repairer
          code: ""                  // string, required if name is blank, must match an existing repairer
        }
      ]
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
  "id": 123,
  "repair_item_ids": [123, 124]
}
```

###### Example

```
curl --location --request PATCH 'https://api.gearbox.com.au/public/v1/repairs/123' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
  "purchase_order": "PO-123",
  "location": "Workshop A",
  "repair_items": [
    {
      "id": 456,
      "problem": "Updated repair item problem"
    }
  ]
}'
```

## Create repair documents

`POST /public/v1/repair_item/:id/documents` creates a document attaching it to a provided repair item `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/repair_item/1/documents?name=image.png
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/repair_item/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```
