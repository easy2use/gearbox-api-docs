# Medicals

- [Get medicals](#get-medicals)
- [Create medicals](#create-medicals)

## Get medicals

`GET /public/v1/medicals` retrieves a [paginated list](../readme.md/#pagination) of medicals.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attribute names</summary>
<br>

- id
- closed
- expiry_date
- medical_number
- employee
- medical_date
- medical_type
- supplier
- passed
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/medicals
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "medicals": [
    {
      "id": 5678,
      "medical_number": 321,
      "employee": "John Smith",
      "medical_type": "Standard Medical",
      "medical_date": "2026-01-01",
      "expiry_date": "2026-12-01",
      "supplier": "Gearbox",
      "passed": false,
      "closed": false,
      "notes": "Waiting for John to confirm",
      "created_at": "2026-08-09T23:30:26.000+10:00",
      "updated_at": "2026-08-10T10:47:10.100+10:00"
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/medicals' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create medicals

`POST /public/v1/medicals` creates a new medical with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing medicals into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/medicals
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
    employee: "",          // string, required, if a match is not found then an error is thrown
    employee_number: "",   // string, optional, if there are multiple employees with the same name you may use this field to specify your search
    medical_type: "",      // string, required, if a match is not found then an error is thrown
    medical_date: "",      // date, required, format: 'yyyy-mm-dd'
    expiry_date: "",       // date, required, format: 'yyyy-mm-dd'
    supplier: "",          // string, optional, if a suppliers name or code match is not found then an error is thrown
    passed: "",            // boolean, optional, true or false
    closed: "",            // boolean, optional, true or false
    notes: ""              // string, optional, maximum 255 characters
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/medicals' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
    "employee": "John Smith",
    "employee_number": "12345",
    "medical_type": "Standard Medical",
    "medical_date": "2026-01-01",
    "expiry_date": "2026-12-01",
    "supplier": "Gearbox",
    "passed": true,
    "closed": false,
    "notes": "Waiting for John to confirm"
}'
```
