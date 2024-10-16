# Vehicle Notes

- [Get notes](#get-notes)
- [Create notes](#create-notes)


## Get notes

`GET /public/v1/vehicles/:vehicle_id/notes` retrieves a [paginated list](../readme.md/#pagination) of notes.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>
  
- id
- vehicle_id
- fleet_number
- registration
- user
- comment
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicles/:vehicle_id/notes
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "notes": [
    {
      "id": 774935874, 
      "vehicle_id": 123456789,
      "fleet_number": "FLT123",
      "registration": "145ATG", 
      "user": "Jane Doe",
      "comment": "Vehicle note"
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/vehicles/123/notes' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create notes

`POST /public/v1/vehicles/:vehicle_id/notes` creates a new vehicle note with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing vehicles into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicles/:vehicle_id/notes
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  user: "",                   // string, required
  comment: "",                // string, required, maximum length 255 character
}
```

### Response status codes:

- 201: Created
- 400: Bad Request
- 401: Unauthorised
- 403: Forbidden
- 413: Request Entity Too Large
- 422: Unprocessable Entity

### 201 - Successful response

```JSON
{
  "id": 774935874
}
```

###### Example

```
curl --location --request POST http://api.gearbox.com.au/public/v1/vehicles/123/notes \
--header "Content-Type: application/json" \
--header "Authorization: Bearer 333" \
--data '{
    "note": {
      "user": "Jane Doe",
      "comment": "Vehicle note"
    }
  }'
```
