# Repairers

- [Get repairers](#get-repairers)
- [Create repairers](#create-repairers)
- [Update a repairer](#update-a-repairer)


## Get repairers

`GET /public/v1/repairers` retrieves a [paginated list](../readme.md/#pagination) of repairers.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>

- id
- name
- address1
- address2
- city
- state
- postcode
- contact
- phone
- fax
- mobile
- business_number
- subcontractor
- notes
- archived
- repairer_only
- supplier_only
- group
- approved
- code
- emails

`emails` supports searching a comma-separated list of emails, e.g. `emails eq test@test.com, example@example.com`

`emails` only supports the `eq` operator, an error will be returned if another operator is used with this field
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/repairers
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "repairers": [
    {
      "id": 123,
      "name": "John's Workshop",
      "address1": "123 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "contact": "John",
      "phone": "+614123456",
      "fax": "112233",
      "mobile": "040123456",
      "business_number": "ABN123",
      "subcontractor": true,
      "notes": "Entrance to workshop located around back",
      "archived": false,
      "repairer_only": false,
      "supplier_only": false,
      "group": "Sydney",
      "approved": true,
      "code": "ABC123",
      "emails": ["john@workshop.com.au", "admin@workshop.com.au"]
    }
  ]
}
```

###### Examples

```
curl --location --request GET "https://api.gearbox.com.au/public/v1/repairers" \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create repairers

`POST /public/v1/repairers` creates a new repairer with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing repairers into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/repairers
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  name: "",             // string, required, maximum length 100 characters
  address1: "",         // string, optional, maximum length 45 characters
  address2: "",         // string, optional, maximum length 45 characters
  city: "",             // string, optional, maximum length 50 characters
  state: "",            // string, optional, maximum length 50 characters
  postcode: "",         // string, optional, maximum length 100 characters
  contact: "",          // string, optional, maximum length 50 characters
  phone: "",            // string, optional, maximum length 50 characters
  fax: "",              // string, optional, maximum length 50 characters
  mobile: "",           // string, optional, maximum length 50 characters
  business_number: "",  // string, optional, maximum length 50 characters
  subcontractor: "",    // boolean, optional, defaults to false
  notes: "",            // string, optional, maximum length 255 characters
  archived: "",         // boolean, optional, defaults to false
  repairer_only: "",    // boolean, optional, defaults to false
  supplier_only: "",    // boolean, optional, defaults to false
  group: "",            // string, optional, must match existing Vehicle Group
  approved: "",         // boolean, optional, defaults to false
  code: "",             // string, optional, maximum length 50 characters
  emails: ""            // array, optional, comma seperated list of valid emails
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
  "code": "abc123"
}
```

###### Example

```
curl --location --request POST http://api.gearbox.com.au/public/v1/repairers \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "name": "John's Workshop",
      "address1": "123 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "contact": "John",
      "phone": "+614123456",
      "fax": "112233",
      "mobile": "040123456",
      "business_number": "ABN123",
      "subcontractor": true,
      "notes": "Entrance to workshop located around back",
      "archived": false,
      "repairer_only": false,
      "supplier_only": false,
      "group": "Sydney",
      "approved": true,
      "code": "ABC123",
      "emails": ["john@workshop.com.au", "admin@workshop.com.au"]
    }' 
```

## Update a repairer

`PATCH /public/v1/repairers/:id` updates the repairer with the provided `:id`. Alternatively, you may instead provide a repairer `:code` in the format of: `PATCH /public/v1/repairers/:code`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing repairers into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/repairers/
Method: PATCH
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  name: "",             // string, required, maximum length 100 characters
  address1: "",         // string, optional, maximum length 45 characters
  address2: "",         // string, optional, maximum length 45 characters
  city: "",             // string, optional, maximum length 50 characters
  state: "",            // string, optional, maximum length 50 characters
  postcode: "",         // string, optional, maximum length 100 characters
  contact: "",          // string, optional, maximum length 50 characters
  phone: "",            // string, optional, maximum length 50 characters
  fax: "",              // string, optional, maximum length 50 characters
  mobile: "",           // string, optional, maximum length 50 characters
  business_number: "",  // string, optional, maximum length 50 characters
  subcontractor: "",    // boolean, optional, defaults to false
  notes: "",            // string, optional, maximum length 255 characters
  archived: "",         // boolean, optional, defaults to false
  repairer_only: "",    // boolean, optional, defaults to false
  supplier_only: "",    // boolean, optional, defaults to false
  group: "",            // string, optional, must match existing Vehicle Group
  approved: "",         // boolean, optional, defaults to false
  code: "",             // string, optional, maximum length 50 characters
  emails: ""            // array, optional, comma seperated list of valid emails
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
  "code": "ABC123"
}
```

###### Example

```
curl --location --request PATCH http://api.gearbox.com.au/public/v1/repairers/123 \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "name": "John's Workshop",
      "address1": "123 Peter Brock Drive",
      "address2": "PO Box 500",
      "city": "Oran Park",
      "state": "NSW",
      "postcode": "2570",
      "contact": "John",
      "phone": "+614123456",
      "fax": "112233",
      "mobile": "040123456",
      "business_number": "ABN123",
      "subcontractor": true,
      "notes": "Entrance to workshop located around back",
      "archived": false,
      "repairer_only": false,
      "supplier_only": false,
      "group": "Sydney",
      "approved": true,
      "code": "ABC123",
      "emails": ["john@workshop.com.au", "admin@workshop.com.au"]
    }' 
```
