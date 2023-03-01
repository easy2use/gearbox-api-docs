# Fault Reports

## Create fault reports

`POST /public/v1/fault_reports` creates a new fault report with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing fault reports into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/fault_reports
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: “”,     // string, required, must match existing fleet number in system
  created_at: “”,       // datetime, required, format: yyyy-mm-dd hh:mm:ss
  employee: “”,         // string, required, if a match is not found then it is stored as a string
  employee_number: “”,  // string, optional, if there are multiple employees with the same name you may use this field to specify your search
  fail_reason: “”,      // string, optional, maximum 256 characters
  notes: “”             // string, optional, maximum 500 characters
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/fault_reports' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
	"fleet_number": "PM01",
	"created_at": "2021-10-01 12:00:00",
	"employee": "John Smith",
  "employee_number": "7",
	"fail_reason": "Broken",
	"notes": "Some notes"
}'
```
