# Contractors

- [Create contractors](#create-contractors)
- [Update a contractor](#update-a-contractor)
- [Create contractor documents](#create-contractor-documents)

## Create contractors

`POST /public/v1/contractors` creates a new contractor with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing contractors into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/contractors
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  company_name: "",         // string, required, maximum length 100 characters
  terminated: "",           // boolean, optional, defaults to false
  phone: "",                // string, optional, maximum length 20 characters
  mobile: "",               // string, optional, maximum length 20 characters
  address1: "",             // string, optional, maximum length 100 characters
  address2: "",             // string, optional, maximum length 100 characters
  city: "",                 // string, optional, maximum length 100 characters
  state: "",                // string, optional, maximum length 20 characters
  postcode: "",             // string, optional, maximum length 10 characters
  abn: "",                  // string, optional, maximum length 100 characters
  email: "",                // string, optional, maximum length 50 characters
  fax: "",                  // string, optional, maximum length 20 characters
  contact: "",              // string, optional, maximum length 50 characters
  notes: "",                // string, optional
  code: "",                 // string, optional, maximum length 20 characters
  position: "",             // string, optional
  vehicle_group: "",        // string, optional, must match existing Vehicle Group
  vehicle_sub_group: ""     // string, optional, must match existing Vehicle Sub Group
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
curl --location --request POST http://api.gearbox.com.au/public/v1/contractors \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "company_name": "Contractors R Us",
      "terminated": false,
      "phone": "123456",
      "mobile": "0411111111",
      "address1": "123 Fake Street"
      "city": "Adelaide",
      "state": "SA",
      "postcode": "5005",
      "position": "CEO",
      "abn": "1938431907",
      "email": "contractors@contractorsrus.com.au",
      "vehicle_group": "Adelaide",
      "vehicle_sub_group": "Trucks"
    }' 
```

## Update a contractor

`PATCH /public/v1/contractors/:id` updates the contractor with the provided `:id`. Alternatively, you may instead provide a contractor `:code` in the format of: `PATCH /public/v1/contractors/:code`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing contractors into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/contractors/
Method: PATCH
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  company_name: "",     // string, required, maximum length 100 characters
  terminated: "",       // boolean, optional, defaults to false
  phone: "",            // string, optional, maximum length 20 characters
  mobile: "",           // string, optional, maximum length 20 characters
  address1: "",         // string, optional, maximum length 100 characters
  address2: "",         // string, optional, maximum length 100 characters
  city: "",             // string, optional, maximum length 100 characters
  state: "",            // string, optional, maximum length 20 characters
  postcode: "",         // string, optional, maximum length 10 characters
  abn: "",              // string, optional, maximum length 100 characters
  email: "",            // string, optional, maximum length 50 characters
  fax: "",              // string, optional, maximum length 20 characters
  contact: "",          // string, optional, maximum length 50 characters
  notes: "",            // string, optional
  code: "",             // string, optional, maximum length 20 characters
  position: "",         // string, optional
  vehicle_group: ""     // string, optional, must match existing Vehicle Group
  vehicle_sub_group: "" // string, optional, must match existing Vehicle Sub Group
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
curl --location --request PATCH http://api.gearbox.com.au/public/v1/contractor/123 \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "company_name": "Example Contractor",
      "terminated": true,
      "phone": "0411111111",
      "mobile": "0411111111",
      "address1": "Example street,
      "address2": "Example address",
      "city": "Sydney",
      "state": "NSW",
      "postcode": "1234",
      "abn": "1234ABC",
      "email": "example@email.com",
      "fax": "1234ABC",
      "contact": "Example contact",
      "notes": "Example notes",
      "code": "ABC123",
      "position": "Manager",
      "vehicle_group": "QLD",
      "vehicle_sub_group": "Brisbane"
    }' 
```

## Create contractor documents

`POST /public/v1/contractor/:id/documents` creates a document attaching it to a provided contractor `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/contractor/1/documents?name=image.png
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/contractor/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```
