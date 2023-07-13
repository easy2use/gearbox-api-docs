# Vehicle Groups

- [Get vehicle groups](#get-vehicle-groups)
- [Create vehicle groups](#create-vehicle-groups)
- [Update a vehicle group](#update-a-vehicle-group)

## Get vehicle groups

`GET /public/v1/vehicle_groups` retrieves a [paginated list](../readme.md/#pagination) of vehicle groups.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>
  
- group_name
- tax_number
- address1
- address2
- city
- state
- postcode
- contact
- parts_used_include_tax
- invoice_header
- invoice_footer
- purchase_order_header
- purchase_order_footer
- default_repairer
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicle_groups
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "vehicle_groups": [
    {
      "id": 774935874, 
      "group_name": "Gearbox",
      "tax_number": "ABC123456",
      "address1": "1 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "contact": "John Smith",
      "parts_used_include_tax": true,
      "invoice_header": "Easy2use",
      "invoice_footer": "PO Box 500",
      "purchase_order_header": "Easy2use",
      "purchase_order_footer": "PO Box 500",
      "default_repairer": "Workshop"
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/vehicle_groups' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create vehicle groups

`POST /public/v1/vehicle_groups` creates a new vehicle group with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing vehicle groups into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicle_groups
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  group_name: "",              // string, required, maximum length 100 characters
  tax_number: "",              // string, optional, maximum length 255 characters
  address1: "",                // string, optional, maximum length 255 characters
  address2: "",                // string, optional, maximum length 255 characters
  city: "",                    // string, optional, maximum length 255 characters
  state: "",                   // string, optional, maximum length 255 characters
  postcode: "",                // string, optional, maximum length 255 characters
  contact: "",                 // string, optional, maximum length 255 characters
  parts_used_include_tax: "",  // boolean, optional, defaults to false
  invoice_header: "",          // string, optional, maximum length 1000 characters
  invoice_footer: "",          // string, optional, maximum length 1000 characters
  purchase_order_header: "",   // string, optional, maximum length 1000 characters
  purchase_order_footer: "",   // string, optional, maximum length 1000 characters
  default_repairer: ""         // string, optional, must match an existing Repairer
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
curl --location --request POST http://api.gearbox.com.au/public/v1/vehicle_groups \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "group_name": "Gearbox",
      "tax_number": "ABC123456",
      "address1": "1 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "contact": "John Smith",
      "parts_used_include_tax": true,
      "invoice_header": "Easy2use",
      "invoice_footer": "PO Box 500",
      "purchase_order_header": "Easy2use",
      "purchase_order_footer": "PO Box 500",
      "default_repairer": "Workshop"
    }' 
```

## Update a vehicle group

`PATCH /public/v1/vehicle_groups/:id` updates the vehicle group with the provided `:id`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing vehicle groups into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicle_groups/1
Method: PATCH
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  group_name: "",              // string, required, maximum length 100 characters
  tax_number: "",              // string, optional, maximum length 255 characters
  address1: "",                // string, optional, maximum length 255 characters
  address2: "",                // string, optional, maximum length 255 characters
  city: "",                    // string, optional, maximum length 255 characters
  state: "",                   // string, optional, maximum length 255 characters
  postcode: "",                // string, optional, maximum length 255 characters
  contact: "",                 // string, optional, maximum length 255 characters
  parts_used_include_tax: "",  // boolean, optional, defaults to false
  invoice_header: "",          // string, optional, maximum length 1000 characters
  invoice_footer: "",          // string, optional, maximum length 1000 characters
  purchase_order_header: "",   // string, optional, maximum length 1000 characters
  purchase_order_footer: "",   // string, optional, maximum length 1000 characters
  default_repairer: ""         // string, optional, must match an existing Repairer
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
  "id": 123
}
```

###### Example

```
curl --location --request PATCH http://api.gearbox.com.au/public/v1/vehicle_groups/6245 \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "group_name": "Gearbox",
      "tax_number": "ABC123456",
      "address1": "1 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "contact": "John Smith",
      "parts_used_include_tax": true,
      "invoice_header": "Easy2use",
      "invoice_footer": "PO Box 500",
      "purchase_order_header": "Easy2use",
      "purchase_order_footer": "PO Box 500",
      "default_repairer": "Workshop"
    }' 
```
