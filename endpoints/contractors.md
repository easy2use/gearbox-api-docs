# Contractors

- [Create contractors](#create-contractors)

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
  company_name: "", // string, required, maximum length 100 characters
  terminated: "", // boolean, optional, defaults to false
  phone: "", // string, optional, maximum length 20 characters
  mobile: "", // string, optional, maximum length 20 characters
  address1: "", // string, optional, maximum length 100 characters
  address2: "", // string, optional, maximum length 100 characters
  city: "", // string, optional, maximum length 100 characters
  state: "", // string, optional, maximum length 20 characters
  postcode: "", // string, optional, maximum length 10 characters
  abn: "", // string, optional, maximum length 100 characters
  email: "", // string, optional, maximum length 50 characters
  fax: "", // string, optional, maximum length 20 characters
  contact: "", // string, optional, maximum length 50 characters
  notes: "", // string, optional
  code: "", // string, optional, maximum length 20 characters
  position: "", // string, optional
  group: "" // string, optional, must match existing Vehicle Group
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
curl --location --request POST http://api.lvh.me:3000/public/v1/contractors \
--header "Content-Type: application/json" \
--header "Authorization: Bearer 78PJHNcLMRBi_jpVDFaPt-iJkbEwX4KTW3ezgEDrDeM" \
--data '{
      "company_name":"Contractors R Us",
      "terminated":false,
      "phone":"123456",
      "mobile":"0411111111",
      "position":"CEO",
      "abn":"1938431907",
      "vehicle_group":"Adelaide"
    }' 
```
