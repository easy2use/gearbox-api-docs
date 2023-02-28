# Vehicles

- [Create vehicles](#create-vehicles)
- [Update a vehicle](#update-a-vehicle)
- [Create vehicle documents](#create-vehicle-documents)

## Create vehicles

`POST /public/v1/vehicles` creates a new vehicle with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing vehicles into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicles
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "",           // string, required, maximum length 12 characters, must be unique
  service_group: "",          // string, required, must match existing Service Group
  registration: "",           // string, optional, maximum length 12 characters
  registration_due_date: "",  // date, optional, format 'yyyy-mm-dd'
  unregistered: "",           // boolean, optional, defaults to false
  group: "",                  // string, optional, must match existing Vehicle Group
  sub_group: "",              // string, optional, must match existing Vehicle Group
  type: "",                   // string, optional, must match existing Vehicle Type
  make: "",                   // string, optional, must match existing Vehicle Make
  model: "",                  // string, optional, maximum length 50 characters
  configuration: "",          // string, optional, must match existing Vehicle Configuration
  vin: "",                    // string, optional, maximum length 50 characters
  build_date: "",             // string, optional, maximum length 50 characters
  spare1: "",                 // string, optional, maximum length 50 characters
  spare2: "",                 // string, optional, maximum length 50 characters
  spare3: "",                 // string, optional, maximum length 50 characters
  spare4: "",                 // string, optional, maximum length 50 characters
  spare5: "",                 // string, optional, maximum length 50 characters
  spare6: "",                 // string, optional, maximum length 50 characters
  spare7: "",                 // string, optional, maximum length 50 characters
  spare8: "",                 // string, optional, maximum length 50 characters
  spare9: "",                 // string, optional, maximum length 50 characters
  spare10: "",                // string, optional, maximum length 50 characters
  spare11: "",                // string, optional, maximum length 50 characters
  spare12: "",                // string, optional, maximum length 50 characters
  spare13: "",                // string, optional, maximum length 50 characters
  spare14: "",                // string, optional, maximum length 50 characters
  spare15: "",                // string, optional, maximum length 50 characters
  spare16: "",                // string, optional, maximum length 50 characters
  spare17: "",                // string, optional, maximum length 50 characters
  spare18: "",                // string, optional, maximum length 50 characters
  spare19: "",                // string, optional, maximum length 50 characters
  spare20: "",                // string, optional, maximum length 50 characters
  powered: "",                // boolean, optional, defaults to false
  engine_number: "",          // string, optional, maximum length 20 characters
  engine_make: "",            // string, optional, maximum length 50 characters
  engine_model: "",           // string, optional, maximum length 50 characters
  engine_capacity: "",        // string, optional, maximum length 50 characters
  gearbox: "",                // string, optional, maximum length 50 characters
  sold: "",                   // boolean, optional, defaults to false
  contractor: ""              // string, optional, must match existing Contractor
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
  "fleet_number": "ABC123"
}
```

###### Example

```
curl --location --request POST http://api.gearbox.com.au/public/v1/vehicles \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "fleet_number": "ABC123",
      "service_group": "Trucks",
      "registration": "123ABC",
      "registration_due_date": "2023-5-24",
      "unregistered": false,
      "vin": "1A2B3C4D5E6F",
      "build_date": "2018",
      "spare01": "1234",
      "spare02": "5678",
      "group": "SA",
      "sub_group": "Adelaide"
    }' 
```

## Update a vehicle

`PATCH /public/v1/vehicles/:id` updates the vehicle with the provided `:id`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing vehicles into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicles/1
Method: PATCH
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  fleet_number: "",           // string, optional, maximum length 12 characters, must be unique
  service_group: "",          // string, optional, must match existing Service Group
  registration: "",           // string, optional, maximum length 12 characters
  registration_due_date: "",  // date, optional, format 'yyyy-mm-dd'
  unregistered: "",           // boolean, optional, defaults to false
  group: "",                  // string, optional, must match existing Vehicle Group
  sub_group: "",              // string, optional, must match existing Vehicle Group
  type: "",                   // string, optional, must match existing Vehicle Type
  make: "",                   // string, optional, must match existing Vehicle Make
  model: "",                  // string, optional, maximum length 50 characters
  configuration: "",          // string, optional, must match existing Vehicle Configuration
  vin: "",                    // string, optional, maximum length 50 characters
  build_date: "",             // string, optional, maximum length 50 characters
  spare1: "",                 // string, optional, maximum length 50 characters
  spare2: "",                 // string, optional, maximum length 50 characters
  spare3: "",                 // string, optional, maximum length 50 characters
  spare4: "",                 // string, optional, maximum length 50 characters
  spare5: "",                 // string, optional, maximum length 50 characters
  spare6: "",                 // string, optional, maximum length 50 characters
  spare7: "",                 // string, optional, maximum length 50 characters
  spare8: "",                 // string, optional, maximum length 50 characters
  spare9: "",                 // string, optional, maximum length 50 characters
  spare10: "",                // string, optional, maximum length 50 characters
  spare11: "",                // string, optional, maximum length 50 characters
  spare12: "",                // string, optional, maximum length 50 characters
  spare13: "",                // string, optional, maximum length 50 characters
  spare14: "",                // string, optional, maximum length 50 characters
  spare15: "",                // string, optional, maximum length 50 characters
  spare16: "",                // string, optional, maximum length 50 characters
  spare17: "",                // string, optional, maximum length 50 characters
  spare18: "",                // string, optional, maximum length 50 characters
  spare19: "",                // string, optional, maximum length 50 characters
  spare20: "",                // string, optional, maximum length 50 characters
  powered: "",                // boolean, optional, defaults to false
  engine_number: "",          // string, optional, maximum length 20 characters
  engine_make: "",            // string, optional, maximum length 50 characters
  engine_model: "",           // string, optional, maximum length 50 characters
  engine_capacity: "",        // string, optional, maximum length 50 characters
  gearbox: "",                // string, optional, maximum length 50 characters
  sold: "",                   // boolean, optional, defaults to false
  contractor: ""              // string, optional, must match existing Contractor
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
  "fleet_number": "ABC123"
}
```

###### Example

```
curl --location --request PATCH http://api.gearbox.com.au/public/v1/vehicles/6245 \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "fleet_number": "ABC123",
      "service_group": "Trucks",
      "registration": "123ABC",
      "registration_due_date": "2023-05-24",
      "unregistered" :false,
      "vin": "1A2B3C4D5E6F",
      "build_date": "2018",
      "spare01": "1234",
      "spare02": "5678",
      "group": "SA",
      "sub_group": "Adelaide"
    }' 
```

## Create vehicle documents

`POST /public/v1/vehicle/:id/documents` creates a document attaching it to a provided vehicle `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicle/1/documents?name=image.png
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/vehicle/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```
