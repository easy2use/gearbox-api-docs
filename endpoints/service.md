# Services

## Create services

`POST /public/v1/service` creates new service with the provided information.

+ Content-Type header `Content-Type: application/json` is required when POSTing fault reports into Gearbox.
+ Root element `service` and snake case object keys are required.

### Request
```
URL: https://api.gearbox.com.au/public/v1/services
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

service: {
  fleet_number: “”, // string, must match existing fleet number in system, required
  ...
}]
```

### Response status codes:
 - 200: OK
 - 400: Bad Request
 - 401: Unauthorised
 - 403: Forbidden
 - 413: Request Entity Too Large
 - 415: Unsupported media type