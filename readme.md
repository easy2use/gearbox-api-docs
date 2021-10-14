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
- [Service](endpoints/service.md)


## Handling errors

API clients must expect and gracefully handle transient errors, such as rate limiting, data or server errors.

### 4xx Client Error

If a response returns a 4xx status code all changes will be rolled back and you must resolve all issues before re-sending the request.

### Rate limiting (503 Service unavailable)

You can perform up to 20 requests per 10-second period from the same IP address for the same account. If you exceed this limit, you'll get a 503 Service unavailable for subsequent requests.

### 5xx server errors

If Gearbox is having trouble, you will get a response with a 5xx status code indicating a server error. 500 (Internal Server Error), 502 (Bad Gateway) and 504 (Gateway Timeout) may be retried with exponential backoff.
