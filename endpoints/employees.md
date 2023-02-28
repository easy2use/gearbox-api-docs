# Employees

- [Get employees](#get-employees)

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
- sub_company
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
      "sub_company": "Tims Mowing",
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
