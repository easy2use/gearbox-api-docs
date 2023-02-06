- [Get vehicles](#get-vehicles)

## Get vehicles

`GET /public/v1/vehicles` retrieves a [paginated list](../readme.md/#pagination) of vehicles.

Please note:

- Optional [filtering](../readme.md/#filtering) is available for this endpoint if needed.

<details>
<summary>Filter attributes names</summary>
<br>
  
- id
- fleet_number
- service_group
- registration
- registration_due_date
- unregistered
- model
- vin
- build_date
- spare1
- spare2
- spare3
- spare4
- spare5
- spare6
- spare7
- spare8
- spare9
- spare10
- spare11
- spare12
- spare13
- spare14
- spare15
- spare16
- spare17
- spare18
- spare19
- spare20
- engine_number
- engine_make
- engine_model
- engine_capacity
- gearbox
- sold
- service_group
- group
- subgroup
- type
- make
- configuration
- contractor
</details>

### Request

```
URL: https://api.gearbox.com.au/public/v1/vehicles
Method: GET
Authorization: Bearer $ACCESS_TOKEN
```

### 200 - Successful response

```JSON
{
  "vehicles": [
    {
      "id": 774935874, 
      "fleet_number": "FLT123", 
      "registration": "145ATG", 
      "registration_due_date": "2024-02-06", 
      "unregistered": false, 
      "model": "Model A", 
      "vin": 12345678901, 
      "build_date": "2019", 
      "spare1": "ABC123", 
      "spare2": nil, 
      "spare3": nil, 
      "spare4": nil, 
      "spare5": nil, 
      "spare6": nil, 
      "spare7": nil, 
      "spare8": nil, 
      "spare9": nil, 
      "spare10": nil, 
      "spare11": nil, 
      "spare12": nil, 
      "spare13": nil, 
      "spare14": nil, 
      "spare15": nil, 
      "spare16": nil, 
      "spare17": nil, 
      "spare18": nil, 
      "spare19": nil, 
      "spare20": nil, 
      "powered": true, 
      "engine_number": "ABC123", 
      "engine_make": "Volvo", 
      "engine_model": "D13", 
      "engine_capacity": "5L", 
      "gearbox": "123ABC", 
      "sold": false, 
      "service_group": "Cars", 
      "group": "VIC", 
      "subgroup": "Melbourne", 
      "type": "Car", 
      "make": "Honda", 
      "configuration": "'A' Skel", 
      "contractor": "Test Contractor"
    }
  ]
}
```

###### Example

```
curl --location --request GET 'https://api.gearbox.com.au/public/v1/vehicles' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $ACCESS_TOKEN'
```