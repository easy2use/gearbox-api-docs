# Employees

- [Get employees](#get-employees)
- [Create employees](#create-employees)
- [Update an employee](#update-an-employee)
- [Create employee documents](#create-employee-documents)

## Get employees

`GET /public/v1/employees` retrieves a [paginated list](../readme.md/#pagination) of employees.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>
  
- id
- first_name
- last_name
- start_date
- employee_number
- job_title
- left_date
- phone
- mobile
- address1
- address2
- city
- state
- post_code
- licence
- licence_category
- subcontractor
- sub_abn
- emp_notes
- terminated
- email
- next_of_kin
- next_of_kin_number
- spare1
- spare2
- spare3
- spare4
- created_at
- updated_at
- contractors.name
- groups.name
- sub_groups.name
- types.name
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/employees
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "employees": [
    {
      "id": "73483",
      "first_name": "John",
      "last_name": "Smith",
      "start_date": "2021-12-23",
      "employee_number": "1063",
      "job_title": "Director",
      "left_date": "2022-09-14",
      "phone": "0400000000",
      "mobile": "0411111111",
      "address1": "123 Example Street",
      "address2": "Example Avenue",
      "city": "Sydney",
      "state": "NSW",
      "post_code": "2000",
      "licence": "1234567890",
      "licence_category": "LR",
      "subcontractor": false,
      "sub_abn": "ABC123",
      "contractor": "Tims Mowing",
      "notes": "test notes",
      "terminated": "true",
      "email": "john.smith@email.com.au",
      "next_of_kin": "Jane Smith",
      "next_of_kin_number": "0422222222",
      "spare1": "123ABC",
      "spare2": "123ABC",
      "spare3": "123ABC",
      "spare4": "123ABC",
      "created_at": "2022-11-23T09:12:13.956+11:00",
      "updated_at": "2022-11-23T09:12:13.956+11:00",
      "groups": [
        {
          "name": "NSW"
        }
      ],
      "sub_groups": [
        {
          "name": "Sydney"
        }
      ],
      "types":  [
        {
          "name": "Fridge"
        }
      ]
    }
  ]
}
```

###### Examples

```
curl --location --request GET "https://api.gearbox.com.au/public/v1/employees" \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create employees

`POST /public/v1/employees` creates a new employee with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing employees into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/employees
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  first_name: "",         // string, optional, maximum length 45 characters
  last_name: "",          // string, optional, maximum length 45 characters
  start_date: "",         // date, optional, maximum length 50 characters
  left_date: "",          // date, optional, maximum length 50 characters
  employee_number: "",    // string, optional, must be unique
  job_title: "",          // string, optional, maximum length 45 characters
  phone: "",              // string, optional, maximum length is 20 characters
  mobile: "",             // string, optional, maximum length is 20 characters, must be unique
  email: "",              // string, optional, maximum length is 50 characters
  address1: "",           // string, optional, maximum length 45 characters
  address2: "",           // string, optional, maximum length 45 characters
  city: "",               // string, optional, maximum length 45 characters
  state: "",              // string, optional, maximum length 5 characters
  post_code: "",          // string, optional, maximum length 5 characters
  notes: "",              // string, optional
  terminated: "",         // boolean, optional, defaults to false
  next_of_kin: "",        // string, optional
  next_of_kin_number: "", // string, optional
  licence: "",            // string, optional, maximum length 15 characters
  licence_category: "",   // string, optional, maximum length 50 characters
  subcontractor: "",      // string, optional
  sub_abn: "",            // string, optional, maximum length 15 characters
  spare1: "",             // string, optional, maximum length 255 characters
  spare2: "",             // string, optional, maximum length 255 characters
  spare3: "",             // string, optional, maximum length 255 characters
  spare4: "",             // string, optional, maximum length 255 characters
  contractor: "",         // string, optional, must match existing Contractor
  vehicle_group: "",      // string, optional, must match existing Vehicle Group
  vehicle_sub_group: "",  // string, optional, must match existing Vehicle Sub Group
  vehicle_type: "",       // string, optional, must match existing Vehicle Type
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
  "employee": "John Smith"
}
```

###### Example

```
curl --location --request POST http://api.gearbox.com.au/public/v1/employees \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "first_name": "John",
      "last_name": "Smith",
      "start_date": "2022-05-03",
      "left_date": "2023-11-17",
      "employee_number": "123",
      "job_title": "Manager",
      "phone": "040101010101",
      "mobile": "040202020202",
      "address1": "123 Fake street",
      "address2": "Faketown",
      "city": "Sydney",
      "state": "NSW",
      "post_code": "4234",
      "licence": "test licence",
      "licence_category": "test category",
      "subcontractor": true,
      "sub_abn": "123ABC",
      "notes": "test notes",
      "email": "test_employee@email.com",
      "next_of_kin": "test kin",
      "next_of_kin_number": "040303030303",
      "spare1": "example spare 1",
      "spare2": "example spare 2",
      "spare3": "example spare 3",
      "spare4": "example spare 4",
      "contractor": "Tims mowing",
      "vehicle_group": "Sydney",
      "vehicle_sub_group": "Penrith",
      "vehicle_type": "Forklift"
    }' 
```

## Update an employee

`PATCH /public/v1/employees/:id` updates the employee with the provided `:id`.

Please note:

- Content-Type header `Content-Type: application/json` is required when PATCHing employees into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/employees/1
Method: PATCH
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  first_name: "",         // string, optional, maximum length 45 characters
  last_name: "",          // string, optional, maximum length 45 characters
  start_date: "",         // date, optional, maximum length 50 characters
  left_date: "",          // date, optional, maximum length 50 characters
  employee_number: "",    // string, optional, must be unique
  job_title: "",          // string, optional, maximum length 45 characters
  phone: "",              // string, optional, maximum length is 20 characters
  mobile: "",             // string, optional, maximum length is 20 characters
  email: "",              // string, optional, maximum length is 50 characters
  address1: "",           // string, optional, maximum length 45 characters
  address2: "",           // string, optional, maximum length 45 characters
  city: "",               // string, optional, maximum length 45 characters
  state: "",              // string, optional, maximum length 5 characters
  post_code: "",          // string, optional, maximum length 5 characters
  notes: "",              // string, optional
  terminated: "",         // boolean, optional, defaults to false
  next_of_kin: "",        // string, optional
  next_of_kin_number: "", // string, optional
  licence: "",            // string, optional, maximum length 15 characters
  licence_category: "",   // string, optional, maximum length 50 characters
  subcontractor: "",      // string, optional
  sub_abn: "",            // string, optional, maximum length 15 characters
  spare1: "",             // string, optional, maximum length 255 characters
  spare2: "",             // string, optional, maximum length 255 characters
  spare3: "",             // string, optional, maximum length 255 characters
  spare4: "",             // string, optional, maximum length 255 characters
  contractor: "",         // string, optional, must match existing Contractor
  vehicle_group: "",      // string, optional, must match existing Vehicle Group
  vehicle_sub_group: "",  // string, optional, must match existing Vehicle Sub Group
  vehicle_type: "",       // string, optional, must match existing Vehicle Type
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
  "employee": "John Smith"
}
```

###### Example

```
curl --location --request PATCH http://api.gearbox.com.au/public/v1/employees/6245 \
--header "Content-Type: application/json" \
--header "Authorization: Bearer $ACCESS_TOKEN" \
--data '{
      "first_name": "John",
      "last_name": "Smith",
      "start_date": "2022-05-03",
      "left_date": "2023-11-17",
      "employee_number": "123",
      "job_title": "Manager",
      "phone": "040101010101",
      "mobile": "040202020202",
      "address1": "123 Fake street",
      "address2": "Faketown",
      "city": "Sydney",
      "state": "NSW",
      "post_code": "4234",
      "licence": "test licence",
      "licence_category": "test category",
      "subcontractor": true,
      "sub_abn": "123ABC",
      "contractor": "Tims mowing",
      "notes": "test notes",
      "email": "test_employee@email.com",
      "next_of_kin": "test kin",
      "next_of_kin_number": "040303030303",
      "spare1": "example spare 1",
      "spare2": "example spare 2",
      "spare3": "example spare 3",
      "spare4": "example spare 4",
    }' 
```

## Create employee documents

`POST /public/v1/employee/:id/documents` creates a document attaching it to a provided employee `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/employee/1/documents?name=image.png
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/employee/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```
