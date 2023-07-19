# Fault Reports

- [Get fault reports](#get-fault-reports)
- [Create fault reports](#create-fault-reports)
- [Update a fault report](#update-a-fault-report)

## Get fault reports

`GET /public/v1/fault_reports` retrieves a [paginated list](../readme.md/#pagination) of fault reports.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>
  
- id
- fleet_number
- created_at
- notes
- employee
- fail_reason
- archived
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/fault_reports
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "fault_reports": [
    {
      "id": 774935874, 
      "fleet_number": "FLT123", 
      "created_at": "2023-06-21T09:01:09.276+10:00", 
      "notes": "Found oil pooled below asset",
      "employee": "John Smith", 
      "fail_reason": "Oil leaking", 
      "archived": false
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/fault_reports' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

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

## Update a fault report

`PATCH /public/v1/fault_reports/:id` updates the fault report with the provided `:id`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing fault reports into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/fault_reports/1
Method: PATCH
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
  "id": 6245
}
```

###### Example

```
curl --location --request PATCH http://api.gearbox.com.au/public/v1/fault_reports/6245 \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
	"fleet_number": "PM01",
	"created_at": "2021-10-01 12:00:00",
	"employee": "John Smith",
  "employee_number": "7",
	"fail_reason": "Broken",
	"notes": "Some notes"
}'
```
