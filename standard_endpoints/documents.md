# Documents

- [Get documents](#get-documents)
- [Create documents](#create-documents)

## Get documents

`GET /public/v1/:parent/:id/documents` retrieves a [paginated list](../readme.md/#pagination) of documents associated with the `:parent`.

Note: 
- the page length for documents is 5
- the `download_url` expires 5 minutes after generation
- filtering is not available for this endpoint

### Request

```
URL: https://api.gearbox.com.au/public/v1/repair_item/1/documents
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "documents": [
    {
      "id": 1,
      "filename": "filename.png",
      "content_type": "image/png",
      "content_length": 4245524,
      "download_url": "https://download.url",
      "download_url_expires_at": "2026-11-06T11:48:07Z",
      "created_at": "2026-11-06T11:42:10Z",
      "updated_at": "2026-11-06T11:42:10Z"
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/repair_item/1/documents' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create documents

`POST /public/v1/:parent/:id/documents` creates a document attaching it to a provided `:parent` `:id`.

Allowed parents are:

- `vehicle`
- `vehicle_image`
- `service`
- `repair_item`
- `tyre_item`
- `prestart`
- `mileage`
- `medical`
- `training`
- `employee`
- `inspection`
- `contractor`
- `purchase_order`

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/:parent/:id/documents?name=image.png
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/repair_item/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```