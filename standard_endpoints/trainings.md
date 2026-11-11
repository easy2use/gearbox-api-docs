# Trainings

- [Get trainings](#get-trainings)
- [Create trainings](#create-trainings)
- [Create training documents](#create-training-documents)

Getting and creating trainings requires **Access to Training API** to be enabled for your OAuth application and the Employees module to be enabled for your account.

## Get trainings

`GET /public/v1/trainings` retrieves a [paginated list](../readme.md/#pagination) of trainings.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attribute names</summary>
<br>

- id
- closed
- expiry_date
- training_number
- employee
- employee_number
- training_date
- training_type
- provider
- certificate

`provider` only supports the `eq` operator and matches any linked provider. A comma-separated list matches any of the supplied names, e.g. `provider eq 'Gearbox, Example Training'`. Matching records still return all their providers without duplicate training records.

</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/trainings
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "trainings": [
    {
      "id": 5678,
      "training_number": 321,
      "employee_number": 12345,
      "employee": "John Smith",
      "training_type": "First Aid",
      "training_date": "2026-09-09",
      "expiry_date": "2027-09-09",
      "provider": "Gearbox",
      "certificate": "CERT-123",
      "closed": false,
      "notes": "Annual refresher",
      "created_at": "2026-09-09T09:30:26.000+10:00",
      "updated_at": "2026-09-09T10:47:10.100+10:00"
    }
  ]
}
```

###### Examples

```shell
curl --location --request GET 'https://api.gearbox.com.au/public/v1/trainings' \
--header 'Accept: application/json' \
--header "Authorization: Bearer $ACCESS_TOKEN"
```

Get open trainings for an employee:

```shell
curl --location --get 'https://api.gearbox.com.au/public/v1/trainings' \
--header 'Accept: application/json' \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data-urlencode 'filter=employee_number eq 12345 AND closed eq false'
```

## Create trainings

`POST /public/v1/trainings` creates a new training with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing trainings into Gearbox.
- The employee must already exist. Name matching ignores case and surrounding whitespace. If multiple employees have the same name, supply `employee_number`; both name and number must match when provided.
- The training type must already exist. Its label is matched without regard to case.
- The optional provider must exactly match one existing repairer name, including case. Only one provider can be supplied when creating a training.
- `closed` must be a JSON boolean and defaults to false when omitted or null.
- Training numbers are assigned by Gearbox. To attach a certificate document, use [Create training documents](#create-training-documents) after creating the training.

### Request

```
URL: https://api.gearbox.com.au/public/v1/trainings
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  training: {
    employee: "",         // string, required, full employee name, maximum 90 characters
    employee_number: 0,    // integer, optional, used to distinguish employees with the same name
    training_type: "",    // string, required, must match an existing training type label
    training_date: "",    // date, required, format: 'yyyy-mm-dd'
    expiry_date: "",      // date, optional, format: 'yyyy-mm-dd'
    provider: "",         // string, optional, must match an existing repairer name
    certificate: "",      // string, optional, maximum 50 characters
    closed: false,        // boolean, optional, true or false, defaults to false
    notes: ""             // string, optional, maximum 5,000,000 characters
  }
}
```

### Response status codes:

- 201: Created
- 400: Bad Request
- 401: Unauthorised
- 403: Forbidden
- 415: Unsupported media type
- 422: Unprocessable Entity

Validation errors return a plain-text message, for example `Training type Missing could not be found`. Missing or ambiguous employees, unknown training types or providers, and invalid field values return 422 without creating a training.

### 201 - Successful response

```JSON
{
  "id": 123
}
```

###### Example

```shell
curl --location --request POST 'https://api.gearbox.com.au/public/v1/trainings' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data-raw '{
  "training": {
    "employee": "John Smith",
    "employee_number": 12345,
    "training_type": "First Aid",
    "training_date": "2026-09-09",
    "expiry_date": "2027-09-09",
    "provider": "Gearbox",
    "certificate": "CERT-123",
    "closed": false,
    "notes": "Annual refresher"
  }
}'
```

## Create training documents

`POST /public/v1/training/:id/documents` creates a document attaching it to a provided training `:id`.

Please note:

- This endpoint requires Documents API access for the OAuth application and the Documents module to be enabled for your account.
- Use the training ID returned when creating or retrieving a training.
- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/training/123/documents?name=certificate.pdf
Method: POST
Content-Type: application/pdf
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
  "id": 456
}
```

The returned ID identifies the created document.

###### Example

```shell
curl --location --request POST 'https://api.gearbox.com.au/public/v1/training/123/documents?name=certificate.pdf' \
--header 'Content-Type: application/pdf' \
--header 'Accept: application/json' \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data-binary '@certificate.pdf'
```

Curl sets the Content-Length header from the file size.
