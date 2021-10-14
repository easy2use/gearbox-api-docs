# Fault Reports

## Create fault reports

`POST /public/v1/fault_reports` creates new fault reports with the provided information.

+ Content-Type header `Content-Type: application/json` is required when POSTing fault reports into Gearbox.
+ Root element `fault_reports` and snake case object keys are required.

### Request
```
URL: https://api.gearbox.com.au/public/v1/fault_reports
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

fault_reports: [{
  fleet_number: “”, // string, must match existing fleet number in system, required
  created_at: “”, // datetime, required, format: yyyy-mm-dd hh:mm:ss
  employee: “”, // string, required. If a match is not found then it is stored as a string
  fail_reason: “”, // string, maximum 256 characters, not required
  notes: “” // string, maximum 500 characters, not required
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/fault_reports' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
	"fault_reports": [
		{
			"fleet_number": "PM01",
			"created_at": "2021-10-01 12:00:00",
			"employee": "John Smith",
			"fail_reason": "Broken",
			"notes": "Some notes"
		}
	]
}'
```