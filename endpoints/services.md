# Services

- [Create services](#Create services)
- [Create service documents](#Create service documents)

## Create services

`POST /public/v1/service` creates a new service with the provided information.

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
  fleet_number: "", // string, must match existing fleet number in system
  registration: "", // string, must match existing registration in system
  service_type: "", // character, required, values allowed: A, B, C, D, E
  date_open: "", // date, required, format: 'yyyy-mm-dd'
  odometer_scheduled: "", // integer, not required
  hours_scheduled: "", // integer, not required
  notes: "", // string, maximum 500 characters, not required
  repairer_notes: "", // string, maximum 500 characters, not required
  closed: "", // boolean, required
  date_closed: "", // date, required if closed, format: 'yyyy-mm-dd'
  hours_closed: "", // integer, not required
  odometer_closed: "" // integer, not required
  cost: "",
  purchase_order: "", // string, maximum 20 characters, not required
  invoice: "", // string, maximum 45 characters, not required
  invoice_date: "", // date, optional, format: 'yyyy-mm-dd'
  location: "", // string, maximum 50 characters, not required
  date_scheduled: "", // datetime, optional, format: yyyy-mm-dd hh:mm:ss
  date_scheduled_end: "", // datetime, optional, format: yyyy-mm-dd hh:mm:ss
  site: "", // string, optional
  general_ledger_code: "", // string, optional
  parts: [{
    part_number: "", // string, optional
    quantity: "",  // string, optional
    each: "" // string, optional
  }],
  repairers: [{
    name: "", // string, optional
    code: "" // string, optional
  }]
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

## Create service documents

`POST /public/v1/service/:id/documents` creates a document attaching it to a provided service `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.

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

###### Example

```
curl --location --request POST 'https://api.gearbox.com.au/public/v1/service/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```