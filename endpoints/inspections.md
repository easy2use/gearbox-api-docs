# Inspections

- [Create inspections](#create-inspections)
- [Create inspection documents](#create-inspection-documents)

## Create inspections

`POST /public/v1/inspections` creates a new inspection with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing inspections into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/inspections
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "",        // string, required if registration is blank, must match existing fleet number in the system
  registration: "",        // string, required if fleet_number is blank, must match existing registration in the system
  inspection_type: "",     // string, required, if a match is not found then an error is thrown
  date_open: "",           // date, required, format: 'yyyy-mm-dd'
  odometer_scheduled: "",  // integer, optional
  hours_scheduled: "",     // integer, optional
  jobcard_notes: "",       // string, optional, maximum 500 characters
  repairer_notes: "",      // string, optional, maximum 500 characters
  closed: "",              // boolean, required, true or false
  date_closed: "",         // date, required if closed is true, format: 'yyyy-mm-dd'
  hours_closed: "",        // integer, optional
  odometer_closed: "",     // integer, optional
  cost: "",                // float, optional, format: 100.00
  invoice: "",             // string, optional, maximum 45 characters
  invoice_date: "",        // date, optional, format: 'yyyy-mm-dd'
  location: "",            // string, optional, maximum 50 characters
  date_scheduled: "",      // datetime, optional, format: yyyy-mm-dd hh:mm
  date_scheduled_end: "",  // datetime, optional, format: yyyy-mm-dd hh:mm
  site: "",                // string, optional, if a site match is not found than an error is thrown
  general_ledger_code: "", // string, optional, if a general ledger code match is not found than an error is thrown
  repairer: ""             // string, optional, if a repairers name or code match is not found than an error is thrown
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/inspections' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    fleet_number: "PM01",
    registration: "ABC123",
    inspection_type: "Annual Inspection",
    date_open: "2021-01-01",
    odometer_scheduled: 1000,
    hours_scheduled: 10,
    jobcard_notes: "Perform annual inspection per checklist",
    repairer_notes: "Minor chassis wear discovered",
    closed: false,
    date_closed: "2021-01-01",
    hours_closed: 10,
    odometer_closed: 1000,
    cost: 100.0,
    invoice: "Invoice #123",
    invoice_date: "2021-01-01",
    location: "Sydney",
    date_scheduled: "2021-01-01",
    date_scheduled_end: "2021-01-01",
    site: "Sydney",
    general_ledger_code: "5000-05",
    repairer: "Inhouse mechanic",
}'
```

## Create inspection documents

`POST /public/v1/inspection/:id/documents` creates a document attaching it to a provided inspection `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/inspection/1/documents?name=image.png
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/inspection/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```
