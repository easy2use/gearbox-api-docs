# Purchase Orders

- [Get purchase orders](#get-purchase-orders)
- [Create purchase orders](#create-purchase-orders)
- [Create purchase order documents](#create-purchase-order-documents)

## Get purchase orders

`GET /public/v1/purchase_orders` retrieves a [paginated list](../readme.md/#pagination) of purchase orders.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>
* id
* purchase_order_number
* purchase_order_date
* created_by
* approved
* approved_by
* approved_date
* reference_number
* supplier
* invoice_number
* invoice_date
* instructions
* closed
* cost
* tax
* bulk_receive_date
* fleet_number
* registration
* group
* service_number
* repair_number
* tyre_number
* other_number
* general_ledger_code
* actual_cost
* actual_tax
* tax_rate
* purchase_order_items.parts.part_number
* purchase_order_items.parts.part_description
* purchase_order_items.quantity
* purchase_order_items.each
* purchase_order_items.total
* purchase_order_items.received_quantity
* purchase_order_items.invoice_date
* purchase_order_items.invoice_number
* purchase_order_items.tax
* purchase_order_items.site
* purchase_order_items.general_ledger_code
* purchase_order_items.tax_percent
* purchase_order_items.tax_rate
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/purchase_orders
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "purchase_orders": [
    {
      "id": 123,
      "purchase_order_number": 241,
      "purchase_order_date": "2021-04-07",
      "created_by": "John Smith",
      "approved": true,
      "approved_by": "Jane Doe",
      "approved_date": "2021-04-07",
      "reference_number": "ABC-123",
      "invoice_number": "INV-5546",
      "invoice_date": "2021-04-07",
      "instructions": "Please deliver to back door",
      "closed": false,
      "cost": 165.0,
      "tax": 15.0,
      "bulk_receive_date": "2021-04-07",
      "fleet_number": "PM09",
      "registration": "NV00FV",
      "group": "Sydney",
      "service_number": 489,
      "repair_number": 987,
      "tyre_number": 123,
      "other_number": 8844,
      "supplier": "Trucks R Us",
      "general_ledger_code": "ABC123 - PO Code",
      "actual_cost": 165.0,
      "actual_tax": 15.0,
      "tax_rate": "Inclusive",
      "purchase_order_items": [
        {
            "tax": 15.0,
            "each": 165.0,
            "site": "Sydney",
            "total": 165.0,
            "quantity": 1.0,
            "part_number": "Oil Filter",
            "tax_percent": 10.0,
            "invoice_date": "2021-04-07",
            "invoice_number": "INV-5546",
            "part_description": "Kenworth Oil Filter",
            "received_quantity": 1,
            "general_ledger_code": "ABC123 - PO Code",
            "tax_rate": "Inclusive"
        }
      ]
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/purchase_orders' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```

## Create purchase orders

`POST /public/v1/purchase_orders` creates a new purchase order with the provided information.

Please note:

- Content-Type header `Content-Type: application/json` is required when POSTing purchase orders into Gearbox.
- Snake case object keys are required.

### Request

```
URL: https://api.gearbox.com.au/public/v1/purchase_orders
Method: POST
Content-Type: application/json
Authorization: Bearer $ACCESS_TOKEN

{
  purchase_order_date: "", // date, required, format: 'yyyy-mm-dd'
  supplier: "",            // string, required, must match existing Repairer in the system
  created_by: "",          // string, optional, must match existing User in the system
  approved: "",            // boolean, optional, defaults to false
  approved_by: "",         // string, optional, must match existing User in the system. Required if approved is true.
  approved_date: "",       // date, optional, format: 'yyyy-mm-dd'. Required if approved is true.
  reference_number: "",    // string, optional, maximum length of 20 characters
  invoice_number: "",      // string, optional, maximum length of 20 characters
  invoice_date: "",        // date, optional, format: 'yyyy-mm-dd'
  instructions: "",        // text, optional
  closed: "",              // boolean, optional, defaults to false
  cost: 0.0,               // float, optional, format: 100.00 - this will be calculated based on totals from purchase order items
  tax: 0.0,                // float, optional, format: 100.00 - this will be calculated based on totals from purchase order items
  bulk_receive_date: "",   // date, optional, format: 'yyyy-mm-dd'
  fleet_number: "",        // string, optional, must match existing fleet number in the system
  registration: "",        // string, optional, must match existing registration in the system
  group: "",               // string, optional, must match existing Vehicle Group in the system
  service_number: ,        // integer, optional, must match existing Service number in the system
  repair_number: ,         // integer, optional, must match existing Repair number in the system
  tyre_number: ,           // integer, optional, must match existing Tyre number in the system
  other_number: ,          // integer, optional, must match existing Other number in the system
  general_ledger_code: "", // string, optional, if a general ledger code match is not found than an error is thrown
  actual_cost: 0.0,        // float, optional, format: 100.00 - this will be calculated based on totals from purchase order items
  actual_tax: 0.0,         // float, optional, format: 100.00 - this will be calculated based on totals from purchase order items
  tax_rate: "",            // string, optional, must be either Inclusive, Exclusive or Not Applicable. Automatically calculated if not present.
  purchase_order_items: [  // array, required, size must be less than or equal to 10
    {
        tax: 0.0,                // float, optional, format: 100.00
        each: 0.0,               // float, optional, format: 100.00
        site: "",                // string, optional, if a site match is not found than an error is thrown
        total: 0.0,              // float, optional, format: 100.00
        quantity: 0.0,           // float, optional, format: 100.00
        part_number: "",         // string, optional, if a match is not found a new Part will be created
        tax_percent: 0.0,        // float, optional, format: 100.00
        invoice_date: "",        // date, optional, format: 'yyyy-mm-dd'
        invoice_number: "",      // string, optional, maximum length of 20 characters
        part_description: "",    // string, optional
        received_quantity: 0,    // float, optional, format: 100.00
        general_ledger_code: "", // string, optional, if a general ledger code match is not found than an error is thrown
        tax_rate: ""             // string, optional, must be either Inclusive, Exclusive or Not Applicable. Automatically calculated if not present.
    }
  ]
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
  "purchase_order_item_ids": [123, 124]
}
```

###### Example

```
curl --location --request POST 'https://api.gearbox.com.au/public/v1/purchase_orders' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-raw '{
  "purchase_order_date": "2019-01-01",
  "created_by": "John Smith",
  "approved": true,
  "approved_by": "Jane Doe",
  "approved_date": "2019-01-01",
  "reference_number": "123456",
  "supplier": "Trucks R Us",
  "invoice_number": "123456",
  "invoice_date": "2019-01-01",
  "instructions": "Please deliver to the back door",
  "closed": true,
  "cost": 1000,
  "tax": 100,
  "bulk_receive_date": "2019-01-01",
  "fleet_number": "PM001",
  "registration": "ABC123",
  "group": "Sydney",
  "service_number": "",
  "repair_number": "",
  "tyre_number": "",
  "other_number": "",
  "general_ledger_code": "5000-10",
  "actual_cost": 1000,
  "actual_tax": 100,
  "purchase_order_items": [
    {
      "part_number": "FAD-4455",
      "part_description": "Oil Filter",
      "quantity": 1,
      "each": 50,
      "total": 50,
      "received_quantity": 1,
      "invoice_date": "2019-01-01",
      "invoice": "123456",
      "tax": 10,
      "site": "Sydney",
      "general_ledger_code": "5000-10",
      "tax_percent": 10,
      "tax_rate": "Inclusive"
    }
  ]
}'
```

## Create purchase order documents

`POST /public/v1/purchase_orders/:id/documents` creates a document attaching it to a provided purchase order `:id`.

Please note:

- The request body should be the file's raw binary data.
- Content-Type and Content-Length of the file must be provided in the header.
- The name of the file must be provided as a URI query parameter.
- The file attached must be less than or equal to 5 megabytes.

### Request

```
URL: https://api.gearbox.com.au/public/v1/purchase_orders/1/documents?name=image.png
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
curl --location --request POST 'https://api.gearbox.com.au/public/v1/purchase_orders/1/documents?name=image.png' \
--header 'Content-Type: image/png' \
--header 'Content-Length: 123' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN' \
--data-binary '@image.png'
```
