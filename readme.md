# Gearbox API

Gearbox is REST-style API that uses JSON for serialization and OAuth 2.0 for authorisation. To get started you must first contact Gearbox Support to request your `client_id` and `client_secret` to successfully authorise your application.

- [Authorisation](#Authorisation)
- [Making a request](#Making-a-request)
- [Handling Errors](#Handling-errors)

## Authorisation

As mentioned above, to authenticate you must use OAuth 2.0. A token can be retrieved through one of the following methods:

- [Authorisation Code Grant](authorisation/authorisation_code_grant.md)
- [Client Credentials Grant](authorisation/client_credentials_grant.md)

## Making a request

All URLs start with https://api.gearbox.com.au/public/. URLs are HTTPS only.

To make a request for all endpoints you must include the OAuth 2.0 access token in your request header:

```shell
curl -H "Authorization: Bearer $ACCESS_TOKEN"
```

### Endpoints

- [Prestarts](endpoints/prestarts.md)
- [Fault Reports](endpoints/fault_reports.md)
- [Services](endpoints/services.md)
- [Tyres](endpoints/tyres.md)
- [Repairs](endpoints/repairs.md)
- [Odometers](endpoints/odometers.md)
- [Inspections](endpoints/inspections.md)
- [Contractors](endpoints/contractors.md)
- [Employees](endpoints/employees.md)

## Pagination

The API follows the [RFC5988 convention](https://www.rfc-editor.org/rfc/rfc5988) of using the `Link` header to provide URLs for the next page. Follow this convention to retrieve the next page of data.

Here's an example response header from requesting the second page of services:

`Link: <https://api.gearbox.com.au/public/v1/services?page=2>; rel="next"`

If the `Link` header is blank then that is the last page of results. The `X-Total-Count` header is also provided which displays the total number of resources in the collection you are fetching.

## Filtering

The `filter` query string parameter allows clients to filter a collection of resources in the request URL. The expression language used supports references to attribute names, filter operators, and literal values e.g. strings (enclosed in double quotes), numbers, dates, and boolean values (true or false). Each expression MUST contain an attribute name followed by a filter operator and literal value. Both attribute name and operators are case-sensitive.

### Filter operations

| Operator             | Description           | Example                                          |
|----------------------|-----------------------|--------------------------------------------------|
| Comparison Operators |                       |                                                  |
| eq                   | Equal                 | fleet_number eq 'PM05'                           |
| ne                   | Not equal             | fleet_number ne 'PM05'                           |
| lt                   | Less than             | service_number lt 25                             |
| le                   | Less than or equal    | service_number le 25                             |
| gt                   | Greater than          | service_number gt 30                             |
| ge                   | Greater than or equal | service_number gt 30                             |
| Logical Operators    |                       |                                                  |
| AND                  | Logical and           | fleet_number eq 'PM05' AND service_number gt 30  |
| OR                   | Logical or            | fleet_number eq 'PM05' OR fleet_number eq 'PM04' |

Example: all services assigned to vehicle with fleet number PM05

`GET http://api.gearbox.com.au/public/v1/services?filter=fleet_number eq 'PM05'`

Example: open services with date open greater than 01/01/2022

`GET http://api.gearbox.com.au/public/v1/services?filter=closed ne true AND date_open gt 2022-01-01`

Example: closed services with date open greater than 01/01/2022 assigned to vehicle with fleet number PM01

`GET http://api.gearbox.com.au/public/v1/services?filter=closed eq true AND date_open gt 2022-01-01 AND fleet_number eq 'PM01'`

Example: closed A services assigned to vehicle with fleet number PM01

`GET http://api.gearbox.com.au/public/v1/services?filter=service_type eq 'A' AND fleet_number eq 'PM01' AND closed eq true`

**Please note**: Not all endpoints support filtering. Please check the endpoints documentation before using this query string.

## Handling errors

API clients must expect and gracefully handle transient errors, such as rate limiting, data or server errors.

### 4xx Client Error

If a response returns a 4xx status code all changes will be rolled back and you must resolve all issues before re-sending the request.

### Rate limiting (429 Too Many Requests)

You can perform up to 20 requests per 15-second period from the same IP address for the same account.

If you exceed this limit you will receive a 429 response with a `Retry-After` header, this is how many seconds you must wait before making another request.

### 5xx server errors

If Gearbox is having trouble, you will get a response with a 5xx status code indicating a server error. 500 (Internal Server Error), 502 (Bad Gateway) and 504 (Gateway Timeout) may be retried with exponential backoff.
